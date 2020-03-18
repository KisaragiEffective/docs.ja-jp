---
title: 相関関係のトラブルシューティング
ms.date: 03/30/2017
ms.assetid: 98003875-233d-4512-a688-4b2a1b0b5371
ms.openlocfilehash: be48a55a87d199829de4038e7e2a7642c102acf2
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73976020"
---
# <a name="troubleshooting-correlation"></a>相関関係のトラブルシューティング
相関関係は、ワークフロー サービス メッセージを互いに関連付けたり、正しいワークフロー インスタンスに関連付けたりするために使用されますが、正しく構成されていないと、メッセージが受信されず、アプリケーションが正しく機能しなくなります。 ここでは、相関関係のトラブルシューティングのいくつかの方法の概要と、相関関係を使用するときに発生する一般的な問題について説明します。

## <a name="handle-the-unknownmessagereceived-event"></a>UnknownMessageReceived イベントの処理
 <xref:System.ServiceModel.ServiceHostBase.UnknownMessageReceived> イベントは、サービスで不明なメッセージ (既存のインスタンスに関連付けることができないメッセージなど) が受信されたときに発生します。 自己ホスト型サービスでは、このイベントをホスト アプリケーションで処理できます。

```csharp
host.UnknownMessageReceived += delegate(object sender, UnknownMessageReceivedEventArgs e)
{
    Console.WriteLine("Unknown Message Received:");
    Console.WriteLine(e.Message);
};
```

 Web ホスト サービスでこのイベントを処理するには、まず、<xref:System.ServiceModel.Activities.Activation.WorkflowServiceHostFactory> の派生クラスを作成し、<xref:System.ServiceModel.Activities.Activation.WorkflowServiceHostFactory.CreateWorkflowServiceHost%2A> をオーバーライドします。

```csharp
class CustomFactory : WorkflowServiceHostFactory
{
    protected override WorkflowServiceHost CreateWorkflowServiceHost(Activity activity, Uri[] baseAddresses)
    {
        // Create the WorkflowServiceHost.
        WorkflowServiceHost host = new WorkflowServiceHost(activity, baseAddresses);

        // Handle the UnknownMessageReceived event.
        host.UnknownMessageReceived += delegate(object sender, UnknownMessageReceivedEventArgs e)
        {
            Console.WriteLine("Unknown Message Received:");
            Console.WriteLine(e.Message);
        };

        return host;
    }
}
```

 次に、そのカスタムの <xref:System.ServiceModel.Activities.Activation.WorkflowServiceHostFactory> をサービスの `svc` ファイルで指定します。

`<% @ServiceHost Language="C#" Service="OrderServiceWorkflow" Factory="CustomFactory" %>`

 このハンドラーが呼び出されると、<xref:System.ServiceModel.UnknownMessageReceivedEventArgs.Message%2A> の <xref:System.ServiceModel.UnknownMessageReceivedEventArgs> プロパティを使用して次のようなメッセージを取得できます。

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <To s:mustUnderstand="1" xmlns="http://schemas.microsoft.com/ws/2005/05/addressing/none">http://localhost:8080/OrderService</To>
    <Action s:mustUnderstand="1" xmlns="http://schemas.microsoft.com/ws/2005/05/addressing/none">http://tempuri.org/IService/AddItem</Action>
  </s:Header>
  <s:Body>
    <AddItem xmlns="http://tempuri.org/">
      <Item>Books</Item>
    </AddItem>
  </s:Body>
</s:Envelope>
```

 <xref:System.ServiceModel.ServiceHostBase.UnknownMessageReceived> ハンドラーにディスパッチされたメッセージを調べることで、メッセージがワークフロー サービスのインスタンスに関連付けられなかった理由の手がかりを得られる可能性があります。

## <a name="use-tracking-to-monitor-the-progress-of-the-workflow"></a>追跡によるワークフローの進行状況の監視
 追跡を使用すると、ワークフローの進行状況を監視できます。 既定では、ワークフローのライフサイクル イベント、アクティビティのライフサイクル イベント、エラー伝達、およびブックマーク再開に対して追跡レコードが出力されます。 そのほかに、カスタム アクティビティでカスタム追跡レコードを出力することもできます。 相関関係のトラブルシューティングでは、アクティビティ追跡レコード、ブックマーク再開レコード、およびエラー伝達レコードが最も役立ちます。 アクティビティ追跡レコードを使用すると、ワークフローの現在の進行状況を確認したり、現在メッセージを待機しているメッセージング アクティビティを特定したりすることができます。 ブックマーク再開レコードを使用すると、ワークフローでメッセージが受信されたかどうかを確認できます。エラー伝達レコードは、ワークフローで発生したエラーの記録を提供します。 追跡を有効にするには、<xref:System.Activities.Tracking.TrackingParticipant> の <xref:System.ServiceModel.Activities.WorkflowServiceHost.WorkflowExtensions%2A> で目的の <xref:System.ServiceModel.Activities.WorkflowServiceHost> を指定します。 次の例では、([カスタム追跡](../../../../docs/framework/windows-workflow-foundation/samples/custom-tracking.md)サンプルからの) `ConsoleTrackingParticipant` が既定の追跡プロファイルを使用して構成されています。

```csharp
host.WorkflowExtensions.Add(new ConsoleTrackingParticipant());
```

 ConsoleTrackingParticipant などの追跡参加要素は、コンソール ウィンドウを持つ自己ホスト型ワークフロー サービスで使用できます。 Web ホストサービスの場合、組み込みの <xref:System.Activities.Tracking.EtwTrackingParticipant>、または情報をファイルに記録するカスタムの追跡参加要素など、追跡情報を永続ストアに記録する追跡参加要素を使用する必要があります。

 Web ホストワークフローサービスの追跡および追跡の構成の詳細については、「[ワークフローの追跡とトレース](../../../../docs/framework/windows-workflow-foundation/workflow-tracking-and-tracing.md)」、「[ワークフローの追跡の構成](../../../../docs/framework/windows-workflow-foundation/configuring-tracking-for-a-workflow.md)」、および「ワークフローの[追跡&#91;&#93; ](../../../../docs/framework/windows-workflow-foundation/samples/tracking.md) 」のサンプルを参照してください。

## <a name="use-wcf-tracing"></a>WCF トレースの使用
 WCF トレースは、ワークフロー サービスのメッセージ フローのトレースを提供します。 このトレース情報は、相関関係 (特にコンテンツ ベースの相関関係) のトラブルシューティングに役立ちます。 トレースを有効にするには、`system.diagnostics` ファイル (Web ホスト ワークフロー サービスの場合) または `web.config` ファイル (自己ホスト型ワークフロー サービスの場合) の `app.config` セクションで目的のトレース リスナーを指定します。 トレース ファイルにメッセージの内容を含めるには、`true` の `logEntireMessage` セクションの `messageLogging` 要素で `diagnostics` に対して `system.serviceModel` を指定します。 次の例では、メッセージの内容を含むトレース情報を `service.svclog` という名前のファイルに書き込むように構成しています。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.diagnostics>
    <sources>
      <source name="System.ServiceModel" switchValue="Information" propagateActivity="true">
        <listeners>
          <add name="corr"/>
        </listeners>
      </source>
      <source name="System.ServiceModel.MessageLogging">
        <listeners>
          <add name="corr"/>
        </listeners>
      </source>
    </sources>

    <sharedListeners>
      <add name="corr" type="System.Diagnostics.XmlWriterTraceListener" initializeData="c:\logs\service.svclog">
      </add>
    </sharedListeners>
  </system.diagnostics>

  <system.serviceModel>
    <diagnostics>
      <messageLogging logEntireMessage="true" logMalformedMessages="false"
         logMessagesAtServiceLevel="false" logMessagesAtTransportLevel="true" maxSizeOfMessageToLog="2147483647">
      </messageLogging>
    </diagnostics>
  </system.serviceModel>
</configuration>
```

 `service.svclog`に含まれているトレース情報を表示するには、[サービストレースビューアーツール (svctraceviewer.exe)](../../../../docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md)を使用します。 この方法を使用すると、渡されたメッセージの内容そのものを調べてコンテンツ ベースの相関関係の <xref:System.ServiceModel.CorrelationQuery> に一致するかどうかを確認できるため、コンテンツ ベースの相関関係のトラブルシューティングで特に便利です。 WCF トレースの詳細については、「[サービストレースビューアーツール (svctraceviewer.exe)](../../../../docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md)」、「[トレースの構成](../../../../docs/framework/wcf/diagnostics/tracing/configuring-tracing.md)」、および「トレースを使用した[アプリケーションのトラブルシューティング](../../../../docs/framework/wcf/diagnostics/tracing/using-tracing-to-troubleshoot-your-application.md)」を参照してください。

## <a name="common-context-exchange-correlation-issues"></a>コンテキスト交換の相関関係の一般的な問題
 相関関係の中には、特定の種類のバインドが使用されていないと正しく機能しないものがあります。 たとえば、要求/応答の相関関係には <xref:System.ServiceModel.BasicHttpBinding> などの双方向のバインドが、コンテキスト交換の相関関係には <xref:System.ServiceModel.BasicHttpContextBinding> などのコンテキスト ベースのバインドが必要です。 双方向の操作はほとんどのバインドでサポートされているため、要求/応答の相関関係ではあまり問題になりませんが、コンテキスト ベースのバインドはわずかしかなく (<xref:System.ServiceModel.BasicHttpContextBinding>、<xref:System.ServiceModel.WSHttpContextBinding>、<xref:System.ServiceModel.NetTcpContextBinding> など)、 それらのいずれかが使用されていないと、ワークフロー サービスへの最初の呼び出しは成功しますが、その後の呼び出しが次の <xref:System.ServiceModel.FaultException> で失敗します。

```output
There is no context attached to the incoming message for the service
and the current operation is not marked with "CanCreateInstance = true".
In order to communicate with this service check whether the incoming binding
supports the context protocol and has a valid context initialized.
```

 コンテキスト相関関係に使用されるコンテキスト情報は、双方向の操作が使用されている場合は <xref:System.ServiceModel.Activities.SendReply> から (コンテキスト相関関係を初期化する) <xref:System.ServiceModel.Activities.Receive> アクティビティに返され、操作が一方向の場合は呼び出し元によって指定されます。 コンテキストが呼び出し元によって送信されず、ワークフロー サービスからも返されない場合は、後続の操作が呼び出されたときに上述の <xref:System.ServiceModel.FaultException> が返されます。

## <a name="common-request-reply-correlation-issues"></a>要求/応答の相関関係の一般的な問題
 要求/応答の相関関係は、<xref:System.ServiceModel.Activities.Receive>/<xref:System.ServiceModel.Activities.SendReply> ペアと共に使用して、ワークフローサービスに双方向の操作を実装し、別の Web サービスで双方向の操作を実行する <xref:System.ServiceModel.Activities.Send>/<xref:System.ServiceModel.Activities.ReceiveReply> のペアと共に使用します。 WCF サービスで双方向の操作を呼び出す場合、サービスは、従来の命令型のコードベースの WCF サービスにすることも、ワークフローサービスにすることもできます。 要求/応答の相関関係を使用するには、<xref:System.ServiceModel.BasicHttpBinding> などの双方向のバインドを使用する必要があります。操作が双方向である必要もあります。

 ワークフローサービスに双方向の操作がある場合、または重複する <xref:System.ServiceModel.Activities.Receive>/<xref:System.ServiceModel.Activities.SendReply> または <xref:System.ServiceModel.Activities.Send>/<xref:System.ServiceModel.Activities.ReceiveReply> のペアの場合は、<xref:System.ServiceModel.Activities.WorkflowServiceHost> によって提供される暗黙の相関関係ハンドル管理では十分ではない可能性があります。特に高負荷のシナリオでは、メッセージが正しくルーティングされない可能性があります。 この問題が発生しないようにするために、要求/応答の相関関係を使用する際には常に <xref:System.ServiceModel.Activities.CorrelationHandle> を明示的に指定することをお勧めします。 ワークフローデザイナーの **ツールボックス** の メッセージング セクションから**sendandreceivereply**テンプレートと**receiveandsendreply**テンプレートを使用すると、既定では <xref:System.ServiceModel.Activities.CorrelationHandle> が明示的に構成されます。 コードを使用してワークフローを作成する場合は、ペアの最初のアクティビティの <xref:System.ServiceModel.Activities.CorrelationHandle> で <xref:System.ServiceModel.Activities.Receive.CorrelationInitializers%2A> を指定します。 次の例では、<xref:System.ServiceModel.Activities.Receive> で <xref:System.ServiceModel.Activities.CorrelationInitializer.CorrelationHandle%2A> を明示的に指定して <xref:System.ServiceModel.Activities.RequestReplyCorrelationInitializer> アクティビティを構成しています。

```csharp
Variable<CorrelationHandle> RRHandle = new Variable<CorrelationHandle>();

Receive StartOrder = new Receive
{
    CanCreateInstance = true,
    ServiceContractName = OrderContractName,
    OperationName = "StartOrder",
    CorrelationInitializers =
    {
        new RequestReplyCorrelationInitializer
        {
            CorrelationHandle = RRHandle
        }
    }
};

SendReply ReplyToStartOrder = new SendReply
{
    Request = StartOrder,
    Content = ... // Contains the return value, if any.
};

// Construct a workflow using StartOrder and ReplyToStartOrder.
```

 永続化は、<xref:System.ServiceModel.Activities.Receive>/<xref:System.ServiceModel.Activities.SendReply> ペアまたは <xref:System.ServiceModel.Activities.Send>/<xref:System.ServiceModel.Activities.ReceiveReply> ペア間では許可されていません。 両方のアクティビティが完了するまで存続する非永続化ゾーンが作成されます。 遅延アクティビティなどのアクティビティがこの非永続化ゾーンにあって、ワークフローのアイドル状態を引き起こした場合、ホストでワークフローがアイドル状態になったときにワークフローを永続化するようにホストが構成されていても、ワークフローは永続化されません。 Persist アクティビティなどのアクティビティが非永続化ゾーンで明示的に永続化を試みた場合、致命的な例外がスローされ、ワークフローが中止されて、<xref:System.ServiceModel.FaultException> が呼び出し元に返されます。 次の致命的な例外メッセージが表示されます。"System.InvalidOperationException: 持続性のないブロックに Persist アクティビティを含めることはできません。" この例外は呼び出し元に返されませんが、追跡が有効になっている場合は確認できます。 呼び出し元に返される <xref:System.ServiceModel.FaultException> のメッセージは、"WorkflowInstance '5836145b-7da2-49d0-a052-a49162adeab6' が完了しているため、操作を実行できませんでした" です。

 要求/応答の相関関係の詳細については、「[要求-応答](../../../../docs/framework/wcf/feature-details/request-reply-correlation.md)」を参照してください。

## <a name="common-content-correlation-issues"></a>コンテンツ ベースの相関関係の一般的な問題
 コンテンツ ベースの相関関係は、ワークフロー サービスが複数のメッセージを受信し、交換されるメッセージ内の一部のデータによって目的のインスタンスが識別される場合に使用されます。 コンテンツ ベースの相関関係では、顧客番号や注文 ID などのメッセージ内のデータを使用して、適切なワークフロー インスタンスにメッセージをルーティングします。 ここでは、コンテンツ ベースの相関関係を使用するときに発生するいくつかの一般的な問題について説明します。

### <a name="ensure-the-identifying-data-is-unique"></a>識別データが一意であることの確認
 インスタンスの識別に使用されるデータは、相関関係キーにハッシュされます。 相関関係による関連付けで使用するデータは必ず、一意にする必要があります。一意でない場合は、ハッシュされたキーで競合が発生し、誤った場所にメッセージがルーティングされる可能性があります。 たとえば、顧客名だけに基づいた関連付けでは、同じ名前の顧客が複数存在する場合があるため、競合が発生する可能性があります。 メッセージの関連付けに使用するデータの一部として、コロン (:) を使用することはできません。コロンは、メッセージ クエリのキーと値の区切り文字として既に使用されており、後でハッシュされる文字列に含まれるためです。 永続化を使用している場合は、現在の識別データが、以前に永続化されたインスタンスによって使用されていないかどうかを確認する必要があります。 この問題は、永続化を一時的に無効にすることによって特定できます。 WCF トレースを使用すると、計算された相関関係キーを表示できるため、この種の問題のデバッグに役立ちます。

### <a name="race-conditions"></a>競合状態
 サービスがメッセージを受信してから実際に相関関係が初期化されるまでにはわずかな時間差があり、その間は後続のメッセージが無視されます。 ワークフロー サービスで、クライアントから一方向の操作で渡されるデータを使用してコンテンツ ベースの相関関係が初期化される場合に、呼び出し元が後続のメッセージをすぐに送信すると、この時間差の間それらのメッセージが無視されます。 この問題を回避するには、双方向の操作を使用して相関関係を初期化するか、<xref:System.ServiceModel.Activities.TransactedReceiveScope> を使用します。

### <a name="correlation-query-issues"></a>関連付けクエリについての問題
 関連付けクエリは、メッセージの関連付けに使用するメッセージ内のデータを指定するために使用されます。 このデータは、XPath クエリを使用して指定します。 特に問題がないように見えるにもかかわらず、サービスへのメッセージがディスパッチされない場合は、トラブルシューティングの手法として、メッセージ データの値に一致するリテラル値を XPath クエリの代わりに指定することができます。 リテラル値を指定するには `string` 関数を使用します。 次の例では、<xref:System.ServiceModel.MessageQuerySet> に対して `11445` というリテラル値を使用するように `OrderId` が構成されています。XPath クエリはコメント アウトされています。

```csharp
MessageQuerySet = new MessageQuerySet
{
    {
        "OrderId",
        //new XPathMessageQuery("sm:body()/tempuri:StartOrderResponse/tempuri:OrderId")
        new XPathMessageQuery("string('11445')")
    }
}
```

 関連付けデータが取得されないなど、XPath クエリが正しく構成されていない場合、次のメッセージと共にエラーが返されます。"関連付けクエリで空の結果セットが生成されました。 エンドポイントの関連付けクエリが正しく構成されていることを確認してください。" 前のセクションで説明したとおり、XPath クエリをリテラル値と置換すると、すばやくトラブルシューティングすることができます。 この問題は、**関連付け初期化子の追加** または  **correlateson の定義** ダイアログボックスで XPath クエリビルダーを使用していて、ワークフローサービスでメッセージコントラクトが使用されている場合に発生する可能性があります。 次の例では、メッセージ コントラクト クラスを定義します。

```csharp
[MessageContract]
public class AddItemMessage
{
    [MessageHeader]
    public string CartId;

    [MessageBodyMember]
    public string Item;
}
```

 このメッセージ コントラクトはワークフローの <xref:System.ServiceModel.Activities.Receive> アクティビティで使用されます。 メッセージのヘッダーにある `CartId` は、メッセージを正しいインスタンスに関連付けるために使用されます。 `CartId` を取得する XPath クエリがワークフロー デザイナーの関連付けダイアログを使用して作成される場合、次の正しくないクエリが生成されます。

```
sm:body()/xg0:AddItemMessage/xg0:CartId
```

 <xref:System.ServiceModel.Activities.Receive> アクティビティがデータのパラメーターを使用した場合、この XPath クエリは正しいですが、メッセージ コントラクトを使用しているため、誤りとなります。 次の XPath クエリがヘッダーから `CartId` を取得するために正しい XPath クエリです。

```
sm:header()/tempuri:CartId
```

これはメッセージの本文を調べて確認することができます。

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <Action s:mustUnderstand="1" xmlns="http://schemas.microsoft.com/ws/2005/05/addressing/none">http://tempuri.org/IService/AddItem</Action>
    <h:CartId xmlns:h="http://tempuri.org/">80c95b41-c98d-4660-a6c1-99412206e54c</h:CartId>
  </s:Header>
  <s:Body>
    <AddItemMessage xmlns="http://tempuri.org/">
      <Item>Books</Item>
    </AddItemMessage>
  </s:Body>
</s:Envelope>
```

次の例では、データを受信するために前のメッセージ コントラクトを使用する <xref:System.ServiceModel.Activities.Receive> 操作のために構成された `AddItem` アクティビティを示します。 XPath クエリは正しく構成されています。

```xaml
<Receive CorrelatesWith="[CCHandle] OperationName="AddItem" ServiceContractName="p:IService">
  <Receive.CorrelatesOn>
    <XPathMessageQuery x:Key="key1">
      <XPathMessageQuery.Namespaces>
        <ssx:XPathMessageContextMarkup>
          <x:String x:Key="xg0">http://schemas.datacontract.org/2004/07/MessageContractWFService</x:String>
        </ssx:XPathMessageContextMarkup>
      </XPathMessageQuery.Namespaces>sm:header()/tempuri:CartId</XPathMessageQuery>
  </Receive.CorrelatesOn>
  <ReceiveMessageContent DeclaredMessageType="m:AddItemMessage">
    <p1:OutArgument x:TypeArguments="m:AddItemMessage">[AddItemMessage]</p1:OutArgument>
  </ReceiveMessageContent>
</Receive>
```
