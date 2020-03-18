---
title: セキュリティ
ms.date: 03/30/2017
helpviewer_keywords:
- XAML files [WPF], sandbox behavior
- browser-hosted application security [WPF]
- application security [WPF]
- navigation security [WPF]
- loose XAML files [WPF], sandbox behavior
- WPF security [WPF]
- WebBrowser control [WPF], security
- feature controls [WPF], security
- XBAP security [WPF]
- Internet Explorer security settings [WPF]
ms.assetid: ee1baea0-3611-4e36-9ad6-fcd5205376fb
ms.openlocfilehash: a49634fd955b0dc1f4cac5c785d49c24d16bbc60
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76868045"
---
# <a name="security-wpf"></a>セキュリティ (WPF)
<a name="introduction"></a>Windows Presentation Foundation (WPF) のスタンドアロンアプリケーションとブラウザーでホストされるアプリケーションを開発する場合は、セキュリティモデルを考慮する必要があります。 WPF スタンドアロンアプリケーションは、Windows インストーラー (.msi)、XCopy、または ClickOnce を使用してデプロイされているかどうかにかかわらず、無制限のアクセス許可 (CAS**FullTrust**アクセス許可セット) で実行されます。 部分的に信頼されたスタンドアロンの WPF アプリケーションを ClickOnce で展開することはサポートされていません。 ただし、完全に信頼されたホストアプリケーションでは、.NET Framework アドインモデルを使用して部分信頼 <xref:System.AppDomain> を作成できます。 詳細については、「 [WPF アドインの概要](./app-development/wpf-add-ins-overview.md)」を参照してください。  
  
 WPF ブラウザーでホストされるアプリケーションは、Windows Internet Explorer または Firefox によってホストされ、XAML ブラウザーアプリケーション (Xbap) またはルース [!INCLUDE[TLA#tla_xaml](../../../includes/tlasharptla-xaml-md.md)] ドキュメントのいずれかになります。詳細については、「 [WPF XAML ブラウザーアプリケーションの概要](./app-development/wpf-xaml-browser-applications-overview.md)」を参照してください。  
  
 WPF ブラウザーでホストされるアプリケーションは、既定では、既定の CAS**インターネット**ゾーンのアクセス許可セットに制限されている部分信頼セキュリティサンドボックス内で実行されます。 これにより、一般的な Web アプリケーションが分離されるのと同じ方法で、WPF ブラウザーでホストされるアプリケーションがクライアントコンピューターから効果的に分離されます。 XBAP は、デプロイメント URL およびクライアントのセキュリティ構成のセキュリティ ゾーンに基づいて、完全な信頼まで特権を昇格することができます。 詳細については、「 [WPF 部分信頼セキュリティ](wpf-partial-trust-security.md)」を参照してください。  
  
 このトピックでは、Windows Presentation Foundation (WPF) のスタンドアロンアプリケーションとブラウザーでホストされるアプリケーションのセキュリティモデルについて説明します。  
  
 このトピックは、次のセクションで構成されています。  
  
- [安全なナビゲーション](#SafeTopLevelNavigation)  
  
- [Web ブラウザーのセキュリティ設定](#InternetExplorerSecuritySettings)  
  
- [WebBrowser コントロールと機能コントロール](#webbrowser_control_and_feature_controls)  
  
- [部分信頼クライアント アプリケーションに対する APTCA の無効化](#APTCA)  
  
- [Loose XAML ファイルに対するサンドボックスの動作](#LooseContentSandboxing)  
  
- [セキュリティを向上する WPF アプリケーションを開発するためのリソース](#BestPractices)  
  
<a name="SafeTopLevelNavigation"></a>   
## <a name="safe-navigation"></a>安全なナビゲーション  
 Xbap の場合、WPF はアプリケーションとブラウザーという2種類のナビゲーションを区別します。  
  
 *アプリケーション ナビゲーション*は、ブラウザーによってホストされるアプリケーション内のコンテンツ項目間のナビゲーションです。 *ブラウザー ナビゲーション*は、ブラウザー自体のコンテンツとロケーション URL を変更するナビゲーションです。 アプリケーションナビゲーション (通常は XAML) とブラウザーナビゲーション (通常は HTML) の関係を次の図に示します。
  
 ![アプリケーションナビゲーションとブラウザーナビゲーションの関係。](./media/security-wpf/application-browser-navigation-relationship.png)  
  
 XBAP が移動先として安全であると見なされるコンテンツの種類は、主にアプリケーションナビゲーションまたはブラウザーナビゲーションが使用されているかどうかによって決まります。  
  
<a name="Application_Navigation_Security"></a>   
### <a name="application-navigation-security"></a>アプリケーション ナビゲーションのセキュリティ  
 アプリケーションのナビゲーションは、次の4種類のコンテンツをサポートするパック URI で識別できる場合は安全と見なされます。  
  
|コンテンツ タイプ|説明|URI の例|  
|------------------|-----------------|-----------------|  
|Resource|ビルドの種類が**リソース**のプロジェクトに追加されるファイル。|`pack://application:,,,/MyResourceFile.xaml`|  
|Content|ビルドの種類が**コンテンツ**のプロジェクトに追加されるファイル。|`pack://application:,,,/MyContentFile.xaml`|  
|起点サイト|ビルドの種類が**None**のプロジェクトに追加されるファイル。|`pack://siteoforigin:,,,/MySiteOfOriginFile.xaml`|  
|アプリケーション コード|コンパイルされたコード分離を含む XAML リソース。<br /><br /> -または-<br /><br /> **ページ**のビルドの種類を使用してプロジェクトに追加される XAML ファイル。|`pack://application:,,,/MyResourceFile` `.xaml`|  
  
> [!NOTE]
> アプリケーションデータファイルとパック Uri の詳細については、「 [WPF アプリケーションのリソースファイル、コンテンツファイル、およびデータファイル](./app-development/wpf-application-resource-content-and-data-files.md)」を参照してください。  
  
 これらのコンテンツ タイプのファイルは、ユーザーまたはプログラムを使用して移動できます。  
  
- **ユーザー ナビゲーション**。 ユーザーは <xref:System.Windows.Documents.Hyperlink> 要素をクリックして移動します。  
  
- **プログラム ナビゲーション**。 アプリケーションは、ユーザーを関与させずに移動します。たとえば、<xref:System.Windows.Navigation.NavigationWindow.Source%2A?displayProperty=nameWithType> プロパティを設定します。  
  
<a name="Browser_Navigation_Security"></a>   
### <a name="browser-navigation-security"></a>ブラウザー ナビゲーションのセキュリティ  
 ブラウザー ナビゲーションは、次の条件の下でのみ安全と見なされます。  
  
- **ユーザー ナビゲーション**。 ユーザーは、入れ子になった <xref:System.Windows.Controls.Frame>ではなく、メイン <xref:System.Windows.Navigation.NavigationWindow>内にある <xref:System.Windows.Documents.Hyperlink> 要素をクリックして移動します。  
  
- **ゾーン**。 移動先のコンテンツが、インターネットまたはローカル イントラネット上に存在する。  
  
- **プロトコル**。 使用されるプロトコルは、 **http**、 **https**、 **file**、または**mailto**のいずれかです。  
  
 XBAP が、これらの条件に準拠していない方法でコンテンツに移動しようとすると、<xref:System.Security.SecurityException> がスローされます。  
  
<a name="InternetExplorerSecuritySettings"></a>   
## <a name="web-browsing-software-security-settings"></a>Web ブラウザーのセキュリティ設定  
 コンピューターのセキュリティ設定によって、Web ブラウザーに付与されるアクセス権が決まります。 Web ブラウザーソフトウェアには、 [WinINet](/windows/win32/wininet/portal)または[UrlMon](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/aa767916(v=vs.85)) api を使用するアプリケーションまたはコンポーネントが含まれます (Internet Explorer とプレゼンテーションの cluster.exe を含む)。  
  
 Internet Explorer には、次のような、または Internet Explorer からの実行が許可されている機能を構成するためのメカニズムが用意されています。  
  
- .NET Framework 依存コンポーネント  
  
- ActiveX コントロールおよびプラグイン  
  
- ダウンロード  
  
- スクリプト  
  
- ユーザーの認証  
  
 この方法で保護できる機能のコレクションは、**インターネット**、**イントラネット**、**信頼済みサイト**、および**制限付きサイト**ゾーンのゾーン単位で構成されます。 次の手順では、セキュリティ設定の構成方法について説明します。  
  
1. **[コントロール パネル]** を開きます。  
  
2. **[ネットワークとインターネット]** をクリックし、 **[インターネットオプション]** をクリックします。  
  
     [インターネット オプション] ダイアログ ボックスが表示されます。  
  
3. **[セキュリティ]** タブで、セキュリティ設定を構成するゾーンを選択します。  
  
4. **[レベルのカスタマイズ]** ボタンをクリックします。  
  
     **[セキュリティの設定]** ダイアログボックスが表示され、選択したゾーンのセキュリティ設定を構成できます。  
  
     ![[セキュリティの設定] ダイアログボックスを表示するスクリーンショット。](./media/security-wpf/windows-presentation-foundation-security-settings.png)  
  
> [!NOTE]
> [インターネット オプション] ダイアログ ボックスは、Internet Explorer から開くこともできます。 **[ツール]** をクリックし、 **[インターネットオプション]** をクリックします。  
  
 Windows Internet Explorer 7 以降では、.NET Framework に特化した次のセキュリティ設定が含まれています。  
  
- **Loose XAML**。 Internet Explorer が [!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)] ファイルに移動できるかどうかを制御します。 ([有効]、[無効]、および [ダイアログを表示する] オプション)。  
  
- **XAML ブラウザー アプリケーション**。 Internet Explorer が Xbap に移動して実行できるかどうかを制御します。 ([有効]、[無効]、および [ダイアログを表示する] オプション)。  
  
 既定では、これらの設定はすべて、**インターネット**、**ローカルイントラネット**、および**信頼済みサイト**ゾーンで有効になっています。また、**制限付きサイト**ゾーンでは無効になっています。  
  
<a name="Security_Settings_for_IE6_and_Below"></a>   
### <a name="security-related-wpf-registry-settings"></a>セキュリティ関連の WPF レジストリの設定  
 [インターネット オプション] から使用できるセキュリティ設定以外に、セキュリティ上重要なさまざまな WPF 機能を選択的にブロックするために次のレジストリ値を使用できます。 値は次のキーで定義されます。  
  
 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\Windows Presentation Foundation\Features`  
  
 設定可能な値を次の表に示します。  
  
|［値の名前］|値型|値のデータ|  
|----------------|----------------|----------------|  
|XBAPDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
|LooseXamlDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
|WebBrowserDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
|MediaAudioDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
|MediaImageDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
|MediaVideoDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
|ScriptInteropDisallow|REG_DWORD|許可しない場合は 1、許可する場合は 0。|  
  
<a name="webbrowser_control_and_feature_controls"></a>   
## <a name="webbrowser-control-and-feature-controls"></a>WebBrowser コントロールと機能コントロール  
 WPF <xref:System.Windows.Controls.WebBrowser> コントロールを使用して、Web コンテンツをホストできます。 WPF <xref:System.Windows.Controls.WebBrowser> コントロールは、基になる WebBrowser ActiveX コントロールをラップします。 Wpf <xref:System.Windows.Controls.WebBrowser> コントロールを使用して信頼されていない Web コンテンツをホストする場合、WPF はアプリケーションをセキュリティで保護するためのサポートを提供します。 ただし、一部のセキュリティ機能は、<xref:System.Windows.Controls.WebBrowser> コントロールを使用して、アプリケーションによって直接適用される必要があります。 WebBrowser ActiveX コントロールの詳細については、「 [Webbrowser コントロールの概要とチュートリアル](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/aa752041(v=vs.85))」を参照してください。  
  
> [!NOTE]
> このセクションは、<xref:System.Windows.Controls.WebBrowser> を使用して HTML コンテンツに移動するため、<xref:System.Windows.Controls.Frame> コントロールにも適用されます。  
  
 WPF <xref:System.Windows.Controls.WebBrowser> コントロールを使用して信頼されていない Web コンテンツをホストしている場合は、アプリケーションが部分信頼の <xref:System.AppDomain> を使用して、悪意のある可能性のある HTML スクリプトコードからアプリケーションコードを隔離する必要があります。 これは、アプリケーションが <xref:System.Windows.Controls.WebBrowser.InvokeScript%2A> メソッドと <xref:System.Windows.Controls.WebBrowser.ObjectForScripting%2A> プロパティを使用してホストされているスクリプトと対話する場合に特に当てはまります。 詳細については、「 [WPF アドインの概要](./app-development/wpf-add-ins-overview.md)」を参照してください。  
  
 アプリケーションで WPF <xref:System.Windows.Controls.WebBrowser> コントロールを使用する場合は、セキュリティを強化し、攻撃を軽減するもう1つの方法は、Internet Explorer の機能コントロールを有効にすることです。 機能コントロールは Internet Explorer に追加されており、管理者および開発者はこのコントロールを使用して、WPF <xref:System.Windows.Controls.WebBrowser> コントロールをラップする WebBrowser ActiveX コントロールをホストする Internet Explorer とアプリケーションの機能を構成できます。 機能コントロールは、 [Cointernetsetfeatureenabled](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537168(v=vs.85))関数を使用するか、レジストリの値を変更することによって構成できます。 機能コントロールの詳細については、「[機能コントロール](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537184(v=vs.85))と[インターネット機能コントロール](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/general-info/ee330720(v=vs.85))の概要」を参照してください。  
  
 WPF <xref:System.Windows.Controls.WebBrowser> コントロールを使用するスタンドアロン WPF アプリケーションを開発している場合、WPF は、アプリケーションに対して次の機能コントロールを自動的に有効にします。  
  
|機能コントロール|  
|---------------------|  
|FEATURE_MIME_HANDLING|  
|FEATURE_MIME_SNIFFING|  
|FEATURE_OBJECT_CACHING|  
|FEATURE_SAFE_BINDTOOBJECT|  
|FEATURE_WINDOW_RESTRICTIONS|  
|FEATURE_ZONE_ELEVATION|  
|FEATURE_RESTRICT_FILEDOWNLOAD|  
|FEATURE_RESTRICT_ACTIVEXINSTALL|  
|FEATURE_ADDON_MANAGEMENT|  
|FEATURE_HTTP_USERNAME_PASSWORD_DISABLE|  
|FEATURE_SECURITYBAND|  
|FEATURE_UNC_SAVEDFILECHECK|  
|FEATURE_VALIDATE_NAVIGATE_URL|  
|FEATURE_DISABLE_TELNET_PROTOCOL|  
|FEATURE_WEBOC_POPUPMANAGEMENT|  
|FEATURE_DISABLE_LEGACY_COMPRESSION|  
|FEATURE_SSLUX|  
  
 これらの機能コントロールは無条件で有効になるため、完全信頼アプリケーションに悪影響が及ぶ場合があります。 この場合、特定のアプリケーションとそのアプリケーションがホストしているコンテンツにセキュリティ上のリスクがなければ、対応する機能コントロールを無効にできます。  
  
 機能コントロールは、WebBrowser ActiveX オブジェクトをインスタンス化するプロセスによって適用されます。 そのため、信頼されていないコンテンツに移動できるスタンドアロン アプリケーションを作成する場合は、その他の機能コントロールを有効にすることを検討すべきです。  
  
> [!NOTE]
> この推奨事項は、MSHTML および SHDOCVW ホスト セキュリティの一般的な推奨事項に基づいています。 詳細については、「 [Mshtml ホストのセキュリティに関する faq: パート I ii](https://msrc-blog.microsoft.com/2009/04/02/the-mshtml-host-security-faq-part-i-of-ii/) 」と「 [Mshtml ホストのセキュリティに関する FAQ: 第2部](https://msrc-blog.microsoft.com/2009/04/03/the-mshtml-host-security-faq-part-ii-of-ii/)」を参照してください。  
  
 実行可能ファイルでは、レジストリ値を 1 に設定して以下の機能コントロールを有効にすることを検討してください。  
  
|機能コントロール|  
|---------------------|  
|FEATURE_ACTIVEX_REPURPOSEDETECTION|  
|FEATURE_BLOCK_LMZ_IMG|  
|FEATURE_BLOCK_LMZ_OBJECT|  
|FEATURE_BLOCK_LMZ_SCRIPT|  
|FEATURE_RESTRICT_RES_TO_LMZ|  
|FEATURE_RESTRICT_ABOUT_PROTOCOL_IE7|  
|FEATURE_SHOW_APP_PROTOCOL_WARN_DIALOG|  
|FEATURE_LOCALMACHINE_LOCKDOWN|  
|FEATURE_FORCE_ADDR_AND_STATUS|  
|FEATURE_RESTRICTED_ZONE_WHEN_FILE_NOT_FOUND|  
  
 実行可能ファイルでは、レジストリ値を 0 に設定して以下の機能コントロールを無効にすることを検討してください。  
  
|機能コントロール|  
|---------------------|  
|FEATURE_ENABLE_SCRIPT_PASTE_URLACTION_IF_PROMPT|  
  
 Windows Internet Explorer で WPF <xref:System.Windows.Controls.WebBrowser> コントロールを含む部分信頼 XAML ブラウザーアプリケーション (XBAP) を実行すると、WPF は Internet Explorer プロセスのアドレス空間で WebBrowser ActiveX コントロールをホストします。 WebBrowser ActiveX コントロールは Internet Explorer プロセスでホストされているため、Internet Explorer のすべての機能コントロールが WebBrowser ActiveX コントロールに対しても有効になります。  
  
 Internet Explorer で実行されている XBAP にも、標準のスタンドアロン アプリケーションよりも高いレベルのセキュリティが適用されます。 この追加のセキュリティは、Internet Explorer (したがって、WebBrowser ActiveX コントロール) が Windows Vista および Windows 7 では既定で保護モードで実行されるためです。 保護モードの詳細については、「 [Internet Explorer の保護モードの概要と操作](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/)」を参照してください。  
  
> [!NOTE]
> インターネットゾーン内で、WPF <xref:System.Windows.Controls.WebBrowser> コントロールを含む XBAP を Firefox で実行しようとすると、<xref:System.Security.SecurityException> がスローされます。 これは、WPF セキュリティ ポリシーが原因です。  
  
<a name="APTCA"></a>   
## <a name="disabling-aptca-assemblies-for-partially-trusted-client-applications"></a>部分信頼クライアント アプリケーションに対する APTCA の無効化  
 マネージアセンブリがグローバルアセンブリキャッシュ (GAC) にインストールされると、ユーザーはそれらをインストールするための明示的なアクセス許可を提供する必要があるため、完全に信頼されます。 完全に信頼されているため、これらを使用できるのは完全信頼マネージド クライアント アプリケーションのみです。 部分的に信頼されたアプリケーションでそれらを使用できるようにするには、<xref:System.Security.AllowPartiallyTrustedCallersAttribute> (APTCA) でマークする必要があります。 この属性は、部分信頼で実行しても安全であるとテストで確認されたアセンブリだけに設定します。  
  
 ただし、APTCA アセンブリを GAC にインストールした後にセキュリティの欠陥が発生する可能性があります。 セキュリティ上の欠陥が検出されたら、アセンブリの発行者は、既存のインストールでの問題を解決し、問題発見後に発生する可能性があるインストールに備えるため、セキュリティ更新プログラムを作成できます。 更新プログラムの 1 つのオプションとして、アセンブリのアンインストールが考えられますが、その場合はこのアセンブリを使用する他の完全信頼クライアント アプリケーションを破損するおそれがあります。  
  
 WPF には、APTCA アセンブリをアンインストールすることなく、部分的に信頼された Xbap で APTCA アセンブリを無効にできるメカニズムが用意されています。  
  
 APTCA アセンブリを無効にするには、特殊なレジストリ キーを作成する必要があります。  
  
 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\policy\APTCA\<AssemblyFullName>, FileVersion=<AssemblyFileVersion>`  
  
 次のコードは一例を示しています。  
  
 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\policy\APTCA\aptcagac, Version=1.0.0.0, Culture=neutral, PublicKeyToken=215e3ac809a0fea7, FileVersion=1.0.0.0`  
  
 このキーにより、APTCA アセンブリのエントリが設定されます。 また、アセンブリを有効または無効にする値をこのキーに作成する必要があります。 値の詳細を次に示します。  
  
- 値の名前: **APTCA_FLAG**。  
  
- 値の型: **REG_DWORD**。  
  
- 値のデータ: **1**を無効にします。有効にする場合は**0** 。  
  
 部分信頼クライアント アプリケーションに対してアセンブリを無効にする必要がある場合は、レジストリ キーおよび値を作成する更新プログラムを作成します。  
  
> [!NOTE]
> コア .NET Framework アセンブリは、マネージアプリケーションを実行するために必要なため、この方法で無効にしても影響を受けません。 APTCA アセンブリの無効化のサポートは、主にサードパーティ アプリケーションを対象にしたものです。  
  
<a name="LooseContentSandboxing"></a>   
## <a name="sandbox-behavior-for-loose-xaml-files"></a>Loose XAML ファイルに対するサンドボックスの動作  
 緩い [!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)] ファイルは、分離コード、イベントハンドラー、またはアプリケーション固有のアセンブリに依存しないマークアップのみの XAML ファイルです。 厳密でない [!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)] ファイルがブラウザーから直接移動されると、既定のインターネットゾーンのアクセス許可セットに基づいてセキュリティサンドボックスに読み込まれます。  
  
 ただし、このセキュリティ動作は、<xref:System.Windows.Navigation.NavigationWindow> またはスタンドアロンアプリケーション内の <xref:System.Windows.Controls.Frame> から移動した [!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)] ファイルを移動すると、異なります。  
  
 どちらの場合も、移動先の緩い [!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)] ファイルは、そのホストアプリケーションのアクセス許可を継承します。 ただし、この動作はセキュリティの観点から望ましくない可能性があります。特に、信頼されていないか不明なエンティティによって作成された [!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)] ファイルが緩い場合は、 この種類のコンテンツは*外部コンテンツ*として知られており、<xref:System.Windows.Controls.Frame> と <xref:System.Windows.Navigation.NavigationWindow> の両方を、移動時に分離するように構成できます。 分離を実現するには、次の例に示すように、 **SandboxExternalContent**プロパティを true に設定します。 <xref:System.Windows.Controls.Frame> および <xref:System.Windows.Navigation.NavigationWindow>を参照してください。  
  
 [!code-xaml[SecurityOverviewSnippets#FrameMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/SecurityOverviewSnippets/CS/Window2.xaml#framemarkup)]  
  
 [!code-xaml[SecurityOverviewSnippets#NavigationWindowMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/SecurityOverviewSnippets/CS/Window1.xaml#navigationwindowmarkup)]  
  
 このように設定すると、外部コンテンツは、アプリケーションをホストするプロセスとは異なるプロセスに読み込まれます。 このプロセスは既定のインターネット ゾーン アクセス許可セットに限定されており、ホスト アプリケーションとクライアント コンピューターから外部コンテンツを効果的に分離します。  
  
> [!NOTE]
> <xref:System.Windows.Navigation.NavigationWindow> またはスタンドアロンアプリケーションの <xref:System.Windows.Controls.Frame> のいずれかの [!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)] ファイルへの移動は、プレゼンテーションホストプロセスを含む WPF ブラウザーホスティングインフラストラクチャに基づいて実装されますが、コンテンツが Windows Vista および Windows 7 の Internet Explorer で直接読み込まれる場合 (プレゼンテーションホストを通じて)、セキュリティレベルは若干低くなります。 これは、Web ブラウザーを使用しているスタンドアロン WPF アプリケーションに、Internet Explorer の保護モード セキュリティ機能が追加されていないためです。  
  
<a name="BestPractices"></a>   
## <a name="resources-for-developing-wpf-applications-that-promote-security"></a>セキュリティを向上する WPF アプリケーションを開発するためのリソース  
 セキュリティを強化する WPF アプリケーションの開発に役立つその他のリソースを次に示します。  
  
|区分|Resource|  
|----------|--------------|  
|マネージド コード|[patterns & practices アプリケーション セキュリティ ガイダンス インデックス](https://docs.microsoft.com/previous-versions/msp-n-p/ff650760(v=pandp.10))|  
|CAS|[コード アクセス セキュリティ](../misc/code-access-security.md)|  
|ClickOnce|[ClickOnce のセキュリティと配置](/visualstudio/deployment/clickonce-security-and-deployment)|  
|WPF|[WPF 部分信頼セキュリティ](wpf-partial-trust-security.md)|  
  
## <a name="see-also"></a>関連項目

- [WPF 部分信頼セキュリティ](wpf-partial-trust-security.md)
- [WPF のセキュリティ方針 - プラットフォーム セキュリティ](wpf-security-strategy-platform-security.md)
- [WPF のセキュリティ方針 - セキュリティ エンジニアリング](wpf-security-strategy-security-engineering.md)
- [patterns & practices アプリケーション セキュリティ ガイダンス インデックス](https://docs.microsoft.com/previous-versions/msp-n-p/ff650760(v=pandp.10))
- [コード アクセス セキュリティ](../misc/code-access-security.md)
- [ClickOnce のセキュリティと配置](/visualstudio/deployment/clickonce-security-and-deployment)
- [XAML の概要 (WPF)](../../desktop-wpf/fundamentals/xaml.md)
