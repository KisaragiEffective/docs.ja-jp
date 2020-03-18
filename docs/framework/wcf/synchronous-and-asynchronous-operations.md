---
title: 同期操作と非同期操作
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- service contracts [WCF], synchronous operations
- service contracts [WCF], asynchronous operations
ms.assetid: db8a51cb-67e6-411b-9035-e5821ed350c9
ms.openlocfilehash: 143cc0f4566d86f1d42ebd11063f9af3c1ec331f
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802441"
---
# <a name="synchronous-and-asynchronous-operations"></a>同期操作と非同期操作
ここでは、非同期サービス操作の実装と呼び出しについて説明します。  
  
 多くのアプリケーションは、メソッド呼び出しの実行中に有用な処理を続行できるように、メソッドを非同期的に呼び出します。 Windows Communication Foundation (WCF) のサービスとクライアントは、アプリケーションの異なる 2 つのレベルで操作の非同期呼び出しに参加できます。これにより、WCF アプリケーションの柔軟性がさらに高まり、対話機能とのバランスの取れたスループットを最大限に高めることができます。  
  
## <a name="types-of-asynchronous-operations"></a>非同期操作の種類  
 WCF のすべてのサービス コントラクトでは、パラメーターの型と戻り値に関係なく、WCF の属性を使用して、クライアントとサービス間の特定のメッセージ交換パターンを指定します。 WCF は、適切なサービス操作または実行元のクライアント コードに送受信メッセージを自動的にルーティングします。  
  
 クライアントが所有するのは、特定操作のメッセージ交換パターンが指定されたサービス コントラクトのみです。 基盤となるメッセージ交換パターンに従っている限り、クライアントは選択する任意のプログラミング モデルを開発者に提供できます。 同様に、サービスも、指定されたメッセージ パターンに従っている限り、任意の方法で操作を実装できます。  
  
 サービス コントラクトがサービスとクライアントの両方の実装から独立していることで、WCF アプリケーションでは次の形式の非同期実行が可能になります。  
  
- クライアントは、同期メッセージ交換を使用して要求/応答操作を非同期に呼び出すことができます。  
  
- サービスは、同期メッセージ交換を使用して要求/応答操作を非同期に実装できます。  
  
- クライアントまたはサービスの実装に関係なく、一方向のメッセージ交換が可能です。  
  
### <a name="suggested-asynchronous-scenarios"></a>推奨される非同期シナリオ  
 操作のサービス実装でブロッキング呼び出し (I/O 処理の実行など) を作成する場合は、サービス操作の実装で非同期手法を使用します。 非同期操作を実装している場合、非同期の操作とメソッドを呼び出して、できるだけ非同期呼び出しパスを拡張するようにします。 たとえば、`BeginOperationTwo()` 内から `BeginOperationOne()` を呼び出します。  
  
- 次のような場合には、クライアントまたは呼び出し元アプリケーションで非同期手法を使用します。  
  
- 中間層アプリケーションから操作を呼び出す場合 (このようなシナリオの詳細については、「[中間層クライアント アプリケーション](./feature-details/middle-tier-client-applications.md)」を参照してください)。  
  
- ASP.NET ページ内で操作を呼び出す場合、非同期ページを使用します。  
  
- シングル スレッドのアプリケーション (Windows フォームや Windows Presentation Foundation (WPF) など) から操作を呼び出す場合。 イベント ベースの非同期呼び出しモデルを使用すると、結果イベントが UI スレッドで発生するので複数のスレッドを独自に処理する必要がなく、アプリケーションの応答性が向上します。  
  
- 一般に、同期呼び出しと非同期呼び出しのいずれかを選択する場合は、非同期呼び出しを選択します。  
  
### <a name="implementing-an-asynchronous-service-operation"></a>非同期サービス操作の実装  
 非同期操作は、次の 3 つの方法のいずれかを使用して実装できます。  
  
1. タスク ベースの非同期パターン  
  
2. イベント ベースの非同期パターン  
  
3. IAsyncResult 非同期パターン  
  
#### <a name="task-based-asynchronous-pattern"></a>タスク ベースの非同期パターン  
 非同期操作を実装する方法としては、最も直接的でわかりやすいタスク ベースの非同期パターンが推奨されます。 この方法を使用するには、単にサービス操作を実装し、戻り値の型 Task\<T> (T は論理操作によって返される型) を指定します。 例:  
  
```csharp  
public class SampleService:ISampleService   
{   
   // ...  
   public async Task<string> SampleMethodTaskAsync(string msg)   
   {   
      return Task<string>.Factory.StartNew(() =>   
      {   
         return msg;   
      });   
   }  
   // ...  
}  
```  
  
 SampleMethodTaskAsync 操作は、論理操作によって文字列が返されるため、Task\<string> を返します。 タスク ベースの非同期パターンの詳細については、「[タスク ベースの非同期パターン](https://go.microsoft.com/fwlink/?LinkId=232504)」を参照してください。  
  
> [!WARNING]
> タスク ベースの非同期パターンを使用している場合、操作の完了を待機している間に例外が発生すると T:System.AggregateException がスローされる場合があります。 この例外は、クライアントまたはサービスで発生することがあります。  
  
#### <a name="event-based-asynchronous-pattern"></a>イベント ベースの非同期パターン  
 イベント ベースの非同期パターンをサポートするサービスには、MethodNameAsync という名前の操作が 1 つ以上含まれます。 これらのメソッドは、同期バージョンに対応するもので、現在のスレッドで同じ操作を行います。 クラスには、MethodNameCompleted イベントや MethodNameAsyncCancel メソッド (または単に CancelAsync メソッド) が含まれる場合もあります。 操作の呼び出し元のクライアントは、操作の完了時に呼び出されるイベント ハンドラーを定義します。  
  
 次のコードは、イベント ベースの非同期パターンを使用して非同期操作を宣言する方法を示します。  
  
```csharp  
public class AsyncExample  
{  
    // Synchronous methods.  
    public int Method1(string param);  
    public void Method2(double param);  
  
    // Asynchronous methods.  
    public void Method1Async(string param);  
    public void Method1Async(string param, object userState);  
    public event Method1CompletedEventHandler Method1Completed;  
  
    public void Method2Async(double param);  
    public void Method2Async(double param, object userState);  
    public event Method2CompletedEventHandler Method2Completed;  
  
    public void CancelAsync(object userState);  
  
    public bool IsBusy { get; }  
  
    // Class implementation not shown.  
}  
```  
  
 イベント ベースの非同期パターンの詳細については、「[イベント ベースの非同期パターンの概要](../../standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-overview.md)」を参照してください。  
  
#### <a name="iasyncresult-asynchronous-pattern"></a>IAsyncResult 非同期パターン  
 サービス操作は、.NET Framework 非同期プログラミングパターンを使用して非同期方式で実装し、`<Begin>` メソッドに <xref:System.ServiceModel.OperationContractAttribute.AsyncPattern%2A> プロパティを `true`に設定してマークすることができます。 この場合の非同期操作は、同期操作と同じ形式でメタデータに公開されます。つまり、要求メッセージとそれに関連する応答メッセージを伴う単独操作として公開されます。 このとき、クライアント プログラミング モデルは選択が可能です。 サービスが呼び出されたときに要求/応答メッセージ交換が行われていれば、クライアント プログラミング モデルは、このパターンを同期操作または非同期操作として表すことができます。  
  
 一般に、システムの非同期の性質を考えると、スレッドへの依存は避ける必要があります。  操作のディスパッチ処理のさまざまな段階にデータを渡す最も信頼性の高い方法は、拡張機能を使用する方法です。  
  
 例については、「[方法 : 非同期サービス操作を実装する](how-to-implement-an-asynchronous-service-operation.md)」を参照してください。  
  
 クライアント アプリケーションでの呼び出し方法に関係なく、非同期に実行するコントラクト操作 `X` を定義するには次の処理を実行します。  
  
- パターン `BeginOperation` と `EndOperation` を使用する 2 つのメソッドを定義します。  
  
- `BeginOperation` メソッドには操作のための `in` パラメーターと `ref` パラメーターがあり、<xref:System.IAsyncResult> 型を返します。  
  
- `EndOperation` メソッドには <xref:System.IAsyncResult> パラメーター、`out` パラメーター、および `ref` パラメーターがあり、操作の戻り値の型を返します。  
  
 たとえば、次のメソッドを参照してください。  
  
```csharp  
int DoWork(string data, ref string inout, out string outonly)  
```  
  
```vb  
Function DoWork(ByVal data As String, ByRef inout As String, _out outonly As out) As Integer  
```  
  
 非同期操作を作成するには、2 つのメソッドを次のように記述します。  
  
```csharp  
[OperationContract(AsyncPattern=true)]
IAsyncResult BeginDoWork(string data,
                         ref string inout,
                         AsyncCallback callback,
                         object state);
int EndDoWork(ref string inout, out string outonly, IAsyncResult result);  
```  
  
```vb  
<OperationContract(AsyncPattern := True)>
Function BeginDoWork(ByVal data As String, _
                     ByRef inout As String, _
                     ByVal callback As AsyncCallback, _
                     ByVal state As Object) As IAsyncResult
Function EndDoWork(ByRef inout As String, ByRef outonly As String, ByVal result As IAsyncResult) As Integer  
```  
  
> [!NOTE]
> <xref:System.ServiceModel.OperationContractAttribute> 属性は、`BeginDoWork` のメソッドにのみ適用されます。 生成されるコントラクトには、`DoWork` という名前の WSDL 操作が 1 つ含まれます。  
  
### <a name="client-side-asynchronous-invocations"></a>クライアント側の非同期呼び出し  
 WCF クライアント アプリケーションでは、既に説明した 3 つの非同期呼び出しモデルをどれでも使用できます。  
  
 タスク ベースのモデルを使用する場合は、次のコードに示すように、await キーワードを使用して操作を呼び出すだけです。  
  
```csharp  
await simpleServiceClient.SampleMethodTaskAsync("hello, world");  
```  
  
 イベント ベースの非同期パターンを使用する場合は、応答の通知を受信するイベント ハンドラーを追加するだけで済み、結果イベントはユーザー インターフェイス スレッドで自動的に発生します。 この手法を使用するには、次の例に示すように、[ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) で **/async** と **/tcv:Version35** の両方のコマンド オプションを指定します。  
  
```console  
svcutil http://localhost:8000/servicemodelsamples/service/mex /async /tcv:Version35  
```  
  
 これらのオプションを指定すると、Svcutil.exe によって、イベント インフラストラクチャを持つ WCF クライアント クラスが生成されます。このインフラストラクチャにより、呼び出し元アプリケーションは、応答を受信して適切なアクションを実行するイベント ハンドラーを実装し、割り当てることができます。 詳細な例については、「[方法 : サービス操作を非同期に呼び出す](./feature-details/how-to-call-wcf-service-operations-asynchronously.md)」を参照してください。  
  
 ただし、イベントベースの非同期モデルは、.NET Framework 3.5 でのみ使用できます。 また、<xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType>を使用して WCF クライアントチャネルを作成した場合、.NET Framework 3.5 でもサポートされません。 WCF クライアント チャネル オブジェクトを使用する場合、<xref:System.IAsyncResult?displayProperty=nameWithType> オブジェクトを使用して操作を非同期に呼び出す必要があります。 この手法を使用するには、次の例に示すように、[ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) で **/async** コマンド オプションを指定します。  
  
```console  
svcutil http://localhost:8000/servicemodelsamples/service/mex /async   
```  
  
 これにより、対応する `<Begin>` メソッドを持ち、<xref:System.ServiceModel.OperationContractAttribute.AsyncPattern%2A> プロパティが `true` に設定された `<End>` メソッドとして各操作がモデル化されたサービス コントラクトが生成されます。 <xref:System.ServiceModel.ChannelFactory%601> の詳細な使用例については、「[方法 : チャネル ファクトリを使用して、非同期的に操作を呼び出す](./feature-details/how-to-call-operations-asynchronously-using-a-channel-factory.md)」を参照してください。  
  
 いずれの場合も、サービスが同期的に実装されていても、アプリケーションは操作を非同期に呼び出すことができます。これは、アプリケーションで同じパターンを使用してローカルの同期メソッドを非同期に呼び出す場合と同様です。 操作の実装方法は、クライアントにとって重要ではありません。応答メッセージが到着すると、その内容がクライアントの非同期 <`End`> メソッドにディスパッチされ、クライアントが情報を取得します。  
  
### <a name="one-way-message-exchange-patterns"></a>一方向メッセージ交換パターン  
 クライアントまたはサービスが単独で一方向操作 (<xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A?displayProperty=nameWithType> が `true` に設定されている操作には相関する応答がない) を相手側に送信できる、非同期メッセージ交換パターンを作成することもできます (これにより、双方向メッセージ交換パターンが一方向メッセージで使用されます)。この場合、サービスコントラクトは、両側が非同期呼び出しまたは実装として実装できる一方向メッセージ交換を指定します。または、必要に応じて指定します。 一般的に、コントラクトが一方向メッセージ交換の場合、メッセージが送信されるとアプリケーションは応答を待機することなく他の作業を継続できるため、多くの場合、実装が非同期になります。  
  
### <a name="event-based-asynchronous-clients-and-message-contracts"></a>イベント ベースの非同期クライアントとメッセージ コントラクト  
 イベント ベースの非同期モデルのデザイン ガイドラインには、複数の値を返す場合に、1 つの値を `Result` プロパティとして返し、残りの値を <xref:System.EventArgs> オブジェクトのプロパティとして返すことが記載されています。 この 1 つの結果として、クライアントがイベント ベースの非同期コマンド オプションを使用してメタデータをインポートし、操作から複数の値が返される場合、既定の <xref:System.EventArgs> オブジェクトは 1 つの値を `Result` プロパティとして返し、残りの値は <xref:System.EventArgs> オブジェクトのプロパティになります。  
  
 メッセージ オブジェクトを `Result` プロパティとして受け取り、返された値をそのオブジェクトのプロパティとして取得する場合は、 **/messageContract** コマンド オプションを使用します。 これにより、`Result` オブジェクトの <xref:System.EventArgs> プロパティとして応答メッセージを返すシグネチャが生成されます。 すべての内部戻り値は、応答メッセージ オブジェクトのプロパティになります。  
  
## <a name="see-also"></a>参照

- <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A>
- <xref:System.ServiceModel.OperationContractAttribute.AsyncPattern%2A>
