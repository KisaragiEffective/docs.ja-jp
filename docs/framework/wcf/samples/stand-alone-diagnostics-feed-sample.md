---
title: スタンドアロン診断フィードのサンプル
ms.date: 03/30/2017
ms.assetid: d31c6c1f-292c-4d95-8e23-ed8565970ea5
ms.openlocfilehash: 4520883b5db19c28544a5576ca600b83e37eede3
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76787932"
---
# <a name="stand-alone-diagnostics-feed-sample"></a>スタンドアロン診断フィードのサンプル
このサンプルでは、Windows Communication Foundation (WCF) を使用して配信するために RSS/Atom フィードを作成する方法を示します。 これは基本的な "Hello World" プログラムであり、オブジェクトモデルの基本と、Windows Communication Foundation (WCF) サービスでの設定方法を示しています。  
  
 WCF は、特別なデータ型の <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter>を返すサービス操作として配信フィードをモデル化します。 <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> のインスタンスは、フィードを RSS 2.0 形式および Atom 1.0 形式の両方にシリアル化できます。 使用するコントラクトを次のサンプル コードに示します。  
  
```csharp  
[ServiceContract(Namespace = "")]  
    interface IDiagnosticsService  
    {  
        [OperationContract]  
        //The [WebGet] attribute controls how WCF dispatches  
        //HTTP requests to service operations based on a URI suffix  
        //(the part of the request URI after the endpoint address)  
        //using the HTTP GET method. The UriTemplate specifies a relative  
        //path of 'feed', and specifies that the format is  
        //supplied using a query string.   
        [WebGet(UriTemplate="feed?format={format}")]  
        [ServiceKnownType(typeof(Atom10FeedFormatter))]  
        [ServiceKnownType(typeof(Rss20FeedFormatter))]  
        SyndicationFeedFormatter GetProcesses(string format);  
    }  
```  
  
 `GetProcesses` 操作には、WCF が HTTP GET 要求をサービス操作にディスパッチする方法を制御し、送信されるメッセージの形式を指定できるようにする、<xref:System.ServiceModel.Web.WebGetAttribute> 属性で注釈が付けられています。  
  
 任意の WCF サービスと同様に、配信フィードは任意のマネージアプリケーションで自己ホストすることができます。 配信サービスが適切に機能するには、特定のバインディング (<xref:System.ServiceModel.WebHttpBinding>) と、特定のエンドポイント動作 (<xref:System.ServiceModel.Description.WebHttpBehavior>) が必要です。 新しい <xref:System.ServiceModel.Web.WebServiceHost> クラスには、特定の構成を使用せずにこのようなエンドポイントを作成する際に便利な API が用意されています。  
  
```csharp  
WebServiceHost host = new WebServiceHost(typeof(ProcessService), new Uri("http://localhost:8000/diagnostics"));  
  
            //The WebServiceHost will automatically provide a default endpoint at the base address  
            //using the proper binding (the WebHttpBinding) and endpoint behavior (the WebHttpBehavior)  
```  
  
 または、IIS でホストされる .svc ファイルから <xref:System.ServiceModel.Activation.WebServiceHostFactory> を使用して、同等の機能を用意することもできます (この手法は、このサンプル コードでは示されません)。  
  
```xml
<%@ ServiceHost Language="C#|VB" Debug="true" Service="ProcessService" %>
```
  
 このサービスは標準の HTTP GET を使用して要求を受け取るので、サービスへのアクセスには、RSS または ATOM に対応している任意のクライアントを使用できます。 たとえば、RSS 対応のブラウザーで `http://localhost:8000/diagnostics/feed/?format=atom` または `http://localhost:8000/diagnostics/feed/?format=rss` に移動することによって、このサービスの出力を表示できます。
  
 また、 [WCF 配信オブジェクトモデルが Atom および RSS にマップ](../../../../docs/framework/wcf/feature-details/how-the-wcf-syndication-object-model-maps-to-atom-and-rss.md)され、命令型コードを使用してシンジケートデータを読み取り、処理する方法を使用することもできます。  
  
```csharp
XmlReader reader = XmlReader.Create( "http://localhost:8000/diagnostics/feed/?format=rss",
    new XmlReaderSettings()
    {
        //MaxCharactersInDocument can be used to control the maximum amount of data
        //read from the reader and helps prevent OutOfMemoryException
        MaxCharactersInDocument = 1024 * 64
    } );

SyndicationFeed feed = SyndicationFeed.Load(reader);

foreach (SyndicationItem i in feed.Items)
{
    XmlSyndicationContent content = i.Content as XmlSyndicationContent;
    ProcessData pd = content.ReadContent<ProcessData>();

    Console.WriteLine(i.Title.Text);
    Console.WriteLine(pd.ToString());
}
```
  
## <a name="set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行する
  
1. Windows Communication Foundation のサンプルについては、「 [1 回限りのセットアップ手順](one-time-setup-procedure-for-the-wcf-samples.md)」で説明されているように、コンピューター上で HTTP および HTTPS に対する適切なアドレス登録アクセス許可を持っていることを確認します。

2. ソリューションをビルドします。

3. コンソール アプリケーションを実行します。

4. コンソールアプリケーションの実行中に、RSS 対応のブラウザーを使用して `http://localhost:8000/diagnostics/feed/?format=atom` または `http://localhost:8000/diagnostics/feed/?format=rss` に移動します。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Syndication\DiagnosticsFeed`

## <a name="see-also"></a>関連項目

- [WCF Web HTTP プログラミング モデル](../feature-details/wcf-web-http-programming-model.md)
- [WCF 配信](../feature-details/wcf-syndication.md)
