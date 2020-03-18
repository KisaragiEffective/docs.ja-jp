---
title: アプリのデプロイ
ms.date: 03/30/2017
helpviewer_keywords:
- WPF applications [WPF], deployment
- deployment [WPF], applications
ms.assetid: 12cadca0-b32c-4064-9a56-e6a306dcc76d
ms.openlocfilehash: 9d4b7dd0464960441410d8ff2a196f0912354e5f
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741947"
---
# <a name="deploy-a-wpf-application"></a>WPF アプリケーションの配置

Windows Presentation Foundation (WPF) アプリケーションを構築した後は、アプリケーションを配置する必要があります。 Windows と .NET Framework には、いくつかの展開テクノロジが含まれています。 WPF アプリケーションの配置に使用される配置テクノロジは、アプリケーションの種類によって異なります。 このトピックでは、各配置テクノロジの概要と、各 WPF アプリケーションの種類の配置要件と組み合わせて使用する方法について説明します。

<a name="Deployment_Technologies"></a>   
## <a name="deployment-technologies"></a>配置テクノロジ  
 Windows と .NET Framework には、次のようないくつかの展開テクノロジが含まれています。  
  
- XCopy による配置。  
  
- Windows インストーラーの展開。  
  
- ClickOnce 配置。  
  
<a name="XCopy_Deployment"></a>   
### <a name="xcopy-deployment"></a>XCopy による配置  
 XCopy による配置とは、XCopy コマンド ライン プログラムを使用して、ある場所から別の場所へファイルをコピーすることです。 XCopy による配置は、次のような状況に適しています。  
  
- アプリケーションは自己完結型である。 実行するためにクライアントを更新する必要がない。  
  
- アプリケーションファイルを別の場所に移動する必要があります。たとえば、ビルドの場所 (ローカルディスク、UNC ファイル共有など) から発行場所 (Web サイト、UNC ファイル共有など) に移動します。  
  
- アプリケーションはシェル統合 ([スタート] メニューのショートカット、デスクトップ アイコンなど) を必要としない。  
  
 XCopy は、単純な配置シナリオには適していますが、複雑な配置機能が必要なシナリオには十分に対応できません。 特に、配置を堅牢な方法で管理する場合、XCopy を使用すると、スクリプトの作成、実行、および維持というオーバーヘッドが生じます。 さらに、XCopy は、バージョン管理、アンインストール、およびロールバックをサポートしません。  
  
<a name="Windows_Installer"></a>   
### <a name="windows-installer"></a>Windows インストーラー●windowsいんすとーら○  
 Windows インストーラーを使用すると、アプリケーションをクライアントに簡単に配布して実行できる、自己完結型の実行可能ファイルとしてパッケージ化できます。 さらに、Windows インストーラーは Windows と共にインストールされ、デスクトップ、[スタート] メニュー、および [プログラム] コントロールパネルとの統合を可能にします。  
  
 Windows インストーラーを使用すると、アプリケーションのインストールとアンインストールを簡単に行うことができますが、インストールされているアプリケーションをバージョン管理の観点から最新の状態に保つための機能は提供されません。  
  
 Windows インストーラーの詳細については、「 [Windows インストーラーの展開](/visualstudio/deployment/deploying-applications-services-and-components#create-an-installer-package-windows-desktop)」を参照してください。
  
<a name="ClickOnce_Deployment"></a>   
### <a name="clickonce-deployment"></a>ClickOnce 配置  
 ClickOnce を使用すると、Web 以外のアプリケーションに対して Web スタイルのアプリケーションを配置できます。 アプリケーションは、Web サーバーまたはファイル サーバーに公開され、これらのサーバーから配置されます。 ClickOnce は、Windows インストーラーインストールされているアプリケーションが実行するクライアント機能のすべてをサポートしているわけではありませんが、次のようなサブセットをサポートしています。  
  
- [スタート] メニューおよび [プログラム] コントロール パネルとの統合。  
  
- バージョン管理、ロールバック、およびアンインストール。  
  
- アプリケーションを常に配置場所から起動するオンライン インストール モード。  
  
- 新しいバージョンがリリースされたときの自動更新。  
  
- ファイル拡張子の登録。  
  
 ClickOnce の詳細については、「 [clickonce のセキュリティと配置](/visualstudio/deployment/clickonce-security-and-deployment)」を参照してください。  
  
<a name="Deploying_WPF_Applications"></a>   
## <a name="deploying-wpf-applications"></a>WPF アプリケーションの配置  
 WPF アプリケーションの配置オプションは、アプリケーションの種類によって異なります。 配置の観点から見ると、WPF には次の3つの重要なアプリケーションの種類があります。  
  
- スタンドアロン アプリケーション。  
  
- マークアップのみの [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] アプリケーション。  
  
- XAML ブラウザーアプリケーション (Xbap)。  
  
<a name="Deploying_Standalone_Applications"></a>   
### <a name="deploying-standalone-applications"></a>スタンドアロン アプリケーションの配置  
 スタンドアロンアプリケーションは、ClickOnce または Windows インストーラーを使用してデプロイされます。 いずれの場合も、スタンドアロン アプリケーションを実行するには、アプリケーションが完全に信頼されている必要があります。 完全信頼は、Windows インストーラーを使用して展開されたスタンドアロンアプリケーションに自動的に付与されます。 ClickOnce を使用して配置されるスタンドアロンアプリケーションには、完全な信頼が自動的に付与されるわけではありません。 代わりに、ClickOnce によって、スタンドアロンアプリケーションをインストールする前にユーザーが受け入れる必要があるセキュリティ警告ダイアログが表示されます。 受け入れた場合、スタンドアロン アプリケーションがインストールされ、完全な信頼が付与されます。 受け入れなかった場合、スタンドアロン アプリケーションはインストールされません。  
  
<a name="Deploying_Markup_Only_XAML_Applications"></a>   
### <a name="deploying-markup-only-xaml-applications"></a>マークアップのみの XAML アプリケーションの配置  
 マークアップのみの [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページは、通常、HTML ページなどの Web サーバーに発行され、Internet Explorer を使用して表示できます。 マークアップのみの [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページは、部分信頼セキュリティ サンドボックス内で実行され、インターネット ゾーン アクセス許可セットによって定義された制約が適用されます。 これにより、HTML ベースの Web アプリケーションに対する同等のセキュリティサンドボックスが提供されます。  
  
 WPF アプリケーションのセキュリティの詳細については、「[セキュリティ](../security-wpf.md)」を参照してください。  
  
 マークアップのみの [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページは、XCopy または Windows インストーラーのいずれかを使用してローカルファイルシステムにインストールできます。 これらのページは、Internet Explorer またはエクスプローラーを使用して表示できます。  
  
 XAML の詳細については、「[XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)」を参照してください。  
  
<a name="Deploying_XAML_Browser_Applications"></a>   
### <a name="deploying-xaml-browser-applications"></a>XAML ブラウザー アプリケーションの配置  
 Xbap は、次の3つのファイルを配置する必要があるコンパイル済みアプリケーションです。  
  
- *ApplicationName*.exe: 実行可能アセンブリのアプリケーション ファイル。  
  
- *ApplicationName*.xbap: 配置マニフェスト。  
  
- *ApplicationName*.exe.manifest: アプリケーション マニフェスト。  
  
> [!NOTE]
> 配置マニフェストおよびアプリケーション マニフェストの詳細については、「[WPF アプリケーションのビルド](building-a-wpf-application-wpf.md)」を参照してください。  
  
 これらのファイルは、XBAP がビルドされるときに生成されます。 詳細については、「[方法: 新しい WPF ブラウザー アプリケーション プロジェクトを作成する](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/bb628663(v=vs.100))」を参照してください。 マークアップのみの [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページと同様に、Xbap は通常、Web サーバーに発行され、Internet Explorer を使用して表示されます。  
  
 Xbap は、配置手法のいずれかを使用してクライアントに配置できます。 ただし、次の機能が用意されているため、ClickOnce を使用することをお勧めします。  
  
1. 新しいバージョンが公開されたときの自動更新。  
  
2. 完全信頼で実行されている XBAP の昇格権限。  
  
 既定では、ClickOnce は、.deploy 拡張子を持つアプリケーション ファイルを公開します。 これは問題になる可能性がありますが、無効にできます。 詳細については、「[ClickOnce 配置でのサーバーおよびクライアント構成の問題](/visualstudio/deployment/server-and-client-configuration-issues-in-clickonce-deployments)」を参照してください。  
  
 XAML ブラウザーアプリケーション (Xbap) の配置の詳細については、「 [WPF Xaml ブラウザーアプリケーションの概要](wpf-xaml-browser-applications-overview.md)」を参照してください。  
  
<a name="Installing__NET_Framework_3_0"></a>   
## <a name="installing-the-net-framework"></a>.NET Framework のインストール  
 WPF アプリケーションを実行するには、クライアントに Microsoft .NET Framework がインストールされている必要があります。 Internet Explorer は、WPF ブラウザーでホストされるアプリケーションを表示するときに、クライアントが .NET Framework と共にインストールされるかどうかを自動的に検出します。 .NET Framework がインストールされていない場合、Internet Explorer はユーザーにインストールを促すメッセージを表示します。  
  
 .NET Framework がインストールされているかどうかを検出するために、Internet Explorer には、次の拡張子を持つコンテンツファイルのフォールバック Multipurpose Internet Mail Extensions (MIME) ハンドラーとして登録されたブートストラップアプリケーションが含まれています。 .xaml、.xps、xbap、、および. アプリケーション。 これらのファイルの種類に移動したときに、.NET Framework がクライアントにインストールされていない場合、ブートストラップアプリケーションは、インストールするためのアクセス許可を要求します。 アクセス許可が指定されていない場合は、.NET Framework もアプリケーションもインストールされません。  
  
 アクセス許可が付与されている場合、Internet Explorer は Microsoft バックグラウンドインテリジェント転送サービス (BITS) を使用して .NET Framework をダウンロードしてインストールします。 .NET Framework が正常にインストールされると、最初に要求されたファイルが新しいブラウザーウィンドウで開かれます。  
  
 詳細については、「[.NET Framework およびアプリケーションの配置](../../deployment/index.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [WPF アプリケーションのビルド](building-a-wpf-application-wpf.md)
- [セキュリティ](../security-wpf.md)
