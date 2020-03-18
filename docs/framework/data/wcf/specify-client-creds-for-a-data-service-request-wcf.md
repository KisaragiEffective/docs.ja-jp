---
title: '方法: データ サービス要求のクライアント資格情報を指定する (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, customizing requests
ms.assetid: 1632f9af-e45f-4363-9222-03823daa8e28
ms.openlocfilehash: c445f0d6117abfd1ac7a5153f8f80d55051a3f5a
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75900928"
---
# <a name="how-to-specify-client-credentials-for-a-data-service-request-wcf-data-services"></a>方法: データ サービス要求のクライアント資格情報を指定する (WCF Data Services)
既定では、クライアントライブラリは、OData サービスに要求を送信するときに資格情報を提供しません。 ただし、<xref:System.Net.NetworkCredential> の <xref:System.Data.Services.Client.DataServiceContext.Credentials%2A> プロパティに <xref:System.Data.Services.Client.DataServiceContext> を設定することで、データ サービスへの要求を認証するために資格情報を送信するように指定できます。 詳細については、「 [Securing WCF Data Services](securing-wcf-data-services.md)」を参照してください。 このトピックの例では、データサービスからデータを要求するときに、WCF Data Services クライアントによって使用される資格情報を明示的に指定する方法を示します。  
  
 このトピックの例では、Northwind サンプル データ サービスおよび自動生成されたクライアント データ サービス クラスを使用します。 このサービスとクライアントデータクラスは、 [WCF Data Services のクイックスタート](quickstart-wcf-data-services.md)を完了したときに作成されます。 OData Web サイトで公開されている[Northwind サンプルデータサービス](https://services.odata.org/Northwind/Northwind.svc/)を使用することもできます。このサンプルデータサービスは読み取り専用であるため、変更を保存しようとするとエラーが返されます。 OData Web サイトのサンプルデータサービスでは、匿名認証が許可されています。  
  
## <a name="example"></a>使用例  
 次の例は、Windows Presentation Framework アプリケーションのメインページである Extensible Application Markup Language (XAML) ファイルの分離コードページからのものです。 この例では、`LoginWindow` インスタンスを表示してユーザーから認証資格情報を収集し、データ サービスへの要求を行うときにこれらの資格情報を使用します。  
  
 [!code-csharp[Astoria Northwind Client#ClientCredentials](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/clientcredentials.xaml.cs#clientcredentials)]  
 [!code-vb[Astoria Northwind Client#ClientCredentials](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/clientcredentials.xaml.vb#clientcredentials)]
  
## <a name="example"></a>使用例  
 次の XAML は、WPF アプリケーションのメイン ページを定義します。  
  
 [!code-xaml[Astoria Northwind Client#ClientCredentialsXaml](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/clientcredentials.xaml#clientcredentialsxaml)]  
  
## <a name="example"></a>使用例  
 次の例は、データ サービスへの要求を行う前にユーザーから認証資格情報を収集するために使用するウィンドウの分離コード ページからの抜粋です。  
  
 [!code-csharp[Astoria Northwind Client#ClientCredentialsLogin](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/clientcredentialslogin.xaml.cs#clientcredentialslogin)]  
 [!code-vb[Astoria Northwind Client#ClientCredentialsLogin](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/clientcredentialslogin.xaml.vb#clientcredentialslogin)]
  
## <a name="example"></a>使用例  
 次の XAML は、WPF アプリケーションのログインを定義します。  
  
 [!code-xaml[Astoria Northwind Client#ClientCredentialsLoginXaml](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/clientcredentialslogin.xaml#clientcredentialsloginxaml)]  
  
## <a name="net-framework-security"></a>.NET Framework セキュリティ  
 このトピックの例には、以下のセキュリティに関する注意点があります。  
  
- このサンプルで指定した資格情報が機能することを確認するには、Northwind データ サービスで匿名アクセス以外の認証方式を使用する必要があります。 そうしない場合、データ サービスをホストする Web サイトが資格情報を要求しません。  
  
- ユーザー資格情報は実行時にのみ要求し、キャッシュしないようにする必要があります。 資格情報は常に安全に格納する必要があります。  
  
- 基本認証およびダイジェスト認証で送信されるデータは暗号化されないため、敵対者がデータを見ることができます。 また、基本認証の資格情報 (ユーザー名とパスワード) はクリア テキストで送信されるので、傍受される可能性があります。  
  
 詳細については、「 [Securing WCF Data Services](securing-wcf-data-services.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [WCF Data Services のセキュリティ保護](securing-wcf-data-services.md)
- [WCF Data Services クライアント ライブラリ](wcf-data-services-client-library.md)
