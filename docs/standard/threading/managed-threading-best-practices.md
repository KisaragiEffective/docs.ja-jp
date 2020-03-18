---
title: マネージド スレッド処理のベスト プラクティス
ms.date: 10/15/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- threading [.NET Framework], design guidelines
- threading [.NET Framework], best practices
- managed threading
ms.assetid: e51988e7-7f4b-4646-a06d-1416cee8d557
ms.openlocfilehash: a76cc40f308ac2f636a650cd4a17da0e94e23a34
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160262"
---
# <a name="managed-threading-best-practices"></a>マネージド スレッド処理のベスト プラクティス
マルチスレッドには慎重なプログラミングが必要です。 ほとんどのタスクでは、スレッド プールのスレッドを使って実行の要求をキューに置くことによって、処理の複雑さを軽減できます。 このトピックでは、マルチ スレッド動作の調整や、ブロックするスレッドの処理など、より難しい状況について説明します。  
  
> [!NOTE]
> .NET Framework 4 以降では、マルチスレッド プログラミングの複雑さとリスクを軽減する API が Task Parallel Library および PLINQ に用意されています。 詳細については、[.NET の並列プログラミング](../../../docs/standard/parallel-programming/index.md)に関するページをご覧ください。  
  
## <a name="deadlocks-and-race-conditions"></a>デッドロックと競合状態  
 マルチスレッドはスループットと応答速度の問題を解決しますが、その一方で、デッドロックと競合状態という新たな問題を発生させます。  
  
### <a name="deadlocks"></a>デッドロック  
 デッドロックは、2 つのスレッドのうちの一方が、もう一方によって既にロックされているリソースをロックしようとすると発生します。 こうなると、どちらのスレッドも続行できなくなります。  
  
 マネージド スレッド処理クラスの多くのメソッドには、ロックアウトを検出するためのタイムアウト機能が用意されています。 たとえば、`lockObject` というオブジェクトへのロックの取得を試みるコードを次に示します。 ロックが 300 ミリ秒の間に得られない場合は、<xref:System.Threading.Monitor.TryEnter%2A?displayProperty=nameWithType> は `false` を返します。  
  
```vb  
If Monitor.TryEnter(lockObject, 300) Then  
    Try  
        ' Place code protected by the Monitor here.  
    Finally  
        Monitor.Exit(lockObject)  
    End Try  
Else  
    ' Code to execute if the attempt times out.  
End If  
```  
  
```csharp  
if (Monitor.TryEnter(lockObject, 300)) {  
    try {  
        // Place code protected by the Monitor here.  
    }  
    finally {  
        Monitor.Exit(lockObject);  
    }  
}  
else {  
    // Code to execute if the attempt times out.  
}  
```  
  
### <a name="race-conditions"></a>競合状態  
 競合状態は、2 つ以上のスレッドのうちのどれが特定のコード ブロックに最初に到達するかによって、プログラムの結果が変わってしまうバグのことです。 プログラムを何回か実行すると、異なる結果が得られ、実行の結果は予測できません。  
  
 競合状態の簡単な例として、フィールドのインクリメントがあります。 クラスにプライベートの **static** フィールド (Visual Basic では **Shared**) があり、このフィールドは、`objCt++;` (C# の場合) または `objCt += 1` (Visual Basic の場合) のようなコードを使用して、クラスのインスタンスが生成されるたびにインクリメントされるものとします。 この演算によって、`objCt` からレジスタへの値の読み込み、値のインクリメント、および `objCt` への値の格納が行われます。  
  
 マルチスレッドされたアプリケーションでは、値の読み込みとインクリメントを行ったスレッドが、この 3 つの処理を実行する別のスレッドに先を越される可能性があります。最初のスレッドは、実行を再開して値を格納するとき、待機中に値が変更されたかどうかを考慮せずに、`objCt` の値を上書きしてしまいます。  
  
 このような特定の競合状態は、<xref:System.Threading.Interlocked> など、<xref:System.Threading.Interlocked.Increment%2A?displayProperty=nameWithType> クラスのメソッドを使用することによって容易に回避できます。 複数のスレッド間でデータを同期する他の手法については、「[マルチスレッド処理のためのデータの同期](../../../docs/standard/threading/synchronizing-data-for-multithreading.md)」を参照してください。  
  
 競合状態は、複数のスレッドの動作を同期するときにも発生します。 コードを記述するときには、そのコードの行 (または行を構成する各マシン命令) を実行するスレッドが別のスレッドに追い越された場合に何が起きるのかを考慮しておく必要があります。  
  
## <a name="static-members-and-static-constructors"></a>静的メンバーと静的コンストラクター  
 クラスは、そのクラス コンストラクター (C# では `static` コンストラクター、Visual Basic では `Shared Sub New`) の実行が完了するまで初期化されません。 初期化されていない型のコードの実行を防止するため、共通言語ランタイムは、クラス コンストラクターの実行が完了するまで、他のスレッドからのそのクラスの `static` メンバー (Visual Basic では `Shared` メンバー) 呼び出しをすべてブロックします。  
  
 たとえば、クラス コンストラクターが新しいスレッドを起動し、そのスレッドのプロシージャがクラスの `static` メンバーを呼び出した場合、その新しいスレッドは、クラス コンストラクターが完了するまでブロックされます。  
  
 このことは、`static` コンストラクターを持つことができるすべての型に当てはまります。  

## <a name="number-of-processors"></a>プロセッサの数

システムで利用できるプロセッサの数 (複数か 1 つだけか) がマルチスレッド アーキテクチャに影響を与えることがあります。 詳細については、「[プロセッサの数](https://docs.microsoft.com/previous-versions/dotnet/netframework-1.1/1c9txz50(v%3dvs.71)#number-of-processors)」を参照してください。

実行時に利用できるプロセッサの数を判断するには <xref:System.Environment.ProcessorCount?displayProperty=nameWithType> プロパティを使用します。
  
## <a name="general-recommendations"></a>一般的な推奨事項  
 マルチスレッドを使用するときは、以下のガイドラインを考慮してください。  
  
- 他のスレッドを終了させるために <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> を使用することは避けてください。 他のスレッド **Abort** を呼び出すことは、そのスレッドの処理がどこまで到達しているかを把握せずに例外をスローするのと同じことになります。  
  
- <xref:System.Threading.Thread.Suspend%2A?displayProperty=nameWithType> と <xref:System.Threading.Thread.Resume%2A?displayProperty=nameWithType> を使用して複数のスレッドの動作を同期することは避けてください。 <xref:System.Threading.Mutex>、<xref:System.Threading.ManualResetEvent>、<xref:System.Threading.AutoResetEvent>、および <xref:System.Threading.Monitor> を使用してください。  
  
- メイン プログラムから、イベントなどを使用して、ワーカー スレッドの実行を制御しないでください。 ワーカー スレッドの側で、作業ができるようになるまでの待機、作業の実行、および実行終了後のプログラムへの通知を行うように、プログラムをデザインします。 ワーカー スレッドによるブロックを行わない場合は、スレッド プールのスレッドを使用することを考慮します。 ワーカー スレッドがブロックを行う場合は、<xref:System.Threading.Monitor.PulseAll%2A?displayProperty=nameWithType> が役立ちます。  
  
- ロック オブジェクトとして型を使用しないでください。 つまり、C# の `lock(typeof(X))`、Visual Basic の `SyncLock(GetType(X))` などのコードを使用すること、または <xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType> を <xref:System.Type> オブジェクトと組み合わせて使用することを避けます。 <xref:System.Type?displayProperty=nameWithType> のインスタンスは、1 つの型につきアプリケーション ドメインごとに 1 つのみです。 ロックする型がパブリックの場合、自分以外のコードでもその型をロックできるため、デッドロックになる可能性があります。 詳細については、「[信頼性に関するベスト プラクティス](../../../docs/framework/performance/reliability-best-practices.md)」を参照してください。  
  
- インスタンスをロックする場合 (たとえば、C# の `lock(this)`、Visual Basic の `SyncLock(Me)`) には注意してください。 アプリケーション内の、その型以外の他のコードがオブジェクトをロックすると、デッドロックが発生する場合があります。  
  
- モニター状態に入った (ロックを取得した) スレッドは、モニター状態である間に例外が発生した場合でも、必ずモニター状態から出すようにします。 C# の [lock](../../csharp/language-reference/keywords/lock-statement.md) ステートメントと Visual Basic の [SyncLock](../../visual-basic/language-reference/statements/synclock-statement.md) ステートメントは、**finally** ブロックを使用して <xref:System.Threading.Monitor.Exit%2A?displayProperty=nameWithType> が呼び出されるようにすることで、この動作を自動的に提供します。 **Exit** を確実に呼び出すことができない場合は、**Mutex** を使用するようにデザインを変更することを検討してください。 ミューテックスは、現在それを保持しているスレッドが終了すると、自動的に解放されます。  
  
- マルチ スレッドは異なるリソースを必要とするタスクに使用し、1 つのリソースに複数のスレッドを割り当てることがないように注意します。 たとえば、I/O を含む作業であれば、その作業専用のスレッドを用意すると有益です。I/O 操作の間、このスレッドはブロックを行いますが、他のスレッドは実行できるからです。 ユーザー入力も、専用のスレッドが役に立つリソースの 1 つです。 シングル プロセッサのコンピューターでは、計算中心のタスクがユーザー入力や I/O タスクと共存することはできますが、計算中心タスクどうしは競合してしまいます。  
  
- 単純な状態変更については、<xref:System.Threading.Interlocked> ステートメント (Visual Basic では `lock`) ではなく、`SyncLock` クラスのメソッドの使用を検討します。 `lock` ステートメントは汎用的なツールとして優れていますが、<xref:System.Threading.Interlocked> クラスは、分離不可能な状態であることが必要な更新のパフォーマンスに優れています。 内部的には、競合がない場合は、単一の lock プレフィックスが実行されます。 コードの校閲で、次に示す例のようなコードを探します。 最初の例では、状態変数をインクリメントしています。  
  
    ```vb  
    SyncLock lockObject  
        myField += 1  
    End SyncLock  
    ```  
  
    ```csharp  
    lock(lockObject)
    {  
        myField++;  
    }  
    ```  
  
     <xref:System.Threading.Interlocked.Increment%2A> ステートメントの代わりに `lock` メソッドを次のように使用すると、パフォーマンスを向上できます。  
  
    ```vb  
    System.Threading.Interlocked.Increment(myField)  
    ```  
  
    ```csharp  
    System.Threading.Interlocked.Increment(myField);  
    ```  
  
    > [!NOTE]
    > .NET Framework 2.0 以降では、1 より大きいアトミック インクリメントに対して <xref:System.Threading.Interlocked.Add%2A> メソッドを使用します。  
  
     2 番目の例では、参照型の変数を、null 参照 (Visual Basic では `Nothing`) の場合にのみ更新しています。  
  
    ```vb  
    If x Is Nothing Then  
        SyncLock lockObject  
            If x Is Nothing Then  
                x = y  
            End If  
        End SyncLock  
    End If  
    ```  
  
    ```csharp  
    if (x == null)  
    {  
        lock (lockObject)  
        {  
            x ??= y;
        }  
    }  
    ```  
  
     この代わりに <xref:System.Threading.Interlocked.CompareExchange%2A> メソッドを次のように使用すると、パフォーマンスを向上できます。  
  
    ```vb  
    System.Threading.Interlocked.CompareExchange(x, y, Nothing)  
    ```  
  
    ```csharp  
    System.Threading.Interlocked.CompareExchange(ref x, y, null);  
    ```  
  
    > [!NOTE]
    > .NET Framework 2.0 より、<xref:System.Threading.Interlocked.CompareExchange%60%601%28%60%600%40%2C%60%600%2C%60%600%29> メソッド オーバーロードから参照型に対してタイプ セーフの代替が提供されます。
  
## <a name="recommendations-for-class-libraries"></a>クラス ライブラリに関する推奨事項  
 マルチスレッド用のクラス ライブラリをデザインするときには、次のガイドラインを検討します。  
  
- 可能な限り、同期の必要を避けるようにします。 これは、頻繁に使用するコードの場合に特に言えます。 たとえば、競合状態をなくすのではなく、競合状態に対応できるようにアルゴリズムを調整できる場合があります。 不要な同期があると、パフォーマンスが低下し、デッドロックや競合状態が発生する可能性が生じます。  
  
- 静的なデータ (Visual Basic では `Shared`) は既定でスレッド セーフにします。  
  
- インスタンス データは既定でスレッド セーフにしないようにします。 スレッド セーフなコードを作成するロックを追加すると、パフォーマンスが低下し、ロックの競合が増加し、デッドロックが発生する可能性が生じます。 一般的なアプリケーション モデルでは、一度にユーザー コードを実行するスレッドは 1 つだけにして、スレッド セーフを実現する必要を最小限に抑えます。 この理由から、.NET Framework のクラス ライブラリは既定ではスレッド セーフではないことが必要です。  
  
- 静的状態を変更する静的メソッドは提供しないでください。 一般的なサーバーのシナリオでは、静的状態は要求間で共有されます。つまり、複数のスレッドがそのコードを同時に実行できます。 これにより、スレッド処理のバグが発生する可能性が高くなります。 要求間で共有されないインスタンスにデータをカプセル化するデザイン パターンの使用を検討してください。 加えて、静的なデータを同期する場合は、状態を変更する呼び出しが静的メソッド間にあると、デッドロックや冗長な同期が生じる可能性があり、パフォーマンスに悪影響を及ぼします。  
  
## <a name="see-also"></a>参照

- [スレッド化](../../../docs/standard/threading/index.md)
- [スレッドおよびスレッド処理](../../../docs/standard/threading/threads-and-threading.md)
