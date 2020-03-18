---
title: スレッドの使用とスレッド処理
ms.date: 08/08/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- threading [.NET Framework], about threading
- managed threading
ms.assetid: 9b5ec2cd-121b-4d49-b075-222cf26f2344
ms.openlocfilehash: 1d487edff2cdc2e63f81963bfaa1f68a06e5b36e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75936840"
---
# <a name="using-threads-and-threading"></a>スレッドの使用とスレッド処理

.NET では、複数の操作を同時に実行するアプリケーションを記述できます。 他の操作を止める可能性がある操作は個別のスレッドで実行できます。これは*マルチスレッド*や*フリー スレッド*と呼ばれているプロセスです。  
  
マルチスレッドを利用するアプリケーションはユーザーの入力に対する応答性が良くなります。プロセッサを集中的に利用するタスクが個別のスレッドで実行されるため、ユーザー インターフェイスの動作が妨げられません。 マルチスレッドはまた、スケーラブルなアプリケーションの開発にも便利です。ワークロードが増えたとき、スレッドを追加できるからです。

> [!NOTE]
> アプリケーションのスレッドの動作をさらに細かく制御するには、スレッドを自分で管理します。 しかし、.NET Framework 4 以降では、<xref:System.Threading.Tasks.Parallel?displayProperty=nameWithType> クラスおよび <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> クラス、[Parallel LINQ (PLINQ)](../parallel-programming/parallel-linq-plinq.md)、<xref:System.Collections.Concurrent?displayProperty=nameWithType> 名前空間の新しい同時実行コレクション クラス、スレッドではなくタスクの概念を基にした新しいプログラミング モデルにより、マルチスレッド プログラミングが大幅に簡略化されています。 詳細については、「[.NET での並列プログラミング](../parallel-programming/index.md)」と「[タスク並列ライブラリ (TPL)](../parallel-programming/task-parallel-library-tpl.md)」を参照してください。

## <a name="how-to-create-and-start-a-new-thread"></a>方法: 新しいスレッドを作成して開始する

<xref:System.Threading.Thread?displayProperty=nameWithType> クラスの新しいインスタンスを作成し、コンストラクターに対して新しいスレッドで実行するメソッド名を指定して、新しいスレッドを作成します。 作成したスレッドを開始するには、<xref:System.Threading.Thread.Start%2A?displayProperty=nameWithType> メソッドを呼び出します。 詳細と例については、「[スレッドを作成し、開始時にデータを渡す](creating-threads-and-passing-data-at-start-time.md)」の記事と <xref:System.Threading.Thread> API リファレンスを参照してください。

## <a name="how-to-stop-a-thread"></a>方法: スレッドを停止する

スレッドの実行を終了するには、<xref:System.Threading.CancellationToken?displayProperty=nameWithType> を使用します。 これにより、統一された方法で協調的にスレッドを停止することができます。 詳細については、「[マネージド スレッドのキャンセル](cancellation-in-managed-threads.md)」を参照してください。

協調的なキャンセルを行うように設計されていないサード パーティのコードがスレッドで実行されているために、スレッドを協調的に停止できない場合があります。 この場合は、その実行を強制的に終了することができます。 スレッドの実行を強制的に終了する場合、.NET Framework では <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> メソッドを使用できます。 このメソッドでは、呼び出されたスレッド上で <xref:System.Threading.ThreadAbortException> が生成されます。 詳細については、「[スレッドの破棄](destroying-threads.md)」を参照してください。 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> メソッドは、.NET Core ではサポートされていません。 .NET Core でサード パーティのコードの実行を強制的に終了する必要がある場合は、それを別のプロセスで実行し、<xref:System.Diagnostics.Process.Kill%2A?displayProperty=nameWithType> を使用します。

.NET Framework 4 よりも前では、<xref:System.Threading.CancellationToken?displayProperty=nameWithType> は使用できません。 以前の .NET Framework バージョンでスレッドを停止するには、スレッド同期の手法を使用して、協調的なキャンセルを手動で実装する必要があります。 たとえば、volatile bool 型のフィールド `shouldStop` を作成し、それを使ってスレッドで実行されるコードの停止を要求することができます。 詳細については、C# リファレンスの [volatile](../../csharp/language-reference/keywords/volatile.md) に関する記事と、<xref:System.Threading.Volatile?displayProperty=nameWithType> に関する記事をご覧ください。

<xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType> メソッドを使用すると、停止させるスレッドが終了するまで、呼び出し元のスレッドを待機させることができます。

## <a name="how-to-pause-or-interrupt-a-thread"></a>方法: スレッドを一時停止または中断する

<xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType> メソッドを使用して、現在のスレッドを指定した時間一時停止します。 <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> メソッドを呼び出すことによって、ブロックされたスレッドを中断できます。 詳細については、「[スレッドの一時中断と再開](pausing-and-resuming-threads.md)」を参照してください。

## <a name="thread-properties"></a>スレッド プロパティ

<xref:System.Threading.Thread> プロパティの一部を次の表に示します。  
  
|property|[説明]|  
|--------------|-----------|  
|<xref:System.Threading.Thread.IsAlive%2A>|スレッドが起動していて、正常終了しておらず中止されてもいない場合は、`true` が返されます。|  
|<xref:System.Threading.Thread.IsBackground%2A>|スレッドがバックグラウンド スレッドかどうかを示すブール値を取得または設定します。 バックグラウンド スレッドはフォアグラウンド スレッドに似ていますが、プロセスの停止を防ぐことはありません。 あるプロセスに属するフォアグラウンド スレッドがすべて停止すると、共通言語ランタイムは、アライブ状態のバックグラウンド スレッドで <xref:System.Threading.Thread.Abort%2A> メソッドを呼び出し、プロセスを終了します。 詳細については、「[フォアグラウンド スレッドとバックグラウンド スレッド](foreground-and-background-threads.md)」を参照してください。|  
|<xref:System.Threading.Thread.Name%2A>|スレッドの名前を取得または設定します。 デバッグ時、個々のスレッドを検出するために頻繁に使用されます。|  
|<xref:System.Threading.Thread.Priority%2A>|オペレーティング システムがスレッド スケジュールに優先順位を設定するために使用する <xref:System.Threading.ThreadPriority> 値を取得または設定します。 詳細については、「[スレッドのスケジューリング](scheduling-threads.md)」と <xref:System.Threading.ThreadPriority> リファレンスを参照してください。|  
|<xref:System.Threading.Thread.ThreadState%2A>|スレッドの現在の状態を示す <xref:System.Threading.ThreadState> 値を取得します。|  

## <a name="see-also"></a>参照

- <xref:System.Threading.Thread?displayProperty=nameWithType>
- [スレッドおよびスレッド処理](threads-and-threading.md)
- [並列プログラミング](../parallel-programming/index.md)
