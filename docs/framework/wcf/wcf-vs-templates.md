---
title: WCF Visual Studio テンプレート
ms.date: 03/30/2017
ms.assetid: 6a608575-3535-4190-89da-911e24c8374f
ms.openlocfilehash: ec73036921632bc855e79239f1fc578587de7ca3
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802345"
---
# <a name="wcf-visual-studio-templates"></a>WCF Visual Studio テンプレート
Windows Communication Foundation (WCF) Visual Studio テンプレートは、Visual Studio で使用できる事前定義されたプロジェクトおよび項目テンプレートで、WCF サービスと周囲のアプリケーションをすばやく構築できます。  
  
## <a name="using-the-wcf-templates"></a>WCF テンプレートの使用  
 WCF Visual Studio テンプレートは、サービス開発のための基本クラス構造を提供します。 これには、サービス コントラクト、データ コントラクト、サービス実装、および構成の基本的な定義が含まれます。 これらのテンプレートを使用すると、最小限のコードを使用した単純なサービスや、より高度なサービスのビルド ブロックを作成できます。  
  
### <a name="wcf-service-library-project-template"></a>WCF サービス ライブラリ プロジェクト テンプレート  
 WCF サービスライブラリプロジェクトテンプレートは、新しいプロジェクト ダイアログボックスの  **visual C#** 、Wcf、 **visual basic**、wcf で使用できます。  
  
 **WCF サービス**テンプレートを使用して新しいプロジェクトを作成すると、新しいプロジェクトには次の3つのファイルが自動的に追加されます。  
  
- サービス コントラクト ファイル (IService1.cs または IService1.vb)。 サービスコントラクトファイルは、WCF サービス属性が適用されたインターフェイスです。 このファイルには、サービスの定義方法を示す単純なサービスの定義が含まれています。その他に、パラメーター ベースの操作や、単純なデータ コントラクトのサンプルも含まれています。 これは、WCF サービスプロジェクトの作成後にコードエディターに表示される既定のファイルです。  
  
- サービス実装ファイル (Service1.cs または Service1.vb)。 サービス実装ファイルは、サービス コントラクト ファイルに定義されているコントラクトを実装します。  
  
- アプリケーション構成ファイル (App.config)。 構成ファイルは、セキュリティで保護された HTTP バインディングを使用して、WCF サービスモデルの基本的な要素を提供します。 サービスのエンドポイントも含まれており、メタデータの交換も可能です。  
  
> [!NOTE]
> Visual Studio は、既定の構成である[WCF サービスホスト (wcfsvchost.exe)](wcf-service-host-wcfsvchost-exe.md)を使用して実行されるときに、プロジェクトの構成ファイルとして app.config ファイルを認識するように構成されています。 サービス ライブラリを実行可能ファイルでホストする場合は、DLL の構成ファイルは無効になるため、その実行可能ファイルの構成ファイルに構成コードを移動する必要があります。  
  
### <a name="wcf-service-application-template"></a>WCF サービス アプリケーション テンプレート  
 WCF サービスアプリケーションテンプレートは、新しいプロジェクト ダイアログボックスの  **visual C#** 、Wcf、 **visual basic**、wcf で使用できます。  
  
 **WCF Web アプリケーションサービス**テンプレートを使用して新しいプロジェクトを作成する場合、プロジェクトには次の4つのファイルが含まれます。  
  
- サービス ホスト ファイル (service1.svc)。  
  
- サービス コントラクト ファイル (IService1.cs または IService1.vb)。  
  
- サービス実装ファイル (Service1.svc.cs または Service1.svc.vb)。  
  
- Web 構成ファイル (Web.config)。  
  
 このテンプレートでは、自動的に Web サイトが作成されて (仮想ディレクトリに配置されます)、サービスがホストされます。  
  
### <a name="wcf-web-site-template"></a>WCF Web サイト テンプレート  
 WCF Web サイトテンプレートは、新しいプロジェクト ダイアログボックスの  **visual C#\Web Site\WCF service** と **visual Basic\Web Site\WCF service** で使用できます。 これによって、WCF サービス アプリケーションのテンプレートと同じファイルが作成されますが、ASP.NET Web サイトであるかのように編成されます。 App_Code フォルダーと App_Data フォルダーが作成されます。  
  
### <a name="wcf-service-item-template"></a>WCF サービス項目テンプレート  
 WCF サービス項目テンプレートは、既存の Visual Studio プロジェクトに WCF サービスを簡単に追加できるようにするカスタムテンプレートです。  
  
 このテンプレートを使用するには、**ソリューションエクスプローラー**ウィンドウにアクセスし、プロジェクト名を右クリックして **[追加]** をポイントし、 **[新しい項目]** をクリックして 新しい項目の **[追加]** ダイアログボックスを開きます。  
  
 サービス インターフェイス ファイルとサービス実装ファイルがルート プロジェクト フォルダーに配置されます。  
  
 このテンプレートでは、新しいサービスの構成セクションが既存の構成ファイルと互換性がある場合にそれらがマージされます。  
  
 既存のプロジェクトが Web プロジェクトの場合は、サービス ホスト ファイル (service1.svc) も作成されます。  
  
### <a name="wcf-wf-service-project-and-item-template"></a>WCF WF サービス プロジェクト/項目テンプレート  
 これらのテンプレートは、ワークフローサービスをホストする WCF サービスを作成します。これは、web サービスのようにアクセスできるワークフローです。 この他に、XAML プログラミング モデルや命令型プログラミング モデルのテンプレートも用意されています。 これらのテンプレートを使用すると、シーケンシャル ワークフローやステート マシン ワークフローを作成できます。 これらの種類のワークフローの詳細については、「[方法: ワークフローを作成](../windows-workflow-foundation/how-to-create-a-workflow.md)する」を参照してください。 ワークフロープロジェクトの作成の詳細については、「[従来のワークフロープロジェクトの作成](/visualstudio/workflow-designer/developing-applications-with-the-workflow-designer)」を参照してください。  
  
 コードベースのワークフローではなく、XOML 型のワークフローを使用すると、Visual Studio デザイナーの応答性が向上します。 XOML ワークフローは、既定で作成されるワークフロー型です。  
  
### <a name="wcf-syndication-service-library-template"></a>WCF 配信サービス ライブラリ テンプレート  
 このテンプレートを使用すると、RSS または ATOM 形式のフィードを WCF サービスとして公開できます。 詳細については、「 [WCF 配信](./feature-details/wcf-syndication.md)」を参照してください。  
  
#### <a name="changing-the-address-of-the-feed"></a>フィードのアドレスの変更  
 配信テンプレートは、実行中に Internet Explorer を使用します。 Visual Studio の**ソリューションエクスプローラー**でプロジェクトを右クリックし、 **[プロパティ]** を選択し、 **[デバッグ]** タブを選択すると、テンプレートの既定のアドレスが表示されます。 Internet Explorer は、このアドレスにあるフィードを開きます。  
  
 フィードのアドレスを変更する場合は、 **[デバッグ]** タブのアドレスも変更する必要があります。この操作を行わないと、Internet Explorer は既定のアドレスでフィードを開こうとして失敗します。  
  
### <a name="ajax-enabled-wcf-service-item-template"></a>AJAX 対応 WCF サービス項目テンプレート  
 このテンプレートは、AJAX コントロールを WCF サービスとして公開します。 AJAX コントロールの詳細については、 [ajax コントロールのドキュメント](https://docs.microsoft.com/aspnet/ajax/)を参照してください。  
  
### <a name="silverlight-enabled-wcf-service-item-template"></a>Silverlight 対応 WCF サービス項目テンプレート  
 このテンプレートは、Silverlight クライアントまたはフロントエンドにデータを提供する Web サービスを作成します。 このテンプレートを Web サイトまたは Web アプリケーションプロジェクトに追加すると、WCF サービスを作成できます。これには、Silverlight クライアントとの通信をサポートするサービスコードと構成が含まれます。 次に、**サービス参照の追加**を使用して、クライアントにサービスのクライアントプロキシを追加し、silverlight クライアントと SILVERLIGHT 対応 WCF サービスの間でデータを交換できます。  
  
 このテンプレートにアクセスするには、**ソリューションエクスプローラー**で web サイトまたは web アプリケーションプロジェクトを右クリックし、 **[新しいアイテムの追加]** をクリックして、 **[Silverlight 対応 WCF サービス]** をクリックします。  
  
> [!NOTE]
> Silverlight 対応 WCF サービスは、セキュリティ設定を一切有効にせずに `basicHttpBinding` エンドポイントを公開します。 したがって、サービスに接続しているすべてのクライアントが、このサービスに関する情報を取得できることになります。 また、サービスとクライアント間で交換されるメッセージの署名と暗号化も行われません。 エンドポイントを正しくセキュリティで保護するには、ASP.NET 認証や HTTPS などのメカニズムを使用する必要があります。  
  
## <a name="see-also"></a>参照

- [WCF サービス ホスト (WcfSvcHost.exe)](wcf-service-host-wcfsvchost-exe.md)
- [WCF のテスト用クライアント (WcfTestClient.exe)](wcf-test-client-wcftestclient-exe.md)
