---
title: WCF Web HTTP プログラミング モデルの概要
ms.date: 03/30/2017
ms.assetid: 381fdc3a-6e6c-4890-87fe-91cca6f4b476
ms.openlocfilehash: 8a4b4ff6c0482ed8a09fe30b7d03afc1f84db581
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76739905"
---
# <a name="wcf-web-http-programming-model-overview"></a>WCF Web HTTP プログラミング モデルの概要
Windows Communication Foundation (WCF) WEB HTTP プログラミングモデルは、WCF を使用した WEB HTTP サービスの構築に必要な基本的な要素を提供します。 WCF WEB HTTP サービスは、Web ブラウザーなどの幅広いクライアントからアクセスできるように設計されており、次のような固有の要件があります。  
  
- Uri**と uri の処理**Uri は、WEB HTTP サービスの設計で中心的な役割を果たします。 WCF WEB HTTP プログラミングモデルでは、<xref:System.UriTemplate> クラスと <xref:System.UriTemplateTable> クラスを使用して、URI 処理機能を提供します。  
  
- **GET 操作と POST 操作のサポート**WEB HTTP サービスは、データ変更やリモート呼び出しのためのさまざまな呼び出し動詞に加えて、データ取得のために GET 動詞を使用します。 WCF WEB HTTP プログラミングモデルでは、<xref:System.ServiceModel.Web.WebGetAttribute> と <xref:System.ServiceModel.Web.WebInvokeAttribute> を使用して、サービス操作を GET と、PUT、POST、DELETE などの他の HTTP 動詞の両方に関連付けます。  
  
- **複数のデータ形式**Web スタイルサービスは、SOAP メッセージに加えて、さまざまな種類のデータを処理します。 WCF WEB HTTP プログラミングモデルでは、<xref:System.ServiceModel.WebHttpBinding> と <xref:System.ServiceModel.Description.WebHttpBehavior> を使用して、XML ドキュメント、JSON データオブジェクト、バイナリコンテンツ (画像、ビデオファイル、プレーンテキストなど) のストリームなど、さまざまなデータ形式をサポートしています。  
  
 WCF WEB HTTP プログラミングモデルは、WEB HTTP サービス、AJAX および JSON サービス、配信 (ATOM/RSS) フィードを含む Web スタイルのシナリオに対応するために、WCF の範囲を拡張します。 AJAX および JSON サービスの詳細については、「 [ajax の統合と json のサポート](../../../../docs/framework/wcf/feature-details/ajax-integration-and-json-support.md)」を参照してください。 配信の詳細については、「 [WCF 配信の概要](../../../../docs/framework/wcf/feature-details/wcf-syndication-overview.md)」を参照してください。  
  
 WEB HTTP サービスから返されるデータの種類に追加の制限はありません。 WEB HTTP サービス操作からは任意のシリアル化可能な型を返すことができます。 WEB HTTP サービス操作は Web ブラウザーによって呼び出すことができるため、URL に指定できるデータ型に制限があります。 既定でサポートされている型の詳細については、以下の「 **UriTemplate クエリ文字列パラメーターと url** 」セクションを参照してください。 既定の動作は、URL で指定されたパラメーターから実際のパラメーター型への変換方法を指定する独自の T:System.ServiceModel.Dispatcher.QueryStringConverter 実装を提供することで変更できます。 詳細については、<xref:System.ServiceModel.Dispatcher.QueryStringConverter> を参照してください。  
  
> [!CAUTION]
> WCF WEB HTTP プログラミングモデルで記述されたサービスは、SOAP メッセージを使用しません。 SOAP は使用されないため、WCF によって提供されるセキュリティ機能は使用できません。 ただし、HTTPS でサービスをホストすることによってトランスポート ベースのセキュリティを使用できます。 WCF セキュリティの詳細については、「[セキュリティの概要](../../../../docs/framework/wcf/feature-details/security-overview.md)」を参照してください。  
  
> [!WARNING]
> IIS の WebDAV 拡張をインストールすると、WebDAV 拡張がすべての PUT 要求を処理しようとしたときに Web HTTP サービスが HTTP 405 エラーを返すことがあります。 この問題を解決するには、WebDAV 拡張をアンインストールするか、Web サイトの WebDAV 拡張を無効にします。 詳細については、「 [IIS と WebDav](https://learn.iis.net/page.aspx/357/webdav-for-iis-70/) 」を参照してください。  
  
## <a name="uri-processing-with-uritemplate-and-uritemplatetable"></a>UriTemplate および UriTemplateTable を使用した URI 処理  
 URI テンプレートには、構造的に類似する URI の大規模なセットを効率的に表現するための構文が備わっています。 たとえば、テンプレート a/{segment}/c は、中間のセグメントの値を問わず、"a" で始まり "c" で終了する 3 つのセグメントから成る URI のセットを表しています。  
  
 このテンプレートによって、次のような URI が記述されます。  
  
- a/x/c  
  
- a/y/c  
  
- a/z/c  
  
- その他にもあります。  
  
 このテンプレートでは、中かっこによる表記 ("{segment}") で、リテラル値ではなく、変数のセグメントを示しています。  
  
 .NET Framework は <xref:System.UriTemplate> という URI テンプレートでの作業に使用できる新しい API を提供します。 `UriTemplates` を使用すると、次のことができます。  
  
- パラメーターのセットを使用していずれかの `Bind` メソッドを呼び出すと、テンプレートに一致する*完全に終了*した URI を生成できます。 つまり、URI テンプレート内の変数がすべて、実際の値に置き換えられます。  
  
- 候補の URI を使用して `Match`() を呼び出すことができます。このメソッドは、テンプレートを使用して候補の URI を構成要素に分解し、テンプレート内の変数に従って分類される URI のさまざまな要素を収めたディクショナリを返します。  
  
- `Bind`() と `Match`() は逆のもので、`Match`( `Bind`( x ) ) を呼び出し、最初と同じ環境に戻ることができます。  
  
 包含されたテンプレートを個別に扱うことができるデータ構造内の一連の <xref:System.UriTemplate> オブジェクトを追跡することが必要になる場合がよくあります (特に、URI に基づいて要求をサービス操作にディスパッチすることが必要なサーバー上)。 <xref:System.UriTemplateTable> は、URI テンプレートのセットを表し、テンプレート セットと候補の URI が与えられると、最適の組み合わせを選択します。 これは、必要に応じて使用できるように、特定のネットワークスタック (WCF を含む) には関連していません。  
  
 WCF サービス モデルは、<xref:System.UriTemplate> および <xref:System.UriTemplateTable> を使用して、<xref:System.UriTemplate> によって記述された URI セットにサービス操作を関連付けます。 サービス操作は、<xref:System.UriTemplate> または <xref:System.ServiceModel.Web.WebGetAttribute> によって <xref:System.ServiceModel.Web.WebInvokeAttribute> に関連付けられます。 <xref:System.UriTemplate> と <xref:System.UriTemplateTable>の詳細については、「 [UriTemplate と UriTemplateTable](../../../../docs/framework/wcf/feature-details/uritemplate-and-uritemplatetable.md) 」を参照してください。  
  
## <a name="webget-and-webinvoke-attributes"></a>WebGet および WebInvoke 属性  
 WCF WEB HTTP サービスは、さまざまな呼び出し動詞 (HTTP POST、PUT、DELETE など) に加えて、取得動詞 (HTTP GET など) を使用します。 WCF WEB HTTP プログラミングモデルを使用すると、サービス開発者は、サービス操作に関連付けられた URI テンプレートと動詞の両方を、<xref:System.ServiceModel.Web.WebGetAttribute> と <xref:System.ServiceModel.Web.WebInvokeAttribute>で制御できます。 <xref:System.ServiceModel.Web.WebGetAttribute> および <xref:System.ServiceModel.Web.WebInvokeAttribute> を使用すると、個々の操作を URI と、それらの URI に関連付けられている HTTP メソッドにバインドする方法を制御できます。 たとえば、次のコードでは、<xref:System.ServiceModel.Web.WebGetAttribute> および <xref:System.ServiceModel.Web.WebInvokeAttribute> を追加します。  
  
```csharp
[ServiceContract]  
interface ICustomer  
{  
  //"View It"  
  
  [WebGet]  
  Customer GetCustomer():  
  
  //"Do It"  
    [WebInvoke]  
  Customer UpdateCustomerName( string id,   
                               string newName );  
}  
```  
  
 上記のコードを使用すると、次の HTTP 要求を行うことができます。  
  
 `GET /GetCustomer`  
  
 `POST /UpdateCustomerName`  
  
 <xref:System.ServiceModel.Web.WebInvokeAttribute> は、既定で POST に設定されていますが、他の動詞にも使用できます。  
  
```csharp
[ServiceContract]  
interface ICustomer  
{  
  //"View It" -> HTTP GET  
    [WebGet( UriTemplate="customers/{id}" )]  
  Customer GetCustomer( string id ):  
  
  //"Do It" -> HTTP PUT  
  [WebInvoke( UriTemplate="customers/{id}", Method="PUT" )]  
  Customer UpdateCustomer( string id, Customer newCustomer );  
}  
```  
  
 Wcf WEB HTTP プログラミングモデルを使用する WCF サービスの完全なサンプルについては、「[方法: 基本的な Wcf WEB Http サービスを作成](../../../../docs/framework/wcf/feature-details/how-to-create-a-basic-wcf-web-http-service.md)する」を参照してください。  
  
## <a name="uritemplate-query-string-parameters-and-urls"></a>UriTemplate クエリ文字列パラメーターと URL  
 サービス操作に関連付けられた URL を入力することによって、Web ブラウザーから Web スタイルのサービスを呼び出すことができます。 このようなサービス操作は、文字列形式で指定する必要があるクエリ文字列パラメーターを URL 内で受け取ることができます。 次の表に、URL 内で渡すことができる型と、使用される形式を示します。  
  
|種類|Format|  
|----------|------------|  
|<xref:System.Byte>|0 から 255|  
|<xref:System.SByte>|-128 - 127|  
|<xref:System.Int16>|-32768 - 32767|  
|<xref:System.Int32>|-2,147,483,648 - 2,147,483,647|  
|<xref:System.Int64>|-9,223,372,036,854,775,808 - 9,223,372,036,854,775,807|  
|<xref:System.UInt16>|0 - 65535|  
|<xref:System.UInt32>|0 - 4,294,967,295|  
|<xref:System.UInt64>|0 - 18,446,744,073,709,551,615|  
|<xref:System.Single>|-3.402823e38 - 3.402823e38 (指数表記は要求されない)|  
|<xref:System.Double>|-1.79769313486232e308 - 1.79769313486232e308 (指数表記は要求されない)|  
|<xref:System.Char>|任意の 1 文字|  
|<xref:System.Decimal>|標準表記による任意の小数 (指数なし)|  
|<xref:System.Boolean>|True または False (大文字と小文字は区別されません)|  
|<xref:System.String>|任意の文字列 (null 文字列はサポートされず、エスケープは行われない)|  
|<xref:System.DateTime>|MM/DD/YYYY<br /><br /> MM/DD/YYYY HH: MM: SS [AM&#124;PM]<br /><br /> 月 日 年<br /><br /> Month Day Year HH: MM: SS [AM&#124;PM]|  
|<xref:System.TimeSpan>|DD.HH:MM:SS<br /><br /> DD = 日、HH = 時、MM = 分、SS = 秒|  
|<xref:System.Guid>|GUID。たとえば、次のようになります。<br /><br /> 936DA01F-9ABD-4d9d-80C7-02AF85C822A8|  
|<xref:System.DateTimeOffset>|MM/DD/YYYY HH:MM:SS MM:SS<br /><br /> DD = 日、HH = 時、MM = 分、SS = 秒|  
|列挙型|列挙値。たとえば、次のコードのように列挙体を定義します。<br /><br /> `public enum Days{ Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday };`<br /><br /> クエリ文字列に、任意の列挙値 (またはそれぞれに対応する integer 値) を指定できます。|  
|型と文字列表現を双方向に変換できる `TypeConverterAttribute` を持つ型。|型コンバーターによって異なります。|  
  
## <a name="formats-and-the-wcf-web-http-programming-model"></a>形式と WCF WEB HTTP プログラミング モデル  
 WCF WEB HTTP プログラミングモデルには、さまざまなデータ形式を使用するための新機能があります。 <xref:System.ServiceModel.WebHttpBinding> は、バインディング層で次のさまざまな種類のデータを読み書きできます。  
  
- XML  
  
- JSON  
  
- 不透明なバイナリ ストリーム  
  
 これは、WCF WEB HTTP プログラミングモデルがあらゆる種類のデータを処理できることを意味しますが、<xref:System.IO.Stream>に対してプログラミングすることもできます。  
  
 .NET Framework 3.5 では、JSON データ (AJAX) と配信フィード (ATOM と RSS を含む) がサポートされています。 これらの機能の詳細については、「wcf [WEB HTTP 書式設定](../../../../docs/framework/wcf/feature-details/wcf-web-http-formatting.md)[Wcf 配信の概要](../../../../docs/framework/wcf/feature-details/wcf-syndication-overview.md)」および「 [AJAX の統合と JSON のサポート](../../../../docs/framework/wcf/feature-details/ajax-integration-and-json-support.md)」を参照してください。  
  
## <a name="wcf-web-http-programming-model-and-security"></a>WCF WEB HTTP プログラミング モデルとセキュリティ  

WCF WEB HTTP プログラミングモデルでは WS-* プロトコルがサポートされていないため、WCF WEB HTTP サービスをセキュリティで保護する唯一の方法は、SSL を使用して HTTPS 経由でサービスを公開することです。 IIS 7.0 での SSL の設定の詳細については、「 [iis で ssl を実装する方法](https://support.microsoft.com/help/299875/how-to-implement-ssl-in-iis)」を参照してください。
  
## <a name="troubleshooting-the-wcf-web-http-programming-model"></a>WCF WEB HTTP プログラミング モデルのトラブルシューティング  
 <xref:System.ServiceModel.Channels.ChannelFactoryBase%601> を使用してチャネルを作成するために WCF WEB HTTP サービスを呼び出すと、異なる <xref:System.ServiceModel.Description.WebHttpBehavior> が <xref:System.ServiceModel.EndpointAddress> に渡されるとしても、<xref:System.ServiceModel.EndpointAddress> は構成ファイルに設定されている <xref:System.ServiceModel.Channels.ChannelFactoryBase%601> を使用します。  
  
## <a name="see-also"></a>参照

- [WCF 配信](../../../../docs/framework/wcf/feature-details/wcf-syndication.md)
- [WCF Web HTTP プログラミング オブジェクト モデル](../../../../docs/framework/wcf/feature-details/wcf-web-http-programming-object-model.md)
- [WCF Web HTTP プログラミング モデル](../../../../docs/framework/wcf/feature-details/wcf-web-http-programming-model.md)
