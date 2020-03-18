---
title: WCF Web HTTP プログラミング オブジェクト モデル
ms.date: 03/30/2017
ms.assetid: ed96b5fc-ca2c-4b0d-bdba-d06b77c3cb2a
ms.openlocfilehash: 39c7ec31827d1cde5d95516cc3867f9d6bf9817f
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76739863"
---
# <a name="wcf-web-http-programming-object-model"></a>WCF Web HTTP プログラミング オブジェクト モデル
WCF WEB HTTP プログラミングモデルを使用すると、開発者は SOAP を必要とすることなく、基本的な HTTP 要求を使用して Windows Communication Foundation (WCF) Web サービスを公開できます。 WCF WEB HTTP プログラミングモデルは、既存の WCF 拡張モデル上に構築されています。 Web HTTP プログラミング モデルでは、次のクラスが定義されます。  
  
 **プログラミングモデル:**  
  
- <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute>  
  
- <xref:System.ServiceModel.Web.WebGetAttribute>  
  
- <xref:System.ServiceModel.Web.WebInvokeAttribute>  
  
- <xref:System.ServiceModel.Web.WebServiceHost>  
  
 **チャネルとディスパッチャーインフラストラクチャ:**  
  
- <xref:System.ServiceModel.WebHttpBinding>  
  
- <xref:System.ServiceModel.Description.WebHttpBehavior>  
  
 **ユーティリティクラスと機能拡張ポイント:**  
  
- <xref:System.UriTemplate>  
  
- <xref:System.UriTemplateTable>  
  
- <xref:System.ServiceModel.Dispatcher.QueryStringConverter>  
  
- <xref:System.ServiceModel.Dispatcher.WebHttpDispatchOperationSelector>  
  
## <a name="aspnetcacheprofileattribute"></a>AspNetCacheProfileAttribute  
 <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute> は、サービス操作に適用された場合に、構成ファイル内の ASP.NET 出力キャッシュ プロファイルを示します。この構成ファイルを使用して ASP .NET 出力キャッシュに操作の応答がキャッシュされます。 このプロパティが受け取るパラメーターは 1 つ (構成ファイルでキャッシュ設定を指定するキャッシュ プロファイル名) だけです。  
  
## <a name="webgetattribute"></a>WebGetAttribute  
 <xref:System.ServiceModel.Web.WebGetAttribute> 属性は、サービス操作を HTTP GET 要求に応答する操作としてマークするために使用されます。 メタデータを操作の説明に追加する受信操作の動作です (<xref:System.ServiceModel.Description.IOperationBehavior> メソッドは何の処理も行いません)。 このメタデータを操作の説明で検索する動作 (特に <xref:System.ServiceModel.Web.WebGetAttribute>) がサービスの動作コレクションに追加されていない場合は、<xref:System.ServiceModel.Description.WebHttpBehavior> を適用しても機能しません。 <xref:System.ServiceModel.Web.WebGetAttribute> 属性には、次の表に示すパラメーターをオプションで指定できます。  
  
|パラメーター|[説明]|  
|---------------|-----------------|  
|`BodyStyle`|属性が適用されているサービス操作との間で送受信される要求と応答をラップするかどうかを制御します。|  
|`RequestFormat`|要求メッセージの書式設定方法を制御します。|  
|`ResponseFormat`|応答メッセージの書式設定方法を制御します。|  
|`UriTemplate`|属性が適用されているサービス操作にマップされた HTTP 要求を制御する URI テンプレートを指定します。|  
  
## <a name="webhttpbinding"></a>WebHttpBinding  
 <xref:System.ServiceModel.WebHttpBinding> クラスは、<xref:System.ServiceModel.Channels.WebMessageEncodingBindingElement> を使用して、XML、JSON、および生のバイナリ データのサポートを組み込みます。 これは、<xref:System.ServiceModel.Channels.HttpsTransportBindingElement>、<xref:System.ServiceModel.Channels.HttpTransportBindingElement>、および <xref:System.ServiceModel.WebHttpSecurity> オブジェクトから構成されています。 <xref:System.ServiceModel.WebHttpBinding> は、<xref:System.ServiceModel.Description.WebHttpBehavior> と組み合わせて使用するようにデザインされています。  
  
## <a name="webinvokeattribute"></a>WebInvokeAttribute  
 <xref:System.ServiceModel.Web.WebInvokeAttribute> 属性は <xref:System.ServiceModel.Web.WebGetAttribute> 属性によく似ていますが、サービス操作を GET 以外の HTTP 要求に応答する操作としてマークするために使用されます。 メタデータを操作の説明に追加する受信操作の動作です (<xref:System.ServiceModel.Description.IOperationBehavior> メソッドは何の処理も行いません)。 このメタデータを操作の説明で検索する動作 (特に <xref:System.ServiceModel.Web.WebInvokeAttribute>) がサービスの動作コレクションに追加されていない場合は、<xref:System.ServiceModel.Description.WebHttpBehavior> を適用しても機能しません。  
  
 <xref:System.ServiceModel.Web.WebInvokeAttribute> 属性には、次の表に示すパラメーターをオプションで指定できます。  
  
|パラメーター|[説明]|  
|---------------|-----------------|  
|`BodyStyle`|属性が適用されているサービス操作との間で送受信される要求と応答をラップするかどうかを制御します。|  
|`Method`|サービス操作がマップされる HTTP メソッドを指定します。|  
|`RequestFormat`|要求メッセージの書式設定方法を制御します。|  
|`ResponseFormat`|応答メッセージの書式設定方法を制御します。|  
|`UriTemplate`|属性が適用されているサービス操作にマップされた GET 要求を制御する URI テンプレートを指定します。|  
  
## <a name="uritemplate"></a>UriTemplate  
 <xref:System.UriTemplate> クラスでは、構造が似ている URI のセットを定義できます。 テンプレートは、パスとクエリの 2 つの部分から構成されています。 パスは、スラッシュ (/) で区切られた一連のセグメントから構成されています。 各セグメントには、リテラル値、変数値 (中かっこ [{}] 内に記述)、1つのセグメントの内容と一致するように制約されている、ワイルドカード (アスタリスク [\*] として記述され、パスの末尾に置く必要があります) を指定できます。 クエリ式は、すべて省略することができます。 存在する場合、クエリ式は順序なしの一連の名前/値ペアを指定します。 クエリ式の要素には、リテラルペア (? x = 2) または変数ペア (? x = {*value*}) のいずれかを指定できます。 対になっていない値は使用できません。 <xref:System.UriTemplate> は、特定の Uri または Uri のグループをサービス操作にマップするために、WCF WEB HTTP プログラミングモデルによって内部的に使用されます。  
  
## <a name="uritemplatetable"></a>UriTemplateTable  
 <xref:System.UriTemplateTable> クラスは、開発者が選択したオブジェクトにバインドされた <xref:System.UriTemplate> オブジェクトの結合セットを表します。 これにより、セット内のテンプレートと候補 URI (Uniform Resource Identifier) を照合し、一致したテンプレートに関連付けられているデータを取得することができます。 <xref:System.UriTemplateTable> は、特定の Uri または Uri のグループをサービス操作にマップするために、WCF WEB HTTP プログラミングモデルによって内部的に使用されます。  
  
## <a name="webservicehost"></a>WebServiceHost  
 <xref:System.ServiceModel.Web.WebServiceHost> は <xref:System.ServiceModel.ServiceHost> を拡張するため、SOAP 以外の Web スタイル サービスを容易にホストできます。 <xref:System.ServiceModel.Web.WebServiceHost> では、サービスの説明にエンドポイントが見つからない場合、サービスのベース アドレスに既定のエンドポイントを自動的に作成します。 既定の HTTP エンドポイントを作成するときに、<xref:System.ServiceModel.Web.WebServiceHost> は、HTTP ヘルプ ページと Web サービス記述言語 (WSDL) GET 機能を無効にして、メタデータ エンドポイントが既定の HTTP エンドポイントに干渉しないようにします。 また、<xref:System.ServiceModel.Web.WebServiceHost> は、<xref:System.ServiceModel.WebHttpBinding> を使用するすべてのエンドポイントに必要な <xref:System.ServiceModel.Description.WebHttpBehavior> がアタッチされるようにもします。 最後に、<xref:System.ServiceModel.Web.WebServiceHost> は、エンドポイントのバインディングを、安全な仮想ディレクトリで使用するときに、関連付けられたインターネット インフォメーション サービス (IIS) のセキュリティ設定と連携して動作するよう自動的に構成します。  
  
## <a name="webservicehostfactory"></a>WebServiceHostFactory  
 <xref:System.ServiceModel.Activation.WebServiceHostFactory> クラスは、サービスがインターネット インフォメーション サービス (IIS) または Windows プロセス アクティブ化サービス (WAS) でホストされている場合に、<xref:System.ServiceModel.Web.WebServiceHost> を動的に作成するために使用されます。 ホスト アプリケーションが <xref:System.ServiceModel.Web.WebServiceHost> をインスタンス化する自己ホスト型サービスとは異なり、IIS または WAS でホストされているサービスは、このクラスを使用してサービスの <xref:System.ServiceModel.Web.WebServiceHost> を作成します。 <xref:System.ServiceModel.Activation.WebServiceHostFactory.CreateServiceHost%28System.Type%2CSystem.Uri%5B%5D%29> メソッドは、サービスの受信要求の受信時に呼び出されます。  
  
## <a name="webhttpbehavior"></a>WebHttpBehavior  
 <xref:System.ServiceModel.Description.WebHttpBehavior> クラスは、必要なフォーマッタ、操作セレクターなど、サービス モデル レイヤーで Web スタイル サービスをサポートするために必要な内容を提供します。 このパラメーターは (<xref:System.ServiceModel.WebHttpBinding> と共に使用される) エンドポイントの動作として実装され、これにより、フォーマッタと操作セレクターを各エンドポイントに指定できるようになり、さらに、同じサービス実装で SOAP エンドポイントと POX エンドポイントの両方を公開できるようになります。  
  
### <a name="extending-webhttpbehavior"></a>WebHttpBehavior の拡張  
 <xref:System.ServiceModel.Description.WebHttpBehavior> は、仮想メソッド <xref:System.ServiceModel.Description.WebHttpBehavior.GetOperationSelector%28System.ServiceModel.Description.ServiceEndpoint%29>、<xref:System.ServiceModel.Description.WebHttpBehavior.GetReplyClientFormatter%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Description.ServiceEndpoint%29>、<xref:System.ServiceModel.Description.WebHttpBehavior.GetRequestClientFormatter%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Description.ServiceEndpoint%29>、<xref:System.ServiceModel.Description.WebHttpBehavior.GetReplyDispatchFormatter%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Description.ServiceEndpoint%29>、および <xref:System.ServiceModel.Description.WebHttpBehavior.GetRequestDispatchFormatter%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Description.ServiceEndpoint%29> を使用して拡張できます。 開発者は、<xref:System.ServiceModel.Description.WebHttpBehavior> から派生クラスを作成し、そのメソッドをオーバーライドして既定の動作をカスタマイズすることができます。  
  
 <xref:System.ServiceModel.Description.WebScriptEnablingBehavior> は、<xref:System.ServiceModel.Description.WebHttpBehavior> 拡張の一例です。 <xref:System.ServiceModel.Description.WebScriptEnablingBehavior> を使用すると、Windows Communication Foundation (WCF) エンドポイントはブラウザーベースの ASP.NET AJAX クライアントから HTTP 要求を受信できます。 この機能拡張ポイントを使用する例として、 [HTTP POST を使用する AJAX サービス](../../../../docs/framework/wcf/samples/ajax-service-using-http-post.md)が挙げられます。  
  
> [!WARNING]
> <xref:System.ServiceModel.Description.WebScriptEnablingBehavior> を使用する場合、<xref:System.UriTemplate> は、<xref:System.ServiceModel.Web.WebGetAttribute> または <xref:System.ServiceModel.Web.WebInvokeAttribute> 属性内でサポートされません。  
  
## <a name="webhttpdispatchoperationselector"></a>WebHttpDispatchOperationSelector  
 <xref:System.ServiceModel.Dispatcher.WebHttpDispatchOperationSelector> クラスは、<xref:System.UriTemplate> クラスおよび <xref:System.UriTemplateTable> クラスを使用して、サービス操作に対して呼び出しをディスパッチします。  
  
## <a name="compatibility"></a>互換性  
 WCF WEB HTTP プログラミングモデルは、SOAP ベースのメッセージを使用しないため、WS-* プロトコルをサポートしていません。 ただし、SOAP を使用するエンドポイントと SOAP を使用しないその他のエンドポイントの 2 つの異なるエンドポイントを使用して、同じコントラクトを公開できます。 例については、「[方法: SOAP および Web クライアントにコントラクトを公開](../../../../docs/framework/wcf/feature-details/how-to-expose-a-contract-to-soap-and-web-clients.md)する」を参照してください。  
  
## <a name="security"></a>Security  

WCF WEB HTTP プログラミングモデルでは WS-* プロトコルがサポートされていないため、WCF WEB HTTP プログラミングモデル上に構築された Web サービスをセキュリティで保護する唯一の方法は、SSL を使用してサービスを公開することです。 IIS 7.0 での SSL の設定の詳細については、「 [iis で ssl を実装する方法](https://support.microsoft.com/help/299875/how-to-implement-ssl-in-iis)」を参照してください。
  
## <a name="see-also"></a>参照

- <xref:System.ServiceModel.WebHttpBinding>
- <xref:System.ServiceModel.Web.WebGetAttribute>
- <xref:System.ServiceModel.Web.WebInvokeAttribute>
- <xref:System.ServiceModel.Description.WebHttpBehavior>
- <xref:System.ServiceModel.Dispatcher.WebHttpDispatchOperationSelector>
- [WCF Web HTTP プログラミング モデルの概要](../../../../docs/framework/wcf/feature-details/wcf-web-http-programming-model-overview.md)
