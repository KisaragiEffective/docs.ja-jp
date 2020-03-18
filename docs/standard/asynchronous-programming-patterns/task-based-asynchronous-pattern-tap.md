---
title: タスク ベースの非同期パターン (TAP)
ms.date: 02/26/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- .NET Framework, and TAP
- asynchronous design patterns, .NET Framework
- TAP, .NET Framework support for
- Task-based Asynchronous Pattern, .NET Framework support for
- .NET Framework, asynchronous design patterns
ms.assetid: 8cef1fcf-6f9f-417c-b21f-3fd8bac75007
ms.openlocfilehash: 89c486618729c334bf74f0a1f4f9dd1b3cee8b0e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78158169"
---
# <a name="task-based-asynchronous-pattern-tap"></a>タスク ベースの非同期パターン (TAP)
タスク ベースの非同期パターン (TAP) は、任意の非同期操作を表すために使用される <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 名前空間の <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> 型および <xref:System.Threading.Tasks?displayProperty=nameWithType> 型に基づいています。 TAP は、新規開発に推奨の非同期デザイン パターンです。  
  
## <a name="naming-parameters-and-return-types"></a>名前付け、パラメーター、および戻り値の型

TAP では、非同期操作の開始と終了を表すために単一のメソッドが使用されます。 これは、非同期プログラミング モデル (APM または `IAsyncResult`) パターンとイベントベースの非同期パターン (EAP) の両方とは対照的です。 APM では、`Begin` と `End` メソッドが必要です。 EAP では、`Async` サフィックスを持つメソッドが必要であり、1 つ以上のイベント、イベント ハンドラー デリゲート型、および `EventArg` 派生型も必要です。 TAP の非同期メソッドには、待機可能な型を返すメソッドの操作名の後ろに `Async` サフィックスが含まれます (<xref:System.Threading.Tasks.Task>、<xref:System.Threading.Tasks.Task%601>、<xref:System.Threading.Tasks.ValueTask>、<xref:System.Threading.Tasks.ValueTask%601> など)。 たとえば、`Get` を返す非同期の `Task<String>` 操作を `GetAsync` と名付けることができます。 既に `Async` サフィックスの付いた EAP メソッド名を含むクラスに TAP メソッドを追加する場合は、代わりに `TaskAsync` サフィックスを使用します。 たとえば、既にクラスに `GetAsync` メソッドが含まれている場合は、`GetTaskAsync` という名前を使用します。 メソッドによって非同期操作が開始されるが、待機可能な型が返らない場合は、その名前を `Begin`、`Start`、またはこのメソッドが操作の結果を返したりスローしたりしないことを示すその他の動詞で始める必要があります。  
  
 対応する同期メソッドにより void または <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 型が返されるかどうかに応じて、TAP メソッドは <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> または `TResult` を返します。  
  
 TAP メソッドのパラメーターには、対応する同期メソッドと同じパラメーターを、同じ順序で指定する必要があります。  ただし、`out` パラメーターと `ref` パラメーターはこの規則に該当せず、すべて回避する必要があります。 `out` パラメーターまたは `ref` パラメーターで返されるデータは、代わりに複数の値を格納するために、タプルまたはカスタム データ構造を使用して、`TResult` により返される <xref:System.Threading.Tasks.Task%601> の一部として返す必要があります。 さらに、TAP メソッドに対応する同期メソッドでは提供されていない場合でも、<xref:System.Threading.CancellationToken> パラメーターの追加を検討する必要があります。

 タスクの作成、操作、または組み合わせのためだけに使用されるメソッド (メソッド名またはメソッドが属する型の名前でメソッドの非同期の意図が明確な場合) は、この名前付けパターンに従う必要はありません。このようなメソッドは、*連結子*と呼ばれることもあります。 連結子の例には、<xref:System.Threading.Tasks.Task.WhenAll%2A> および <xref:System.Threading.Tasks.Task.WhenAny%2A> があります。詳細については、記事「[タスク ベースの非同期パターンの利用](../../../docs/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md#combinators)」の「[タスク ベースの組み込み連結子の使用](../../../docs/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md)」セクションを参照してください。  
  
 非同期プログラミング モデル (APM) やイベント ベースの非同期パターン (EAP) など、従来の非同期プログラミング パターンで使用される構文とは異なる TAP 構文の例については、「[非同期プログラミングのパターン](../../../docs/standard/asynchronous-programming-patterns/index.md)」を参照してください。  
  
## <a name="initiating-an-asynchronous-operation"></a>非同期操作の開始  
 TAP に基づく非同期メソッドは、引数の検証や非同期操作の開始などの少量の作業を同期をとって実行してから結果のタスクを返すことができます。 このような同期作業は必要最低限にし、非同期メソッドからすぐに制御を戻すようにします。 制御をすぐに戻す理由は次のとおりです。  
  
- 非同期メソッドはユーザー インターフェイス (UI) スレッドから呼び出される可能性があるため、同期作業の実行に時間がかかると、アプリケーションの応答性が低下します。  
  
- 複数の非同期メソッドが同時に起動される可能性があります。 そのため、非同期メソッドの同期部分の作業に時間がかかると、他の非同期操作の開始が遅れ、コンカレンシーの利点が低減します。  
  
 場合によっては、操作の完了に必要な作業の量は、操作を非同期に起動するのに必要な作業量よりも少なくなります。 このようなシナリオの例にはストリームからの読み取りがあり、既にメモリ バッファーにあるデータを読み取ることで読み取り操作が完了する場合です。 このような場合は、操作を同期をとって実行し、既に完了しているタスクを返すことができます。  
  
## <a name="exceptions"></a>例外  
 非同期メソッドは、使用エラーに応答して非同期メソッド呼び出しからスローされる例外のみを発生する必要があります。 運用コードでは使用エラーを発生させないようにする必要があります。 たとえば、null 参照 (Visual Basic では `Nothing`) がメソッドの引数の 1 つとして渡されたときにエラー状態 (通常 <xref:System.ArgumentNullException> によって表される) が発生する場合、呼び出し元のコードを変更して null 参照が渡されないようにします。 他のエラーの場合はすべて、非同期メソッドの実行中に発生する例外を、返されるタスクに割り当てます。これは、タスクが返される前に非同期メソッドが同期をとって行われる場合でも同じです。 通常、タスクに含まれる例外は最大でも 1 つです。 ただし、タスクが複数の操作 (<xref:System.Threading.Tasks.Task.WhenAll%2A> など) を表す場合は、複数の例外を単一タスクに関連付けることができます。  
  
## <a name="target-environment"></a>ターゲット環境  
 TAP メソッドを実装するときに、非同期実行をどこで行うかを決定できます。 スレッド プールで作業負荷を実行したり、(操作実行のほとんどでスレッドにバインドされないように) 非同期 I/O を使用して実装したり、特定のスレッド (UI スレッドなど) で実行したり、任意の数の可能なコンテキストを使用したりすることができます。 TAP メソッドは何も実行せず、システムの他の場所で発生した何らかの条件を表す <xref:System.Threading.Tasks.Task> を返すのみの場合もあります (待ち行列データ構造に届いたデータを表すタスクなど)。

 TAP メソッドの呼び出し元は、結果的に生成されるタスクに同期的に応答することで、TAP メソッドの完了を待つことをブロックできます。あるいは、非同期操作の完了時に追加 (継続) コードを実行できます。 継続コードの作成者は、そのコードが実行される場所を制御できます。 継続コードは、<xref:System.Threading.Tasks.Task> クラス (<xref:System.Threading.Tasks.Task.ContinueWith%2A> など) のメソッドによって明示的に作成するか、継続の上位にビルドされる言語サポート (C# の `await`、Visual Basic の `Await`、F# の `AwaitValue` など) を使用して暗黙のうちに作成できます。  
  
## <a name="task-status"></a>タスクの状態  
 <xref:System.Threading.Tasks.Task> クラスは、非同期操作の有効期間を提供し、そのサイクルは、<xref:System.Threading.Tasks.TaskStatus> 列挙型によって表されます。 <xref:System.Threading.Tasks.Task> および <xref:System.Threading.Tasks.Task%601> から派生する型のコーナー ケースに加え、スケジューリングからの構造の分離をサポートするために、<xref:System.Threading.Tasks.Task> クラスは <xref:System.Threading.Tasks.Task.Start%2A> メソッドを公開します。 <xref:System.Threading.Tasks.Task> パブリック コンストラクターにより作成されるタスクは、ライフ サイクルがスケジュールされていない *状態から始まり、これらのインスタンスで* が呼び出されるときにのみスケジュールされることから、<xref:System.Threading.Tasks.TaskStatus.Created>コールド タスク<xref:System.Threading.Tasks.Task.Start%2A>と呼ばれます。

 他のすべてのタスクは、ホットな状態からライフ サイクルが始まります。つまり、タスクが表す非同期操作が既に開始され、それらのタスクの状態は <xref:System.Threading.Tasks.TaskStatus.Created?displayProperty=nameWithType> 以外の列挙値であることを意味します。 TAP メソッドから返されるすべてのタスクをアクティブにする必要があります。 **TAP メソッドが返すタスクをインスタンス化するためにタスクのコンストラクターを内部使用する場合、TAP メソッドはタスクを返す前に <xref:System.Threading.Tasks.Task.Start%2A> オブジェクトで <xref:System.Threading.Tasks.Task> を呼び出す必要があります。** TAP メソッドのコンシューマーは、返されたタスクがアクティブであるものと推定しても問題はなく、TAP メソッドから返された <xref:System.Threading.Tasks.Task.Start%2A> 上で <xref:System.Threading.Tasks.Task> 呼び出しを試行しないようにする必要があります。 アクティブなタスク上で <xref:System.Threading.Tasks.Task.Start%2A> を呼び出すと、<xref:System.InvalidOperationException> 例外になります。  
  
## <a name="cancellation-optional"></a>取り消し (省略可能)  
 TAP では、取り消しは非同期メソッドの実装側とコンシューマーのどちらでも省略可能です。 操作の取り消しを許可する場合、キャンセル トークン (<xref:System.Threading.CancellationToken> インスタンス) を受け取る非同期メソッドのオーバーロードを公開します。 規則により、パラメーターには `cancellationToken` という名前が付けられます。  
  
 [!code-csharp[Conceptual.TAP#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap/cs/examples1.cs#1)]
 [!code-vb[Conceptual.TAP#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap/vb/examples1.vb#1)]  
  
 非同期操作は取り消し要求に対してこのトークンを監視します。 取り消し要求を受け取ると、その要求を受け入れて操作を取り消すことができます。 取り消し要求によって作業が途中で終了する場合、TAP メソッドは <xref:System.Threading.Tasks.TaskStatus.Canceled> 状態で終了するタスクを返します。使用できる結果はなく、例外もスローされません。  <xref:System.Threading.Tasks.TaskStatus.Canceled> 状態は、<xref:System.Threading.Tasks.TaskStatus.Faulted> 状態や <xref:System.Threading.Tasks.TaskStatus.RanToCompletion> 状態と共に、タスクの最終状態 (完了状態) と見なされます。 したがって、タスクが <xref:System.Threading.Tasks.TaskStatus.Canceled> 状態の場合、<xref:System.Threading.Tasks.Task.IsCompleted%2A> プロパティは `true` を返します。 タスクが <xref:System.Threading.Tasks.TaskStatus.Canceled> 状態で完了した場合、<xref:System.Threading.Tasks.TaskContinuationOptions.NotOnCanceled> など、継続のオプションを継続から除外するよう指定されていない限り、タスクに登録された継続がスケジュールまたは実行されます。 言語機能を使用して取り消されたタスクを非同期に待機するコードは実行を継続しますが、<xref:System.OperationCanceledException> またはその派生例外を受け取ります。 <xref:System.Threading.Tasks.Task.Wait%2A> や <xref:System.Threading.Tasks.Task.WaitAll%2A> などのメソッドによってタスクでの同期をとって待機している状態をブロックされたコードも、例外を伴って実行を継続します。  
  
 キャンセル トークンが、トークンを受け取る TAP メソッドが呼び出される前に取り消しを要求していた場合、TAP メソッドは <xref:System.Threading.Tasks.TaskStatus.Canceled> タスクを返す必要があります。  ただし、非同期操作の実行中に取り消し要求が出される場合、その非同期操作は取り消し要求を受け取る必要はありません。  取り消し要求の結果として操作が完了した場合にのみ、返されたタスクが <xref:System.Threading.Tasks.TaskStatus.Canceled> 状態で終了します。 取り消しが要求されても、結果 (例外) が依然として生成される場合、タスクは <xref:System.Threading.Tasks.TaskStatus.RanToCompletion> 状態または <xref:System.Threading.Tasks.TaskStatus.Faulted> 状態で終了します。

 何よりもまずキャンセルできる機能を公開することが望まれる非同期メソッドの場合、キャンセル トークンを受け取らないオーバーロードを用意する必要はありません。 取り消せないメソッドの場合、キャンセル トークンを受け取るオーバーロードを用意しません。これにより、ターゲット メソッドが実際に取り消し可能かどうかを呼び出し元に示すことができます。  取り消しを望まないコンシューマー コードは、<xref:System.Threading.CancellationToken> を受け取るメソッドを呼び出し、引数値として <xref:System.Threading.CancellationToken.None%2A> を指定することができます。 <xref:System.Threading.CancellationToken.None%2A> は、既定の <xref:System.Threading.CancellationToken> と機能的には同じです。  
  
## <a name="progress-reporting-optional"></a>進行状況のレポート (省略可能)  
 一部の非同期操作では、進行状況の通知を行うことで利点が得られます。進行状況の通知は、通常、非同期操作の進行状況に関する情報でユーザー インターフェイスを更新するために使用されます。

 TAP では、進行状況が、通常 <xref:System.IProgress%601> という名前のパラメーターとして非同期メソッドに渡される `progress` インターフェイスによって処理されます。  非同期メソッドの呼び出し時に進行状況インターフェイスを指定することで、不適切な使用方法により発生する競合状態 (操作の開始後に不適切に登録されたイベント ハンドラーで更新を検出できない場合) を排除できます。  さらに重要なのは、コンシューマー コードの判断に応じて、進行状況インターフェイスがさまざまな実装方法の進行状況をサポートできるようにすることです。  たとえば、コンシューマー コードが最新の進行状況の更新のみに留意する場合、すべての更新をバッファーに格納することを望む場合、各更新の操作を呼び出すことを望む場合、呼び出しを特定のスレッドにマーシャリングするかどうかの制御を望む場合が考えられます。 これらはすべて、インターフェイスの異なる実装を使用して行うことができ、各実装を特定のコンシューマー ニーズに合わせてカスタマイズできます。  取り消しと同様、TAP の実装では、API が進行状況通知をサポートする場合にのみ、<xref:System.IProgress%601> パラメーターを指定する必要があります。

 たとえば、前に説明した `ReadAsync` メソッドが読み取り済みのバイト数の形式で中間進行状況を報告できる場合、進行状況のコールバックは <xref:System.IProgress%601> インターフェイスとなることが考えられます。  
  
 [!code-csharp[Conceptual.TAP#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap/cs/examples1.cs#2)]
 [!code-vb[Conceptual.TAP#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap/vb/examples1.vb#2)]  
  
 `FindFilesAsync` メソッドが特定の検索パターンに合ったすべてのファイルのリストを返す場合は、進行状況のコールバックで、完了した作業の割合の見積りに加え、現在の部分的な結果セットを示すことが考えられます。  これは、タプルで行ったり、  
  
 [!code-csharp[Conceptual.TAP#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap/cs/examples1.cs#3)]
 [!code-vb[Conceptual.TAP#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap/vb/examples1.vb#3)]  
  
 または次のように API 固有のデータ型で行うことができます。  
  
 [!code-csharp[Conceptual.TAP#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.tap/cs/examples1.cs#4)]
 [!code-vb[Conceptual.TAP#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.tap/vb/examples1.vb#4)]  
  
 後者の場合、特別なデータ型には通常 `ProgressInfo` サフィックスを付けます。  
  
 TAP の実装が `progress` パラメーターを受け取るオーバーロードを用意している場合、`null` の引数を許可し、進行状況を報告しないことを許可する必要があります。 TAP の実装は、<xref:System.Progress%601> オブジェクトに進行状況を同期的に報告して、非同期メソッドが速やかに進行状況を提供し、進行状況のコンシューマーが、最適な情報の処理方法と処理場所を判断できるようにします。 たとえば、進行状況のインスタンスはコールバックをマーシャリングし、キャプチャされた同期コンテキストでイベントを発生するように選択することができます。  
  
## <a name="iprogresst-implementations"></a>IProgress\<T> の実装  
 .NET Framework 4.5 では、単一の <xref:System.IProgress%601> implementation: <xref:System.Progress%601> が実施されます。 <xref:System.Progress%601> クラスは次のように宣言されます。  
  
```csharp  
public class Progress<T> : IProgress<T>  
{  
    public Progress();  
    public Progress(Action<T> handler);  
    protected virtual void OnReport(T value);  
    public event EventHandler<T> ProgressChanged;  
}  
```  
  
```vb  
Public Class Progress(Of T) : Inherits IProgress(Of T)  
    Public Sub New()  
    Public Sub New(handler As Action(Of T))  
    Protected Overridable Sub OnReport(value As T)  
    Public Event ProgressChanged As EventHandler(Of T>  
End Class  
```  
  
 <xref:System.Progress%601> のインスタンスは、非同期操作が進行状況の更新を報告するたびに発生する <xref:System.Progress%601.ProgressChanged> イベントを公開します。 <xref:System.Progress%601.ProgressChanged> イベントは、<xref:System.Threading.SynchronizationContext> インスタンスがインスタンス化されたときにキャプチャされた <xref:System.Progress%601> オブジェクトで発生します。 同期コンテキストを利用できない場合は、スレッド プールをターゲットとして、既定のコンテキストが使用されます。 ハンドラーは、このイベントに登録することができます。 1 つのハンドラーは、利便性のために <xref:System.Progress%601> コンストラクターにも提供でき、<xref:System.Progress%601.ProgressChanged> イベントのイベント ハンドラーと同様に作動します。 進行状況の更新は、イベント ハンドラーの実行中、非同期操作を遅延しないように、非同期に発生します。 別のセマンティクスを適用するため、別の <xref:System.IProgress%601> の実装を選択できます。  
  
## <a name="choosing-the-overloads-to-provide"></a>提供するオーバーロードの選択  
 ともに省略可能な <xref:System.Threading.Tasks.TaskFactory.CancellationToken%2A> パラメーターと <xref:System.IProgress%601> パラメーターの両方を TAP の実装に使用すると、4 つまでオーバーロードを要求することができます。  
  
```csharp  
public Task MethodNameAsync(…);  
public Task MethodNameAsync(…, CancellationToken cancellationToken);  
public Task MethodNameAsync(…, IProgress<T> progress);
public Task MethodNameAsync(…,
    CancellationToken cancellationToken, IProgress<T> progress);  
```  
  
```vb  
Public MethodNameAsync(…) As Task  
Public MethodNameAsync(…, cancellationToken As CancellationToken cancellationToken) As Task  
Public MethodNameAsync(…, progress As IProgress(Of T)) As Task
Public MethodNameAsync(…, cancellationToken As CancellationToken,
                       progress As IProgress(Of T)) As Task  
```  
  
 ただし、TAP の実装の多くは取り消しまたは進行状況の機能を提供しないため、必要なメソッドは 1 つです。  
  
```csharp  
public Task MethodNameAsync(…);  
```  
  
```vb  
Public MethodNameAsync(…) As Task  
```  
  
 TAP の実装で取り消しまたは進行状況の両方ではなく、いずれかを一方をサポートする場合は、実装に 2 つのオーバーロードを提供することができます。  
  
```csharp  
public Task MethodNameAsync(…);  
public Task MethodNameAsync(…, CancellationToken cancellationToken);  
  
// … or …  
  
public Task MethodNameAsync(…);  
public Task MethodNameAsync(…, IProgress<T> progress);  
```  
  
```vb  
Public MethodNameAsync(…) As Task  
Public MethodNameAsync(…, cancellationToken As CancellationToken) As Task  
  
' … or …  
  
Public MethodNameAsync(…) As Task  
Public MethodNameAsync(…, progress As IProgress(Of T)) As Task  
```  
  
 TAP の実装で取り消しおよび進行状況の両方をサポートする場合は、4 種類のオーバーロードをすべて公開できます。 ただし、次の 2 種類のみ提供することもできます。  
  
```csharp  
public Task MethodNameAsync(…);  
public Task MethodNameAsync(…,
    CancellationToken cancellationToken, IProgress<T> progress);  
```  
  
```vb  
Public MethodNameAsync(…) As Task  
Public MethodNameAsync(…, cancellationToken As CancellationToken,
                       progress As IProgress(Of T)) As Task  
```  
  
 不足する 2 種類の中間の組み合わせを補足するために、開発者は <xref:System.Threading.CancellationToken.None%2A> パラメーターに <xref:System.Threading.CancellationToken> または既定の `cancellationToken` を渡したり、`null` パラメーターに `progress` を渡すことができます。  
  
 TAP メソッドに必ず取り消しや進行状況をサポートすることを想定する場合、必要なパラメーターを受け取らないオーバーロードは省略できます。  
  
 TAP メソッドの複数のオーバーロードを公開して取り消しや進行状況を省略可能にする場合、取り消しや進行状況をサポートしないオーバーロードは、これらをサポートするオーバーロードに、取り消しの場合は <xref:System.Threading.CancellationToken.None%2A>、進行状況の場合は `null` が渡されたように機能する必要があります。  
  
## <a name="related-topics"></a>関連トピック  
  
|タイトル|[説明]|  
|-----------|-----------------|  
|[非同期プログラミングのパターン](../../../docs/standard/asynchronous-programming-patterns/index.md)|非同期操作を実行するための 3 種類のパターンとして、タスク ベースの非同期パターン (TAP)、非同期プログラミング モデル (APM)、およびイベント ベースの非同期パターン (EAP) を紹介します。|  
|[タスク ベースの非同期パターンの実装](../../../docs/standard/asynchronous-programming-patterns/implementing-the-task-based-asynchronous-pattern.md)|タスク ベースの非同期パターン (TAP) の実装の 3 つの方法として、Visual Studio の C# および Visual Basic コンパイラを使用する方法、手動で行う方法、またはコンパイラと手動による方法を組み合わせた方法を説明します。|  
|[タスク ベースの非同期パターンの利用](../../../docs/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md)|タスクとコールバックを使用して、ブロックすることなく待機できる方法を説明します。|  
|[他の非同期パターンと型との相互運用](../../../docs/standard/asynchronous-programming-patterns/interop-with-other-asynchronous-patterns-and-types.md)|タスク ベースの非同期パターン (TAP) を使用して、非同期プログラミング モデル (APM) とイベント ベースの非同期パターン (EAP) を実装する方法について説明します。|
