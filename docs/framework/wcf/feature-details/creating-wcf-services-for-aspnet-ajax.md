---
title: ASP.NET AJAX 用の WCF サービスの作成
ms.date: 03/30/2017
ms.assetid: 04c0402c-e617-4ba5-aedf-d17692234776
ms.openlocfilehash: 2ec4d2f1f2fb3a6a184a524ed0134360407b4649
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964064"
---
# <a name="creating-wcf-services-for-aspnet-ajax"></a>ASP.NET AJAX 用の WCF サービスの作成

Microsoft ASP.NET AJAX により、応答性に優れ、使い慣れたユーザー インターフェイス要素を使用して、充実したユーザー エクスペリエンスを提供する Web ページを簡単に作成できます。 ASP.NET AJAX には、ブラウザーに依存しない ECMAScript (JavaScript) テクノロジとダイナミック HTML (DHTML) テクノロジを組み込んだクライアント スクリプト ライブラリが用意されており、これらのライブラリが ASP.NET 2.0 サーバー ベース開発プラットフォームと統合されます。 ASP.NET AJAX を使用することで、Web アプリケーションのユーザー エクスペリエンスと効率を向上させることができます。

ASP.NET AJAX は、クライアント スクリプト ライブラリとサーバー コンポーネントを統合した強固な開発フレームワークです。 ASP.NET ページからサービスにアクセスする場合、サービス URL をページ上の ASP.NET スクリプト マネージャーに追加すると、標準の JavaScript 関数の呼び出しとまったく同じに見える JavaScript コードを使い、サービス操作を呼び出すことができます。

適切な ASP.NET AJAX エンドポイントを追加することによって、ほとんどの Windows Communication Foundation (WCF) サービスが ASP.NET AJAX と互換性のあるサービスとして公開される場合があります。

Visual Studio を使用している場合は、ASP.NET Web サイトまたは Web アプリケーションを操作するときに **[新しい項目の追加]** ダイアログボックスで使用できる、AJAX 対応 WCF サービス用のビルド済みテンプレートを使用できます。

Visual Studio テンプレートを使用しない場合、ASP.NET AJAX エンドポイントを作成するには次の 2 つの方法があります。

- 構成を使用せずに、動的ホスト アクティブ化を使用してエンドポイントを作成する。 これは、WCF 構成システムの操作に慣れていない場合の最も簡単な方法です。 詳細については、「[方法: 構成を使用せずに ASP.NET AJAX エンドポイントを追加する](../../../../docs/framework/wcf/feature-details/how-to-add-an-aspnet-ajax-endpoint-without-using-configuration.md)」を参照してください。

- 構成を使用して、AJAX 対応エンドポイントを WCF サービスに追加します。 詳細については、「[方法: 構成を使用して ASP.NET AJAX エンドポイントを追加](../../../../docs/framework/wcf/feature-details/how-to-use-configuration-to-add-an-aspnet-ajax-endpoint.md)する」を参照してください。

「 [WCF WEB HTTP プログラミングモデルの概要](../../../../docs/framework/wcf/feature-details/wcf-web-http-programming-model-overview.md)」で説明されている Web プログラミングモデルは、ASP.NET AJAX サービスで使用できます。 具体的には、次のように使用します。

- <xref:System.ServiceModel.Web.WebGetAttribute> および <xref:System.ServiceModel.Web.WebInvokeAttribute> 属性を使用して、HTTP GET 動詞と HTTP POST 動詞のいずれかを選択できます。 正しく使用すれば、アプリケーションのパフォーマンスを大幅に向上させることができます。 詳細については、「[方法: ASP.NET AJAX エンドポイントに対して HTTP POST 要求または HTTP GET 要求を選択する](../../../../docs/framework/wcf/feature-details/http-post-and-http-get-requests-for-aspnet-ajax-endpoints.md)」を参照してください。

- <xref:System.ServiceModel.Web.WebGetAttribute.ResponseFormat%2A> および <xref:System.ServiceModel.Web.WebInvokeAttribute.ResponseFormat%2A> プロパティを使用して、サービスが返すデータを、既定の JSON (JavaScript Object Notation) ではなく XML データにすることができます。 これを ASP.NET AJAX フレームワークで行うと、JavaScript クライアントは XML DOM オブジェクトを受け取ります。

  > [!WARNING]
  > この動作を機能させるには、コンテンツ タイプを text/xml に設定する必要があります。 設定しない場合、JavaScript クライアントは XML DOM オブジェクトではなく XML を含む文字列を受け取ります。

    コンテンツ タイプが適切に設定された XML データを返す操作の例を次に示します。

  ```csharp
  [OperationContract, WebGet(ResponseFormat=WebMessageFormat.Xml)]
  public XElement GetData()
  {
      XElement x;
      //Get some data here...

      WebOperationContext.Current.OutgoingResponse.ContentType = "text/xml";
      return x;
  }
  ```

- ASP.NET AJAX との互換性が必要な場合、<xref:System.ServiceModel.Web.WebGetAttribute> および <xref:System.ServiceModel.Web.WebInvokeAttribute> 属性の他の属性は変更できません。 ASP.NET AJAX 呼び出し規約に違反しない限り、Web プログラミング モデルのその他の機能を使用できます。

 より高度なシナリオでは、WCF での AJAX サポートの詳細をいくつか理解する必要があります。

- JavaScript を使用して AJAX ページクライアントと WCF サービスの間でデータが転送される方法を理解するには、.NET Framework 型を JavaScript の型にマップする方法の詳細については、「 [FOR JSON とその他のデータ転送形式のサポート](../../../../docs/framework/wcf/feature-details/support-for-json-and-other-data-transfer-formats.md)」を参照してください。

- たとえば、URL ベースの認証や ASP.NET セッション情報へのアクセスなどの ASP.NET 機能を活用するには、構成で ASP.NET 互換モードを有効にします。

WCF の AJAX エンドポイントは、ASP.NET AJAX フレームワークなしでも使用できます。 これを行うには、WCF における AJAX サポートのサポートアーキテクチャを理解する必要があります。 このアーキテクチャの詳細については、「 [WCF WEB HTTP プログラミングオブジェクトモデル](../../../../docs/framework/wcf/feature-details/wcf-web-http-programming-object-model.md)」を参照してください。 この方法を示すコードサンプルについては、「 [JSON および XML を使用した AJAX サービス](../../../../docs/framework/wcf/samples/ajax-service-with-json-and-xml-sample.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [WCF Web HTTP プログラミング モデル](../../../../docs/framework/wcf/feature-details/wcf-web-http-programming-model.md)
- [方法 : 構成を使用せずに ASP.NET AJAX エンドポイントを追加する](../../../../docs/framework/wcf/feature-details/how-to-add-an-aspnet-ajax-endpoint-without-using-configuration.md)
- [方法 : 構成を使用して ASP.NET AJAX エンドポイントを追加する](../../../../docs/framework/wcf/feature-details/how-to-use-configuration-to-add-an-aspnet-ajax-endpoint.md)
- [方法 : ASP.NET AJAX エンドポイントのために HTTP POST または HTTP GET を選択する](../../../../docs/framework/wcf/feature-details/http-post-and-http-get-requests-for-aspnet-ajax-endpoints.md)
