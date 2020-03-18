---
title: カスタム メッセージ インターセプター
ms.date: 03/30/2017
ms.assetid: 73f20972-53f8-475a-8bfe-c133bfa225b0
ms.openlocfilehash: 53005212bc834d73ab5cbb4545d1477112f29c75
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74716814"
---
# <a name="custom-message-interceptor"></a>カスタム メッセージ インターセプター
このサンプルでは、チャネル拡張モデルの使用方法を示します。 特に、チャネル ファクトリとチャネル リスナーを作成するカスタム バインド要素を実装して、ランタイム スタックの特定のポイントですべての送受信メッセージを中断する方法を示します。 また、このサンプルには、こうしたカスタム ファクトリの使用方法を示すクライアントとサーバーも含まれます。  
  
 このサンプルでは、クライアントとサービスは両方ともコンソール プログラム (.exe) です。 そして、これらのクライアントとサービスの両方で、カスタム バインド要素およびこれに関連付けられたランタイム オブジェクトを含む共通ライブラリ (.dll) を使用します。  
  
> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Channels\MessageInterceptor`  
  
 このサンプルでは、チャネルフレームワークと次の WCF のベストプラクティスに従って、Windows Communication Foundation (WCF) でカスタム層チャネルを作成するための推奨手順について説明します。 カスタム階層チャネルを作成する手順は、次のとおりです。  
  
1. どのチャネル形状をチャネル ファクトリおよびチャネル リスナがサポートするかを決定します。  
  
2. チャネル形状をサポートするチャネル ファクトリおよびチャネル リスナを作成します。  
  
3. チャネル スタックにカスタム階層チャネルを追加するバインド要素を追加します。  
  
4. バインド要素拡張セクションを追加して、新しいバインド要素を構成システムに公開します。  
  
## <a name="channel-shapes"></a>チャネル形状  
 カスタム階層チャネルを記述する最初の手順として、どの形状がチャネルに必要かを判断します。 メッセージ インスペクタの場合、下のレイヤでサポートされる任意の形状をサポートします (たとえば、下のレイヤで <xref:System.ServiceModel.Channels.IOutputChannel> と <xref:System.ServiceModel.Channels.IDuplexSessionChannel> が作成できる場合は、<xref:System.ServiceModel.Channels.IOutputChannel> と <xref:System.ServiceModel.Channels.IDuplexSessionChannel> を公開します)。  
  
## <a name="channel-factory-and-listener-factory"></a>チャネル ファクトリとリスナ ファクトリ  
 カスタム階層チャネルを記述する次の手順では、クライアント チャネルでの <xref:System.ServiceModel.Channels.IChannelFactory> の実装とサービス チャネルでの <xref:System.ServiceModel.Channels.IChannelListener> の実装を作成します。  
  
 これらのクラスでは、内部ファクトリとリスナが取得され、この内部ファクトリとリスナが `OnCreateChannel` と `OnAcceptChannel` 以外のすべての呼び出しを代行します。  
  
```csharp
class InterceptingChannelFactory<TChannel> : ChannelFactoryBase<TChannel>  
{ 
    //... 
}

class InterceptingChannelListener<TChannel> : ListenerFactoryBase<TChannel>  
{ 
    //...
}  
```  
  
## <a name="adding-a-binding-element"></a>バインド要素の追加  
 このサンプルでは、`InterceptingBindingElement` というカスタム バインド要素を定義します。 `InterceptingBindingElement` は `ChannelMessageInterceptor` を入力として受け取り、この `ChannelMessageInterceptor` を使用してメッセージを通過するメッセージを操作します。 公開する必要があるクラスはこれだけです。 ファクトリ、リスナー、およびチャネルは、いずれもパブリックなランタイム インターフェイスの内部的な実装にすることができます。  
  
```csharp
public class InterceptingBindingElement : BindingElement
{
}
```  
  
## <a name="adding-configuration-support"></a>構成サポートの追加  
 バインディング構成と統合するには、ライブラリで、構成セクション ハンドラをバインディング要素拡張セクションとして定義します。 クライアントとサーバーの構成ファイルでは、バインド要素拡張を構成システムに登録する必要があります。 バインディング要素を構成システムに公開する実装は、このクラスから派生できます。  
  
```csharp
public abstract class InterceptingElement : BindingElementExtensionElement 
{ 
    //... 
}
```  
  
## <a name="adding-policy"></a>ポリシーの追加  
 ポリシー システムと統合するには、`InterceptingBindingElement` に IPolicyExportExtension を実装し、ポリシーの生成に参加する必要があることを通知します。 生成されたクライアント上でポリシーのインポートをサポートするには、`InterceptingBindingElementImporter` の派生クラスを登録し、`CreateMessageInterceptor`() をオーバーライドして、ポリシーが有効なそれらの `ChannelMessageInterceptor` クラスを生成します。  
  
## <a name="example-droppable-message-inspector"></a>例: 破棄可能なメッセージ インスペクタ  
 このサンプルには、メッセージを破棄する `ChannelMessageInspector` の実装例が含まれています。  
  
```csharp
class DroppingServerElement : InterceptingElement  
{  
    protected override ChannelMessageInterceptor CreateMessageInterceptor()  
    {  
        return new DroppingServerInterceptor();  
    }  
}  
```  
  
 この例には、次のように構成からアクセスできます。  
  
```xml  
<configuration>  
    ...  
    <system.serviceModel>  
        ...  
        <extensions>  
            <bindingElementExtensions>  
                <add name="droppingInterceptor"   
                   type=  
          "Microsoft.ServiceModel.Samples.DroppingServerElement, library"/>  
            </bindingElementExtensions>  
        </extensions>  
    </system.serviceModel>  
</configuration>  
```  
  
 クライアントとサーバーでは、両方ともこの新しく作成された構成セクションを使用して、カスタム ファクトリをランタイム チャネル スタックの最低レベル (トランスポート レベルの上) に挿入します。  
  
```xml  
<customBinding>  
  <binding name="sampleBinding">  
    <droppingInterceptor/>  
    <httpTransport/>  
  </binding>  
</customBinding>  
```  
  
 クライアントでは `MessageInterceptor` ライブラリを使用して、カスタム ヘッダーを偶数番号付きメッセージに追加します。 一方、サービスでは `MessageInterceptor` ライブラリを使用して、この特殊なヘッダーを持たないメッセージを削除します。  
  
 サービスを実行し、次にクライアントを実行すると、次のクライアント出力が表示されます。  
  
```console  
Reporting the next 10 wind speed  
100 kph  
Server dropped a message.  
90 kph  
80 kph  
Server dropped a message.  
70 kph  
60 kph  
Server dropped a message.  
50 kph  
40 kph  
Server dropped a message.  
30 kph  
20 kph  
Server dropped a message.  
10 kph  
Press ENTER to shut down client  
```  
  
 クライアントからは 10 種類の異なる風速がサービスに報告されますが、特殊なヘッダーがあるのはそれらのタグの半分だけです。  
  
 サービスには、次の出力が表示されます。  
  
```console  
Press ENTER to exit.  
Dangerous wind detected! Reported speed (90) is greater than 64 kph.  
Dangerous wind detected! Reported speed (70) is greater than 64 kph.  
5 wind speed reports have been received.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. 次のコマンドを使用して、ASP.NET 4.0 をインストールします。  
  
    ```console  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
3. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
4. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。  
  
5. 最初に Service.exe を実行して次に Client.exe を実行し、両方のコンソール ウィンドウで出力を表示します。  
