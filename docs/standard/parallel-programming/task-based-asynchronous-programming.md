---
title: タスク ベースの非同期プログラミング - .NET
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallelism, task
ms.assetid: 458b5e69-5210-45e5-bc44-3888f86abd6f
ms.openlocfilehash: 51292d977f2be87cec7c3481f5004fe5fe756224
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74204537"
---
# <a name="task-based-asynchronous-programming"></a>タスク ベースの非同期プログラミング

タスク並列ライブラリ (TPL) は、非同期操作を表す*タスク*の概念に基づいています。 いくつかの点で、タスクはスレッドまたは <xref:System.Threading.ThreadPool> 作業項目に似ていますが、高いレベルで抽象化しています。 *タスクの並列化*とは、1 つ以上の独立したタスクを同時に実行することです。 タスクが提供する主な利点は次の 2 つです。

- システム リソースをより効率的かつスケーラブルに利用する。

     背後では、タスクは <xref:System.Threading.ThreadPool> へのキューとして配置されます。これはスレッド数を判別および調整し、負荷分散によってスループットを最大化する、アルゴリズムを使用して強化されています。 これによりタスクが比較的軽量化されるため、多数のタスクを作成して粒度の高い並列化を実現できます。

- スレッドまたは作業項目より、プログラムによる制御を詳細に行うことができる。

     タスクおよびタスクを中心に構築されたフレームワークでは、待機、キャンセル、継続、信頼性の高い例外処理、詳細なステータス、カスタムのスケジュール設定などをサポートする豊富な API が用意されています。

この 2 つの理由により、.NET Framework では、マルチスレッド、非同期および並列コードを記述するために、TPL で API を使用することをお勧めしています。

## <a name="creating-and-running-tasks-implicitly"></a>暗黙的なタスクの作成と実行

<xref:System.Threading.Tasks.Parallel.Invoke%2A?displayProperty=nameWithType> メソッドには、任意の数のステートメントを同時に実行する便利な方法が用意されています。 作業項目ごとに <xref:System.Action> デリゲートに渡すだけです。 これらのデリゲートを最も簡単に作成するには、ラムダ式を使用します。 ラムダ式では、名前付きメソッドを呼び出したり、コード インラインを指定したりできます。 次の例では、2 つのタスクを同時に作成および開始する基本の <xref:System.Threading.Tasks.Parallel.Invoke%2A> 呼び出しを示しています。 最初のタスクは `DoSomeWork` という名のメソッドを呼び出すラムダ式によって表され、2 番目のタスクは `DoSomeOtherWork` という名のメソッドを呼び出すラムダ式によって表されます。

> [!NOTE]
> ここでは、ラムダ式を使用して TPL でデリゲートを定義します。 C# または Visual Basic のラムダ式についての情報が必要な場合は、「[PLINQ および TPL のラムダ式](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md)」を参照してください。

[!code-csharp[TPL#21](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl/cs/tpl.cs#21)]
[!code-vb[TPL#21](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl/vb/tpl_vb.vb#21)]

> [!NOTE]
> <xref:System.Threading.Tasks.Task> によって背後で作成される <xref:System.Threading.Tasks.Parallel.Invoke%2A> インスタンスの数は、指定するデリゲートの数と等しくなくてもかまいません。 TPL では、特に多数のデリゲートによるさまざまな最適化方法を採用しています。

詳細については、「[How to: Use Parallel.Invoke to Execute Parallel Operations](../../../docs/standard/parallel-programming/how-to-use-parallel-invoke-to-execute-parallel-operations.md)」を参照してください。

タスクの実行をさらに制御する場合、またはタスクから値を返す場合、<xref:System.Threading.Tasks.Task> オブジェクトをより明示的に操作する必要があります。

## <a name="creating-and-running-tasks-explicitly"></a>明示的なタスクの作成と実行

値を返さないタスクは、<xref:System.Threading.Tasks.Task?displayProperty=nameWithType> クラスによって表されます。 値を返すタスクは、<xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> から継承された <xref:System.Threading.Tasks.Task> クラスで表されます。 タスク オブジェクトはインフラストラクチャの詳細を処理し、タスクの有効期間内に呼び出し元のスレッドからアクセスできるメソッドとプロパティを提供します。 たとえば、タスクの <xref:System.Threading.Tasks.Task.Status%2A> プロパティに任意のタイミングでアクセスして、タスクが開始されたか、完了まで実行されたか、取り消されたか、または例外がスローされたかどうかを確認できます。 状態は、<xref:System.Threading.Tasks.TaskStatus> 列挙型によって表されます。

タスクを作成するときは、タスクが実行するコードをカプセル化するユーザー デリゲートを指定します。 このデリゲートは名前付きデリゲート、匿名メソッド、またはラムダ式として表すことができます。 ラムダ式には、次の例で示すような名前付きメソッドへの呼び出しを含めることができます。 この例は <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> メソッドの呼び出しを含み、コンソール モードのアプリケーションが終了する前にタスクの実行が完了するようにしていることに注意してください。

[!code-csharp[TPL_TaskIntro#1](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/lambda1.cs#1)]
[!code-vb[TPL_TaskIntro#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/lambda1.vb#1)]

<xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> メソッドを使用して、一度の操作でタスクを作成および開始することもできます。 タスクを管理するため、<xref:System.Threading.Tasks.Task.Run%2A> メソッドは、現在のスレッドに関連付けられたタスク スケジューラにかかわらず、既定のタスク スケジューラを使用します。 <xref:System.Threading.Tasks.Task.Run%2A> メソッドは、タスクの作成とスケジュールの詳細な制御が必要ない場合に、タスクを作成および開始するために適しています。

[!code-csharp[TPL_TaskIntro#2](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/run1.cs#2)]
[!code-vb[TPL_TaskIntro#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/run1.vb#2)]

<xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType> メソッドを使用して、一度の操作でタスクを作成および開始することもできます。 次の例に示すように、作成とスケジュール設定を分ける必要がない場合、追加のタスク作成オプションまたは特定のスケジューラを使う必要がある場合、または <xref:System.Threading.Tasks.Task.AsyncState%2A?displayProperty=nameWithType> プロパティを使用して取得できるタスクに追加の状態を渡す必要がある場合は、このメソッドを使用します。

[!code-csharp[TPL_TaskIntro#3](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/asyncstate.cs#23)]
[!code-vb[TPL_TaskIntro#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/asyncstate.vb#23)]

<xref:System.Threading.Tasks.Task> および <xref:System.Threading.Tasks.Task%601> はそれぞれ、<xref:System.Threading.Tasks.Task.Factory%2A> の既定のインスタンスを返す、静的な <xref:System.Threading.Tasks.TaskFactory> プロパティを公開するので、メソッドを `Task.Factory.StartNew()` として呼び出すことができます。 また、次の例のタスクは <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> 型であるため、それぞれのタスクは計算の結果を格納するパブリックな <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> プロパティを持ちます。 タスクは非同期に実行され、任意の順序で完了されることがあります。 計算が終了する前に <xref:System.Threading.Tasks.Task%601.Result%2A> プロパティにアクセスした場合、このプロパティは値が使用可能な状態になるまで呼び出しスレッドをブロックします。

[!code-csharp[TPL_TaskIntro#4](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/result1.cs#4)]
[!code-vb[TPL_TaskIntro#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/result1.vb#4)]

詳細については、「[方法: タスクから値を返す](../../../docs/standard/parallel-programming/how-to-return-a-value-from-a-task.md)」を参照してください。

ラムダ式を使用してデリゲートを作成すると、ソース コード内の該当ポイントで参照できるすべての変数にアクセスできます。 ただし、特にループ内では、ラムダによって変数が予想どおりにキャプチャされない場合があります。 ラムダでは、反復処理が実行されるたびに変更された値をキャプチャするのではなく、最終値だけがキャプチャされます。 この問題の例を次に示します。 これは `CustomData` オブジェクトをインスタンス化するラムダ式にループ カウンターを渡し、オブジェクトの識別子としてループ カウンターを使用します。 この例の出力結果が示すように、`CustomData` の各オブジェクトは同じ識別子を持ちます。

[!code-csharp[TPL_TaskIntro#22](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/iteration1b.cs#22)]
[!code-vb[TPL_TaskIntro#22](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/iteration1b.vb#22)]

反復処理が実行されるたびに値にアクセスできるようにするには、コンストラクターによって状態オブジェクトをタスクに提供します。 次の例では、ラムダ式に渡される `CustomData` オブジェクトを作成するときに、ループ カウンターを使用して前の例を変更しています。  この例の出力結果が示すように、`CustomData` の各オブジェクトは、オブジェクトがインスタンス化されたときのループ カウンターの値に基づいて、一意の識別子を持ちます。

[!code-csharp[TPL_TaskIntro#21](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/iteration1a.cs#21)]
[!code-vb[TPL_TaskIntro#21](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/iteration1a.vb#21)]

この状態は、タスク デリゲートに引数として渡されます。<xref:System.Threading.Tasks.Task.AsyncState%2A?displayProperty=nameWithType> プロパティを使用することで、タスク オブジェクトから状態にアクセスできます。  次の例は、前の例を変更したものです。 <xref:System.Threading.Tasks.Task.AsyncState%2A> プロパティを使用して、ラムダ式に渡される `CustomData` オブジェクトに関する情報を表示します。

[!code-csharp[TPL_TaskIntro#23](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/asyncstate.cs#23)]
[!code-vb[TPL_TaskIntro#23](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/asyncstate.vb#23)]

## <a name="task-id"></a>[タスク ID]

各タスクは、アプリケーション ドメイン内で一意に識別され、<xref:System.Threading.Tasks.Task.Id%2A?displayProperty=nameWithType> プロパティを使用してアクセスできる整数の ID を受け取ります。 この ID は、Visual Studio デバッガーの **[並列スタック]** ウィンドウおよび **[タスク]** ウィンドウでタスク情報を確認する場合に役立ちます。 この ID は ID が要求されるまでは作成されません。したがって、タスクの ID はプログラムが実行されるたびに異なる場合があります。 デバッガーでタスク ID を表示する方法の詳細については、「[[タスク] ウィンドウの使用](/visualstudio/debugger/using-the-tasks-window)」と「[[並列スタック] ウィンドウの使用](/visualstudio/debugger/using-the-parallel-stacks-window)」を参照してください。

## <a name="task-creation-options"></a>タスクの作成オプション

タスクを作成するほとんどの API には、<xref:System.Threading.Tasks.TaskCreationOptions> パラメーターを受け入れるオーバーロードが用意されています。 これらのオプションのいずれかを指定することで、タスク スケジューラにスレッド プール上のタスクをスケジュールする方法を指示できます。 タスクの作成オプションの一覧を次に示します。

|<xref:System.Threading.Tasks.TaskCreationOptions> パラメーター値|[説明]|
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
|<xref:System.Threading.Tasks.TaskCreationOptions.None>|オプションを指定しなかった場合の既定値です。 スケジューラは既定のヒューリスティックを使用してタスクをスケジュールします。|
|<xref:System.Threading.Tasks.TaskCreationOptions.PreferFairness>|先に作成されたタスクが先に実行され、後から作成されたタスクは後から実行されるように、タスクのスケジュールを指定します。|
|<xref:System.Threading.Tasks.TaskCreationOptions.LongRunning>|実行に時間のかかる操作を表すタスクであることを示します。|
|<xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent>|子が存在する場合、現在のタスクにアタッチされた子としてタスクを作成するように指定します。 詳細については、「[アタッチされた子タスクとデタッチされた子タスク](../../../docs/standard/parallel-programming/attached-and-detached-child-tasks.md)」を参照してください。|
|<xref:System.Threading.Tasks.TaskCreationOptions.DenyChildAttach>|内側のタスクが `AttachedToParent` オプションを指定すると、そのタスクはアタッチされた子タスクにならないことを指定します。|
|<xref:System.Threading.Tasks.TaskCreationOptions.HideScheduler>|<xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType> または <xref:System.Threading.Tasks.Task%601.ContinueWith%2A?displayProperty=nameWithType> などのメソッドを特定のタスクから呼び出して作成されたタスクのタスク スケジューラは、このタスクが実行されているスケジューラではなく、既定のスケジューラであることを指定します。|

このオプションは、ビットごとの **OR** 演算を使用して組み合わせることができます。 次の例は、<xref:System.Threading.Tasks.TaskCreationOptions.LongRunning> オプションと <xref:System.Threading.Tasks.TaskContinuationOptions.PreferFairness> オプションが指定されたタスクを示しています。

[!code-csharp[TPL_TaskIntro#03](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/taskintro.cs#03)]
[!code-vb[TPL_TaskIntro#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/tpl_intro.vb#03)]

## <a name="tasks-threads-and-culture"></a>タスク、スレッド、およびカルチャ

各スレッドにはカルチャと UI カルチャが関連付けられています。それぞれのカルチャは <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> と <xref:System.Threading.Thread.CurrentUICulture%2A?displayProperty=nameWithType> プロパティにより定義されています。 スレッドのカルチャは、書式設定、解析、並べ替え、文字列比較などの操作で使用されます。 スレッドの UI カルチャはリソースの検索で使用されます。 通常、<xref:System.Globalization.CultureInfo.DefaultThreadCurrentCulture%2A?displayProperty=nameWithType> プロパティと <xref:System.Globalization.CultureInfo.DefaultThreadCurrentUICulture%2A?displayProperty=nameWithType> プロパティを使用してアプリケーション ドメイン内のすべてのスレッドの既定のカルチャを指定していない限り、スレッドの既定のカルチャと既定の UI カルチャはシステム カルチャによって定義されます。 スレッドのカルチャを明示的に設定して新しいスレッドを開始すると、新しいスレッドは呼び出し元スレッドのカルチャを継承せず、既定のシステム カルチャがそのカルチャとして使用されます。 .NET Framework 4.6 より前のバージョンの .NET Framework を対象とするアプリのタスク ベース プログラミング モデルは、この方針に準拠します。

> [!IMPORTANT]
> 呼び出し元スレッドのカルチャは、タスクのコンテキストの一部として、.NET Framework 4.6 で*実行される*アプリではなく、.NET Framework 4.6 を*対象*としたアプリに適用されます。 Visual Studio 内部でプロジェクトを作成する場合、 **[新しいプロジェクト]** ダイアログ ボックスの上部にあるドロップダウン リストから特定バージョンの .NET Framework を選択すると、そのバージョンを対象にできます。また、Visual Studio 外部では <xref:System.Runtime.Versioning.TargetFrameworkAttribute> 属性を使用できます。 .NET Framework 4.6 より前のバージョンの .NET Framework を対象とするアプリ、または特定のバージョンの .NET Framework を対象としないアプリでは、タスクのカルチャは継続的に、それが実行されているスレッドのカルチャによって決まります。

.NET Framework 4.6 以降を対象とするアプリでは、タスクがスレッド プールのスレッドで非同期に実行されている場合でも、呼び出し元スレッドのカルチャが各タスクに継承されます。

簡単な例を次に示します。 <xref:System.Runtime.Versioning.TargetFrameworkAttribute> 属性を使用して .NET Framework 4.6 を対象とし、アプリの現在のカルチャをフランス語 (フランス)、またはフランス語 (フランス) が現在のカルチャである場合は英語 (米国) に変更します。 次に、変更後のカルチャの通貨値として書式設定された数値を返す `formatDelegate` という名前のデリゲートを呼び出します。 デリゲートは、同期タスクまたは非同期タスクのいずれの場合でも、予期される結果を返すことに注意してください。これは、非同期タスクは呼び出し元スレッドのカルチャを継承するためです。

[!code-csharp[System.Globalization.CultureInfo.Class.Async#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.globalization.cultureinfo.class.async/cs/asyncculture1.cs#5)]
[!code-vb[System.Globalization.CultureInfo.Class.Async#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.globalization.cultureinfo.class.async/vb/asyncculture1.vb#5)]

Visual Studio を使用している場合、プロジェクトを <xref:System.Runtime.Versioning.TargetFrameworkAttribute>[新しいプロジェクト] **ダイアログで作成する際に、** 属性を省略して、代わりに .NET Framework 4.6 を対象として選択できます。

.NET Framework 4.6 より前のバージョンの .NET Framework を対象とするアプリの動作が出力に反映されるようにするため、ソース コードから <xref:System.Runtime.Versioning.TargetFrameworkAttribute> 属性を削除します。 出力には、呼び出し元スレッドのカルチャではなく、既定のシステム カルチャの書式指定規則が反映されます。

非同期タスクとカルチャの詳細については、「<xref:System.Globalization.CultureInfo>」トピックの「カルチャおよび非同期タスク ベースの操作」を参照してください。

## <a name="creating-task-continuations"></a>タスクの継続の作成

<xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> メソッドおよび <xref:System.Threading.Tasks.Task%601.ContinueWith%2A?displayProperty=nameWithType> メソッドで、"*継続元タスク*" が終了したときに開始されるタスクを指定できます。 継続タスクのデリゲートは継続元タスクへの参照を渡し、継続元タスクのステータスを調査できるようにし、また <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> プロパティの値を取得して、継続元の出力を継続への入力として使用できるようにします。

次の例では、`getData` タスクは <xref:System.Threading.Tasks.TaskFactory.StartNew%60%601%28System.Func%7B%60%600%7D%29?displayProperty=nameWithType> メソッドの呼び出しによって開始されます。 `processData` タスクは `getData` が終了したときに自動的に開始され、`displayData` は `processData` が終了したときに開始されます。 `getData` は、`processData` タスクの `getData` プロパティを使用して <xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> タスクがアクセス可能な、整数の配列を生成します。 `processData` タスクはその配列を処理し、<xref:System.Threading.Tasks.Task%601.ContinueWith%60%601%28System.Func%7BSystem.Threading.Tasks.Task%7B%600%7D%2C%60%600%7D%29?displayProperty=nameWithType> メソッドに渡されるラムダ式の戻り値の型から推論される型を持つ結果を返します。 `displayData` タスクは、`processData` が終了したときに自動的に実行され、<xref:System.Tuple%603> ラムダ式が返した `processData` オブジェクトは、`displayData` タスクの `processData` プロパティを使用して、<xref:System.Threading.Tasks.Task%601.Result%2A?displayProperty=nameWithType> タスクからアクセス可能です。 `displayData` タスクは `processData` タスクから結果を受け取り、同様の方法を使用して (プログラムで使用できるようになったと) 推論される型を持つ結果を <xref:System.Threading.Tasks.Task%601.Result%2A> プロパティで生成します。

[!code-csharp[TPL_TaskIntro#5](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/continuations1.cs#5)]
[!code-vb[TPL_TaskIntro#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/continuations1.vb#5)]

<xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> がインスタンス メソッドであるため、<xref:System.Threading.Tasks.Task%601> のオブジェクトをそれぞれの継続元タスクにインスタンス化する代わりに、メソッド呼び出しを連結することができます。 次の例は前の例と機能的には同じものですが、呼び出しを <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType> メソッドに連結している点が異なります。 メソッドの呼び出しチェーンによって返される <xref:System.Threading.Tasks.Task%601> オブジェクトが最終的な継続タスクであることに注意してください。

[!code-csharp[TPL_TaskIntro#24](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/continuations2.cs#24)]
[!code-vb[TPL_TaskIntro#24](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/continuations2.vb#24)]

<xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A> メソッドおよび <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A> メソッドを使用すると、複数のタスクから継続できます。

詳細については、「[継続タスクを使用したタスクの連結](../../../docs/standard/parallel-programming/chaining-tasks-by-using-continuation-tasks.md)」を参照してください。

## <a name="creating-detached-child-tasks"></a>デタッチされた子タスクの作成

タスクで実行中のユーザー コードで新しいタスクを作成し、<xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent> オプションを指定しない場合、新しいタスクはどのような方法でも親タスクとは同期されません。 非同期タスクのこの型は、*デタッチされた入れ子のタスク*、または*デタッチされた子タスク*と呼ばれます。 次の例は、デタッチされた子タスクを 1 つ作成するタスクを示しています。

[!code-csharp[TPL_TaskIntro#07](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/taskintro.cs#07)]
[!code-vb[TPL_TaskIntro#07](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/tpl_intro.vb#07)]

親タスクはデタッチされた子タスクの終了を待機しないことに注意してください。

## <a name="creating-child-tasks"></a>子タスクの作成

タスクで実行中のユーザー コードで <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent> オプションを使用してタスクを作成すると、新しいタスクは親タスクに "*アタッチされた子タスク*" になります。 <xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent> オプションを使用すると、構成されたタスクの並列化を表現できます。親タスクは、すべてのアタッチされた子タスクが終了するのを暗黙的に待機するためです。 次の例は、アタッチされた子タスクを 10 個作成する親タスクを示しています。 この例は <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> メソッドを呼び出して親タスクの完了を待機しているが、アタッチされた子タスクの完了を明示的には待機する必要がないことに注意してください。

[!code-csharp[TPL_TaskIntro#8](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/child1.cs#8)]
[!code-vb[TPL_TaskIntro#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/child1.vb#8)]

親タスクは <xref:System.Threading.Tasks.TaskCreationOptions.DenyChildAttach?displayProperty=nameWithType> オプションを使用して、他のタスクが親タスクにアタッチすることを防ぐことができます。 詳細については、「[アタッチされた子タスクとデタッチされた子タスク](../../../docs/standard/parallel-programming/attached-and-detached-child-tasks.md)」を参照してください。

## <a name="waiting-for-tasks-to-finish"></a>タスクの完了を待機する

<xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 型と <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> 型には、タスクが終了するまで待機できる <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> メソッドのオーバーロードがいくつか用意されています。 さらに、静的な <xref:System.Threading.Tasks.Task.WaitAll%2A?displayProperty=nameWithType> メソッドおよび <xref:System.Threading.Tasks.Task.WaitAny%2A?displayProperty=nameWithType> メソッドのオーバーロードにより、一部またはすべてのタスクの配列が終了するまで待機できます。

通常は、タスクを待機するのは次のいずれかの場合です。

- メイン スレッドが、タスクで計算される最終的な結果に依存する。

- タスクからスローされる可能性のある例外を処理する必要がある。

- アプリケーションは、すべてのタスクが実行を完了する前に終了する場合があります。 たとえば、コンソール アプリケーションは `Main` (アプリケーションのエントリ ポイント) のすべての同期コードが実行されると、すぐに終了します。

次の例は、例外処理を含まない基本的なパターンを示しています。

[!code-csharp[TPL_TaskIntro#06](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_taskintro/cs/taskintro.cs#06)]
[!code-vb[TPL_TaskIntro#06](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_taskintro/vb/tpl_intro.vb#06)]

例外処理を示す例については、[例外処理](../../../docs/standard/parallel-programming/exception-handling-task-parallel-library.md)に関する記事を参照してください。

タイムアウトを指定できるオーバーロードおよび別の <xref:System.Threading.CancellationToken> を入力パラメーターとして受け取るオーバーロードの中には、プログラムによって、またはユーザーの入力に応答して待機自体を取り消すことができるものがあります。

タスクを待機する場合は、<xref:System.Threading.Tasks.TaskCreationOptions.AttachedToParent?displayProperty=nameWithType> オプションを使用して作成されたタスクのすべての子タスクを暗黙的に待機します。 タスクが既に完了している場合、直ちに <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> が返されます。 タスクで発生した例外は、<xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> メソッドがタスクの完了後に呼び出された場合でも <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> メソッドによってスローされます。

## <a name="composing-tasks"></a>タスクを構成する

<xref:System.Threading.Tasks.Task> と <xref:System.Threading.Tasks.Task%601> クラスは複数のメソッドを提供し、共通のパターンを実装したり、C#、Visual Basic、F# によって提供される非同期言語機能を使うために、複数のタスクを構成したりするのに役立ちます。 このセクションでは <xref:System.Threading.Tasks.Task.WhenAll%2A>、<xref:System.Threading.Tasks.Task.WhenAny%2A>、<xref:System.Threading.Tasks.Task.Delay%2A> および <xref:System.Threading.Tasks.Task.FromResult%2A> メソッドについて説明します。

### <a name="taskwhenall"></a>Task.WhenAll

<xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> メソッドは、複数の <xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> オブジェクトが終了するのを、非同期的に待機します。 提供されるオーバーロード バージョンにより、不均一なタスクのセットを待機することができます。 たとえば、1 回のメソッド呼び出しで完了する、複数の <xref:System.Threading.Tasks.Task> および <xref:System.Threading.Tasks.Task%601> オブジェクトを待機できます。

### <a name="taskwhenany"></a>Task.WhenAny

<xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> メソッドは、複数の <xref:System.Threading.Tasks.Task> の 1 つ、または <xref:System.Threading.Tasks.Task%601> オブジェクトが終了するのを、非同期的に待機します。 <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> メソッドと同様に、このメソッドはオーバーロード バージョンを提供し、これによって不均一なタスクのセットを待機することができます。 <xref:System.Threading.Tasks.Task.WhenAny%2A> メソッドは、特に次のシナリオに役立ちます。

- 重複した操作。 多くの方法で実行できるアルゴリズムまたは操作を検討してください。 <xref:System.Threading.Tasks.Task.WhenAny%2A> メソッドを使用すると、最初の操作を完了して残りの操作を取り消すように、操作を選択できます。

- インタリーブされた操作。 複数の操作を開始して、それらの操作のすべてが完了し、各操作が完了したら <xref:System.Threading.Tasks.Task.WhenAny%2A> メソッドを使って結果を処理できるようにします。 1 つの操作が完了したら、1 つ以上の追加タスクを開始できます。

- 制限された操作。 <xref:System.Threading.Tasks.Task.WhenAny%2A> メソッドを使用して、前のシナリオを拡張し、同時操作の数を制限することができます。

- 有効期限切れの操作。 <xref:System.Threading.Tasks.Task.WhenAny%2A> メソッドを使うと、1 つ以上のタスクと特定の時間後に終了する 1 つのタスクから選択できます。たとえば、<xref:System.Threading.Tasks.Task.Delay%2A> メソッドが返すタスクなどです。 <xref:System.Threading.Tasks.Task.Delay%2A> メソッドについては、次のセクションで説明します。

### <a name="taskdelay"></a>Task.Delay

<xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType> メソッドは、指定時間後に終了する <xref:System.Threading.Tasks.Task> オブジェクトを生成します。 このメソッドを使用すると、データのポーリングをときどき行うループをビルドしたり、タイムアウトを使用したり、ユーザー入力の処理を遅延させたりすることができます。

### <a name="tasktfromresult"></a>Task(T).FromResult

<xref:System.Threading.Tasks.Task.FromResult%2A?displayProperty=nameWithType> メソッドを使用すると、あらかじめ計算された結果を保持する <xref:System.Threading.Tasks.Task%601> オブジェクトを作成できます。 このメソッドは <xref:System.Threading.Tasks.Task%601> オブジェクトの結果があらかじめ計算されている <xref:System.Threading.Tasks.Task%601> オブジェクトを返す、非同期操作を実行する場合に便利です。 キャッシュに保持されている非同期ダウンロード操作の結果を取得する <xref:System.Threading.Tasks.Task.FromResult%2A> の使用例の詳細については、「[方法: 事前計算済みのタスクを作成する](../../../docs/standard/parallel-programming/how-to-create-pre-computed-tasks.md)」を参照してください。

## <a name="handling-exceptions-in-tasks"></a>タスクでの例外処理

タスクで 1 つ以上の例外がスローされると、例外は <xref:System.AggregateException> 例外でラップされます。 例外はタスクに連結されたスレッドに反映されます。通常は、タスクの終了を待機しているスレッドか、または <xref:System.Threading.Tasks.Task%601.Result%2A> プロパティにアクセスするスレッドです。 この動作では、.NET Framework ポリシーが適用され、既定ではすべてのハンドルされない例外によってプロセスが終了されます。 呼び出し元のコードは `try`/`catch` ブロックで、次のいずれかを使用して、例外を処理できます。

- <xref:System.Threading.Tasks.Task.Wait%2A> メソッド

- <xref:System.Threading.Tasks.Task.WaitAll%2A> メソッド

- <xref:System.Threading.Tasks.Task.WaitAny%2A> メソッド

- <xref:System.Threading.Tasks.Task%601.Result%2A> プロパティ

連結しているスレッドでも、タスクがガベージ コレクトされる前に <xref:System.Threading.Tasks.Task.Exception%2A> プロパティにアクセスすることで例外を処理できます。 このプロパティにアクセスすると、ハンドルされない例外が、オブジェクトが最終処理されたときにプロセスを終了する例外の反映動作をトリガーしないようにできます。

例外とタスクの詳細については、[例外処理](../../../docs/standard/parallel-programming/exception-handling-task-parallel-library.md)に関する記事を参照してください。

## <a name="canceling-tasks"></a>タスクの取り消し

<xref:System.Threading.Tasks.Task> クラスは他の処理と連携したキャンセル処理をサポートしており、.NET Framework 4 で導入された <xref:System.Threading.CancellationTokenSource?displayProperty=nameWithType> クラスおよび <xref:System.Threading.CancellationToken?displayProperty=nameWithType> クラスと完全に統合されています。 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> クラスの多くのコンストラクターは、<xref:System.Threading.CancellationToken> オブジェクトを入力パラメーターとして受け取ります。 <xref:System.Threading.Tasks.TaskFactory.StartNew%2A> および <xref:System.Threading.Tasks.Task.Run%2A> オーバーロードの多くも、<xref:System.Threading.CancellationToken> パラメーターを含みます。

<xref:System.Threading.CancellationTokenSource> クラスを使用すると、トークンを作成し、後でキャンセル要求を発行できます。 このトークンを <xref:System.Threading.Tasks.Task> に引数として渡し、同じトークンをキャンセル要求に応答するユーザー デリゲートで参照します。

詳細については、「[タスクのキャンセル](../../../docs/standard/parallel-programming/task-cancellation.md)」および「[方法: タスクとその子を取り消す](../../../docs/standard/parallel-programming/how-to-cancel-a-task-and-its-children.md)」を参照してください。

## <a name="the-taskfactory-class"></a>TaskFactory クラス

<xref:System.Threading.Tasks.TaskFactory> クラスには、タスクおよび継続タスクの作成と開始について、一般的なパターンをカプセル化する静的メソッドが用意されています。

- 最も一般的なパターンは、1 つのステートメントで 1 つのタスクを作成および開始する <xref:System.Threading.Tasks.TaskFactory.StartNew%2A> です。

- 複数の継続元から継続タスクを作成する場合は、<xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A> メソッドまたは <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A> メソッドを使用するか、<xref:System.Threading.Tasks.Task%601> クラスの同等のメソッドを使用します。 詳細については、「[継続タスクを使用したタスクの連結](../../../docs/standard/parallel-programming/chaining-tasks-by-using-continuation-tasks.md)」を参照してください。

- `BeginX` インスタンスまたは `EndX` インスタンスで非同期プログラミング モデルの <xref:System.Threading.Tasks.Task> メソッドおよび <xref:System.Threading.Tasks.Task%601> メソッドをカプセル化するには、<xref:System.Threading.Tasks.TaskFactory.FromAsync%2A> メソッドを使用します。 詳細については、「[TPL と従来の .NET Framework 非同期プログラミング](../../../docs/standard/parallel-programming/tpl-and-traditional-async-programming.md)」を参照してください。

既定の <xref:System.Threading.Tasks.TaskFactory> へは、<xref:System.Threading.Tasks.Task> クラスまたは <xref:System.Threading.Tasks.Task%601> クラス上の静的なプロパティとしてアクセスできます。 <xref:System.Threading.Tasks.TaskFactory> を直接インスタンス化し、さまざまなオプションを指定することもできます。たとえば、<xref:System.Threading.CancellationToken>、<xref:System.Threading.Tasks.TaskCreationOptions> オプション、<xref:System.Threading.Tasks.TaskContinuationOptions> オプション、<xref:System.Threading.Tasks.TaskScheduler> などです。 タスク ファクトリを作成するときに指定されるオプションは、タスク ファクトリで作成したすべてのタスクに適用されます。ただし、<xref:System.Threading.Tasks.Task> が <xref:System.Threading.Tasks.TaskCreationOptions> 列挙型を使用して作成された場合は例外で、タスクのオプションによってタスク ファクトリのオプションがオーバーライドされます。

## <a name="tasks-without-delegates"></a>デリゲートなしのタスク

<xref:System.Threading.Tasks.Task> を使用して、固有のユーザー デリゲートではなく外部コンポーネントによって実行される非同期操作をカプセル化する場合があります。 操作が非同期プログラミング モデルの Begin/End パターンに基づいている場合、<xref:System.Threading.Tasks.TaskFactory.FromAsync%2A> メソッドを使用できます。 そうでない場合は、<xref:System.Threading.Tasks.TaskCompletionSource%601> オブジェクトを使用して、タスク内の操作をラップして、<xref:System.Threading.Tasks.Task> を外部からプログラミング可能にする利点を活用できます。たとえば、例外の反映および継続のサポートです。 詳細については、<xref:System.Threading.Tasks.TaskCompletionSource%601> を参照してください。

## <a name="custom-schedulers"></a>カスタム スケジューラ

アプリケーションまたはライブラリのほとんどの開発者は、タスクを実行するプロセッサ、他のタスクと動作を同期する方法、<xref:System.Threading.ThreadPool?displayProperty=nameWithType> でスケジュールする方法などについては気にしません。 気にするのは、ホスト コンピューター上でできるだけ効率的に実行することだけです。 スケジュールの詳細についてより詳細に制御する必要がある場合、タスク並列ライブラリでは、既定のタスク スケジューラの設定を構成でき、さらにカスタム スケジューラを利用することもできます。 詳細については、<xref:System.Threading.Tasks.TaskScheduler> を参照してください。

## <a name="related-data-structures"></a>関連のデータ構造

TPL には、並列のシナリオおよび順次的なシナリオの両方に役立つ複数の新しいパブリック型があります。 これらの型には、<xref:System.Collections.Concurrent?displayProperty=nameWithType> 名前空間における、スレッド セーフで、高速、スケーラブルなコレクション クラス、および新しい同期の型が含まれます。たとえば、<xref:System.Threading.Semaphore?displayProperty=nameWithType> および <xref:System.Threading.ManualResetEventSlim?displayProperty=nameWithType> は、特定の種類の作業負荷に関しては、先行タスクより効率的です。 その他の .NET Framework 4 の新しい型には、<xref:System.Threading.Barrier?displayProperty=nameWithType> と <xref:System.Threading.SpinLock?displayProperty=nameWithType> があり、以前のリリースでは利用できなかった機能が用意されています。 詳細については、「[Data Structures for Parallel Programming](../../../docs/standard/parallel-programming/data-structures-for-parallel-programming.md)」を参照してください。

## <a name="custom-task-types"></a>カスタムのタスクの型

<xref:System.Threading.Tasks.Task?displayProperty=nameWithType> または <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> から継承しないことをお勧めします。 代わりに、<xref:System.Threading.Tasks.Task.AsyncState%2A> プロパティを使用して、追加のデータまたは状態を <xref:System.Threading.Tasks.Task> オブジェクトまたは <xref:System.Threading.Tasks.Task%601> オブジェクトに関連付けることをおすすめします。 拡張メソッドを使用して、<xref:System.Threading.Tasks.Task> クラスおよび <xref:System.Threading.Tasks.Task%601> クラスの機能を拡張することもできます。 拡張メソッドの詳細については、[拡張メソッド (C# プログラミングガイド)](../../csharp/programming-guide/classes-and-structs/extension-methods.md) と[拡張メソッド (Visual Basic)](../../visual-basic/programming-guide/language-features/procedures/extension-methods.md) に関する記事を参照してください。

<xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> から継承する必要がある場合、<xref:System.Threading.Tasks.Task.Run%2A>、<xref:System.Threading.Tasks.TaskFactory?displayProperty=nameWithType>、<xref:System.Threading.Tasks.TaskFactory%601?displayProperty=nameWithType>、<xref:System.Threading.Tasks.TaskCompletionSource%601?displayProperty=nameWithType> の各クラスを使用して、カスタムのタスクの型のインスタンスを作成することはできません。これらのクラスで作成されるのは、<xref:System.Threading.Tasks.Task> および <xref:System.Threading.Tasks.Task%601> オブジェクトだけであるためです。 また、<xref:System.Threading.Tasks.Task>、<xref:System.Threading.Tasks.Task%601>、<xref:System.Threading.Tasks.TaskFactory>、および <xref:System.Threading.Tasks.TaskFactory%601> で提供されるタスク継続機構でも、<xref:System.Threading.Tasks.Task> オブジェクトと <xref:System.Threading.Tasks.Task%601> オブジェクトしか作成されないため、これらの機構を使用してカスタムのタスクの型のインスタンスを作成することはできません。

## <a name="related-topics"></a>関連トピック

|タイトル|[説明]|
|-|-|
|[継続タスクを使用したタスクの連結](../../../docs/standard/parallel-programming/chaining-tasks-by-using-continuation-tasks.md)|継続の機能について説明します。|
|[アタッチされた子タスクとデタッチされた子タスク](../../../docs/standard/parallel-programming/attached-and-detached-child-tasks.md)|アタッチされた子タスクとデタッチされた子タスクの違いについて説明します。|
|[タスクのキャンセル](../../../docs/standard/parallel-programming/task-cancellation.md)|<xref:System.Threading.Tasks.Task> オブジェクトに組み込まれているキャンセルのサポートについて説明します。|
|[例外処理](../../../docs/standard/parallel-programming/exception-handling-task-parallel-library.md)|同時実行スレッド上の例外を処理する方法について説明します。|
|[方法: Parallel.Invoke を使用して並列操作を実行する](../../../docs/standard/parallel-programming/how-to-use-parallel-invoke-to-execute-parallel-operations.md)|<xref:System.Threading.Tasks.Parallel.Invoke%2A> の使用方法について説明します。|
|[方法: タスクから値を返す](../../../docs/standard/parallel-programming/how-to-return-a-value-from-a-task.md)|タスクから値を返す方法について説明します。|
|[方法: タスクとその子を取り消す](../../../docs/standard/parallel-programming/how-to-cancel-a-task-and-its-children.md)|タスクを取り消す方法について説明します。|
|[方法: 事前計算済みのタスクを作成する](../../../docs/standard/parallel-programming/how-to-create-pre-computed-tasks.md)|キャッシュに保持されている非同期ダウンロード操作の結果を取得する <xref:System.Threading.Tasks.Task.FromResult%2A?displayProperty=nameWithType> メソッドの使用例の詳細について説明します。|
|[方法: 並列タスクでバイナリ ツリーを走査する](../../../docs/standard/parallel-programming/how-to-traverse-a-binary-tree-with-parallel-tasks.md)|タスクを使用してバイナリ ツリーを走査する方法について説明します。|
|[方法: 入れ子のタスクのラップを解除する](../../../docs/standard/parallel-programming/how-to-unwrap-a-nested-task.md)|<xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> 拡張メソッドの使用方法を説明します。|
|[データの並列化](../../../docs/standard/parallel-programming/data-parallelism-task-parallel-library.md)|<xref:System.Threading.Tasks.Parallel.For%2A> および <xref:System.Threading.Tasks.Parallel.ForEach%2A> を使用してデータを対象に並列ループを作成する方法について説明しています。|
|[並列プログラミング](../../../docs/standard/parallel-programming/index.md)|.NET Framework 並列プログラミングのトップ レベル ノード。|

## <a name="see-also"></a>参照

- [並列プログラミング](../../../docs/standard/parallel-programming/index.md)
- [.NET Framework による並列プログラミングのサンプル](https://code.msdn.microsoft.com/Samples-for-Parallel-b4b76364)
