---
title: 操作フォーマッタと操作セレクター
ms.date: 03/30/2017
ms.assetid: 1c27e9fe-11f8-4377-8140-828207b98a0e
ms.openlocfilehash: 64f2d807946d5365c01cd1a46488c868ebc603ac
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74714641"
---
# <a name="operation-formatter-and-operation-selector"></a>操作フォーマッタと操作セレクター
このサンプルでは、Windows Communication Foundation (WCF) 拡張ポイントを使用して、WCF で想定されるものとは異なる形式のメッセージデータを許可する方法を示します。 既定では、WCF フォーマッタは、メソッドパラメーターを `soap:body` 要素の下に含めることを想定しています。 このサンプルでは、代わりに HTTP GET クエリ文字列のパラメータ データを解析するカスタム操作フォーマッタを実装し、そのデータを使用してメソッドを呼び出す方法を示します。  
  
 このサンプルは、`ICalculator` サービスコントラクトを実装する[はじめに](../../../../docs/framework/wcf/samples/getting-started-sample.md)に基づいています。 加算、減算、乗算、および除算のメッセージを変更して、クライアントからサーバーへの要求を行うための HTTP GET と、サーバーからクライアントへの要求を行うための POX メッセージ付きの HTTP POST を使用する方法を示します。  
  
 これを行うために、サンプルには次の機能が用意されています。  
  
- `QueryStringFormatter`。クライアントとサーバー用に、それぞれ <xref:System.ServiceModel.Dispatcher.IClientMessageFormatter> と <xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter> を実装し、クエリ文字列内のデータを処理します。  
  
- `UriOperationSelector`。サーバー上に、GET 要求内の操作名に基づいて操作ディスパッチを実行する <xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector> を実装します。  
  
- `EnableHttpGetRequestsBehavior` エンドポイント動作 (および対応する構成)。必要な操作セレクタをランタイムに追加します。  
  
- 新しい操作フォーマッタをランタイムに挿入する方法を示します。  
  
- このサンプルでは、クライアントとサービスは両方ともコンソール アプリケーション (.exe) です。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
## <a name="key-concepts"></a>主要な概念  
 `QueryStringFormatter`-操作フォーマッタは WCF のコンポーネントであり、メッセージをパラメーターオブジェクトの配列に変換し、パラメーターオブジェクトの配列をメッセージに変換します。 この操作は、クライアント上では <xref:System.ServiceModel.Dispatcher.IClientMessageFormatter> インターフェイスを使用して、サーバー上では <xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter> インターフェイスを使用して行われます。 これらのインターフェイスを使用すると、ユーザーは `Serialize` メソッドと `Deserialize` メソッドから要求メッセージと応答メッセージを取得できます。  
  
 このサンプルでは、`QueryStringFormatter` にこれら両方のインターフェイスが実装され、クライアントとサーバーで実装されます。  
  
 要求 :  
  
- このサンプルでは、<xref:System.ComponentModel.TypeConverter> クラスを使用して要求メッセージ内のパラメータ データを文字列に変換したり、その逆の処理を行ったりします。 <xref:System.ComponentModel.TypeConverter> が特定の型で使用できない場合は、サンプルのフォーマッタから例外がスローされます。  
  
- クライアントの `IClientMessageFormatter.SerializeRequest` メソッドでは、フォーマッタは適切な宛先アドレスを使用して URI を作成し、操作名をサフィックスとして追加します。 この操作名は、サーバー上の適切な操作へのディスパッチに使用されます。 次にパラメータ名と <xref:System.ComponentModel.TypeConverter> によって変換された値を使用して、パラメータ オブジェクトの配列を取得し、そのパラメータ データを URI クエリ文字列にシリアル化します。 <xref:System.ServiceModel.Channels.MessageHeaders.To%2A> プロパティおよび <xref:System.ServiceModel.Channels.MessageProperties.Via%2A> プロパティは、この URI に設定されます。 <xref:System.ServiceModel.Channels.MessageProperties> にアクセスするには、<xref:System.ServiceModel.Channels.Message.Properties%2A> プロパティを使用します。  
  
- サーバーの `IDispatchMessageFormatter.DeserializeRequest` メソッドでは、フォーマッタは受信要求メッセージ プロパティ内で`Via` URI を検索します。 次に URI クエリ文字列内にある名前と値の組み合わせを解析してパラメータ名と値に分け、そのパラメータ名と値を使用してこのメソッドに渡されるパラメータの配列を設定します。 操作ディスパッチは既に発生しているので、このメソッドでは操作名のサフィックスは無視されます。  
  
 応答 :  
  
- このサンプルでは、HTTP GET は要求のみに使用されます。 応答を送信するには、フォーマッタを XML メッセージの生成時に使用できたはずの最初のフォーマッタに代行させます。 このサンプルの目的の 1 つは、このような代行フォーマッタの実装方法を示すことです。  
  
### <a name="uripathsuffixoperationselector-class"></a>UriPathSuffixOperationSelector クラス  
 <xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector> インターフェイスを使用すると、特定のメッセージをディスパッチする必要がある操作で、ユーザー独自のロジックを実装できます。  
  
 このサンプルでは、操作名がメッセージのアクション ヘッダーではなく、HTTP GET URI に含まれているため、適切な操作を選択するには、`UriPathSuffixOperationSelector` がサーバーに実装されている必要があります。 サンプルは、大文字小文字が区別されない操作名のみを許可するようにセットアップされています。  
  
 `SelectOperation` メソッドは、受信メッセージを取得し、メッセージ プロパティ内の `Via` URI を検索します。 次に、この URI から操作名のサフィックスを抽出し、内部テーブルを検索してメッセージをディスパッチする必要がある操作名を取得して、その操作名を返します。  
  
### <a name="enablehttpgetrequestsbehavior-class"></a>EnableHttpGetRequestsBehavior クラス  
 `UriPathSuffixOperationSelector` コンポーネントは、プログラムまたはエンドポイント動作を使用してセットアップできます。 サンプルでは、サービスのアプリケーション構成ファイルで指定された `EnableHttpGetRequestsBehavior` の動作を実装します。  
  
 サーバー側 :  
  
 <xref:System.ServiceModel.Dispatcher.DispatchRuntime.OperationSelector%2A> は <xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector> 実装に設定されます。  
  
 既定では、WCF は完全一致アドレスフィルターを使用します。 受信メッセージ上の URI には、操作名のサフィックスと、その後にパラメータ データを格納するクエリ文字列が含まれています。したがって、アドレス フィルタは、エンドポイントの動作によっても、プレフィックスが一致するフィルタに変更されます。 このためには、WCF<xref:System.ServiceModel.Dispatcher.PrefixEndpointAddressMessageFilter> を使用します。  
  
### <a name="installing-operation-formatters"></a>操作フォーマッタのインストール  
 フォーマッタを指定する操作の動作は一意です。 このような動作は、常に既定で操作ごとに実装され、必要な操作フォーマッタを作成します。 ただし、これらの動作は他の操作動作と同様に見えるため、他の属性では識別できません。 置換動作をインストールするには、既定で WCF 型ローダーによってインストールされている特定のフォーマッタの動作を検索し、それを置き換えるか、既定の動作の後に実行する互換性のある動作を追加する必要があります。  
  
 これらの操作フォーマッタの動作は、<xref:System.ServiceModel.Channels.CommunicationObject.Open%2A?displayProperty=nameWithType> を呼び出す前にプログラムでセットアップするか、または既定のフォーマッタ動作後に実行される操作動作を指定することによってセットアップすることができます。 ただし、エンドポイント動作を介したセットアップ (および構成を介したセットアップ) は容易ではありません。この動作モデルでは、動作によって別の動作を置き換えることはできず、他の方法で説明ツリーを変更することもできないためです。  
  
 クライアント側 :  
  
 <xref:System.ServiceModel.Dispatcher.IClientMessageFormatter> の実装を行って、要求を HTTP GET 要求に変換し、応答を元のフォーマッタに代行させる必要があります。 これを行うには、`EnableHttpGetRequestsBehavior.ReplaceFormatterBehavior` ヘルパー メソッドを呼び出します。  
  
 この手順は、`CreateChannel` を呼び出す前に行う必要があります。  
  
```csharp  
void ReplaceFormatterBehavior(OperationDescription operationDescription, EndpointAddress address)  
{  
    // Remove the DataContract behavior if it is present.  
    IOperationBehavior formatterBehavior = operationDescription.Behaviors.Remove<DataContractSerializerOperationBehavior>();  
    if (formatterBehavior == null)  
    {  
        // Remove the XmlSerializer behavior if it is present.  
        formatterBehavior = operationDescription.Behaviors.Remove<XmlSerializerOperationBehavior>();  
        ...  
    }  
  
    // Remember what the innerFormatterBehavior was.  
    DelegatingFormatterBehavior delegatingFormatterBehavior = new DelegatingFormatterBehavior(address);  
    delegatingFormatterBehavior.InnerFormatterBehavior = formatterBehavior;  
   operationDescription.Behaviors.Add(delegatingFormatterBehavior);  
}  
```  
  
 サーバー側 :  
  
- <xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter> インターフェイスを実装して、HTTP GET 要求を読み込み、応答の書き出しを元のフォーマッタに代行させる必要があります。 これを行うには、クライアントと同じ`EnableHttpGetRequestsBehavior.ReplaceFormatterBehavior` ヘルパー メソッドを呼び出します (前述のコード サンプルを参照)。  
  
- この手順は、<xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> を呼び出す前に行う必要があります。 このサンプルでは、<xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> を呼び出す前にこのフォーマッタを手動で変更する方法を示します。 同じ結果を得るための別の方法としては、オープン操作の前に <xref:System.ServiceModel.ServiceHost> を呼び出す `EnableHttpGetRequestsBehavior.ReplaceFormatterBehavior` からクラスを派生します (ホスティングのドキュメントおよびサンプル例を参照してください)。  
  
### <a name="user-experience"></a>ユーザー側の表示と操作  
 サーバー側 :  
  
- サーバーの `ICalculator` 実装は、変更する必要はありません。  
  
- サービス用の App.config では、`messageVersion` 要素の `textMessageEncoding` 属性を `None` に設定するカスタムの POX バインディングを使用する必要があります。  
  
    ```xml  
    <bindings>  
      <customBinding>  
        <binding name="poxBinding">  
          <textMessageEncoding messageVersion="None" />  
          <httpTransport />  
        </binding>  
      </customBinding>  
    </bindings>  
    ```  
  
- サービス用の App.config ではさらに、カスタムの `EnableHttpGetRequestsBehavior` を動作の拡張セクションに追加して使用することによって、これを指定する必要があります。  
  
    ```xml  
    <behaviors>  
      <endpointBehaviors>  
        <behavior name="enableHttpGetRequestsBehavior">  
          <enableHttpGetRequests />  
        </behavior>  
      </endpointBehaviors>  
    </behaviors>  
  
    <extensions>  
      <behaviorExtensions>  
        <!-- Enabling HTTP GET requests: Behavior Extension -->  
        <add   
          name="enableHttpGetRequests"           type="Microsoft.ServiceModel.Samples.EnableHttpGetRequestsBehaviorElement, QueryStringFormatter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />  
      </behaviorExtensions>  
    </extensions>  
    ```  
  
- <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> を呼び出す前に操作フォーマッタを追加します。  
  
 クライアント側 :  
  
- クライアントの実装は、変更する必要はありません。  
  
- クライアント用の App.config では、`messageVersion` 要素の `textMessageEncoding` 属性を `None` に設定するカスタムの POX バインディングを使用する必要があります。 サービスとの違いは、クライアントでは送信先アドレスを変更できるように手動アドレス指定を有効にする必要があるという点です。  
  
    ```xml  
    <bindings>  
      <customBinding>  
        <binding name="poxBinding">  
          <textMessageEncoding messageVersion="None" />  
          <httpTransport manualAddressing="True" />  
        </binding>  
      </customBinding>  
    </bindings>  
    ```  
  
- クライアント用の App.config では、サーバーと同じカスタムの `EnableHttpGetRequestsBehavior` を指定する必要があります。  
  
- <xref:System.ServiceModel.ChannelFactory%601.CreateChannel> を呼び出す前に操作フォーマッタを追加します。  
  
 このサンプルを実行すると、操作要求および応答がクライアントのコンソール ウィンドウに表示されます。 4 つすべての操作 (加算、減算、乗算、および除算) が正常に行われる必要があります。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Formatters\QueryStringFormatter`  
  
##### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
