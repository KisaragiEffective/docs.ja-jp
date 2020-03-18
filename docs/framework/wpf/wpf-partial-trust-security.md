---
title: 部分信頼セキュリティ
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- partial trust security [WPF]
- detecting permissions [WPF]
- security settings for Internet Explorer [WPF]
- partial trust applications [WPF], debugging
- permissions [WPF], managing
- debugging partial trust applications [WPF]
- permissions [WPF], detecting
- feature security requirements [WPF]
- managing permissions [WPF]
ms.assetid: ef2c0810-1dbf-4511-babd-1fab95b523b5
ms.openlocfilehash: 0d9bbcc32eea49afc6ecc713b0cf005b4434a67d
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743330"
---
# <a name="wpf-partial-trust-security"></a>WPF 部分信頼セキュリティ
<a name="introduction"></a>一般に、インターネットアプリケーションは、重要なシステムリソースへの直接アクセスを制限して、悪意のある損害を防ぐ必要があります。 既定では、HTML およびクライアント側のスクリプト言語は、重要なシステムリソースにアクセスできません。 Windows Presentation Foundation (WPF) ブラウザーでホストされるアプリケーションは、ブラウザーから起動できるため、同様の制限のセットに準拠している必要があります。 これらの制限を適用するために、WPF はコードアクセスセキュリティ (CAS) と ClickOnce の両方に依存しています (「 [wpf のセキュリティ方針-プラットフォームセキュリティ](wpf-security-strategy-platform-security.md)」を参照してください)。 既定では、ブラウザーでホストされるアプリケーションは、インターネット、ローカルイントラネット、またはローカルコンピューターのどちらから起動するかに関係なく、インターネットゾーンの CA のアクセス許可セットを要求します。 すべてのアクセス許可のセットよりも少ないアプリケーションで実行されるアプリケーションは、部分信頼で実行されていると言います。  
  
 WPF には、部分信頼で安全に使用できるできるだけ多くの機能を確保するためのさまざまなサポートが用意されています。また、CA と共に、部分信頼プログラミングのサポートが追加されています。  
  
 このトピックには、次のセクションが含まれます。  
  
- [WPF 機能の部分信頼のサポート](#WPF_Feature_Partial_Trust_Support)  
  
- [部分信頼プログラミング](#Partial_Trust_Programming)  
  
- [アクセス許可の管理](#Managing_Permissions)  
  
<a name="WPF_Feature_Partial_Trust_Support"></a>   
## <a name="wpf-feature-partial-trust-support"></a>WPF 機能の部分信頼のサポート  
 次の表は、インターネットゾーンのアクセス許可セットの制限内で安全に使用できる Windows Presentation Foundation (WPF) の高度な機能を示しています。  
  
 表 1: 部分信頼で安全な WPF 機能  
  
|機能領域|機能|  
|------------------|-------------|  
|全般|ブラウザーウィンドウ<br /><br /> 起点サイトアクセス<br /><br /> & lt; (512 KB)<br /><br /> UIAutomation プロバイダー<br /><br /> コマンド実行<br /><br /> 入力方式エディター (IME)<br /><br /> タブレットのスタイラスとインク<br /><br /> マウスキャプチャイベントおよび移動イベントを使用した、シミュレートされたドラッグアンドドロップ<br /><br /> OpenFileDialog<br /><br /> XAML 逆シリアル化 (XamlReader 経由)|  
|Web 統合|ブラウザーダウンロードダイアログ<br /><br /> 最上位レベルのユーザーが開始したナビゲーション<br /><br /> mailto: リンク<br /><br /> Uniform Resource Identifier パラメーター<br /><br /> HTTPWebRequest<br /><br /> IFRAME でホストされている WPF コンテンツ<br /><br /> Frame を使用した同一サイトの HTML ページのホスト<br /><br /> WebBrowser を使用した同じサイトの HTML ページのホスト<br /><br /> Web サービス (ASMX)<br /><br /> Web サービス (Windows Communication Foundation を使用)<br /><br /> スクリプトの作成<br /><br /> ドキュメント オブジェクト モデル|  
|効果|2D と3D<br /><br /> アニメーション<br /><br /> メディア (起点サイトとクロスドメイン)<br /><br /> イメージング/オーディオ/ビデオ|  
|リード|FlowDocument<br /><br /> XPS ドキュメント<br /><br /> 埋め込み & システムフォント<br /><br /> CFF & TrueType フォント|  
|編集中|スペルチェック<br /><br /> RichTextBox<br /><br /> プレーンテキストとインククリップボードのサポート<br /><br /> ユーザーが開始した貼り付け<br /><br /> 選択したコンテンツのコピー|  
|コントロール|一般的なコントロール|  
  
 この表では、大まかな WPF 機能について説明します。 詳細については、Windows SDK が WPF の各メンバーに必要なアクセス許可を文書に記載しています。 また、次の機能には、特別な考慮事項など、部分的な信頼の実行に関する詳細情報が含まれています。  
  
- [!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)] (「 [XAML の概要 (WPF)](../../desktop-wpf/fundamentals/xaml.md)」を参照してください)。  
  
- ポップアップ (<xref:System.Windows.Controls.Primitives.Popup?displayProperty=nameWithType>を参照)。  
  
- ドラッグアンドドロップ ([ドラッグアンドドロップの概要を](./advanced/drag-and-drop-overview.md)参照)。  
  
- クリップボード (「<xref:System.Windows.Clipboard?displayProperty=nameWithType>)」を参照してください。  
  
- イメージング (<xref:System.Windows.Controls.Image?displayProperty=nameWithType>を参照してください)。  
  
- シリアル化 (「<xref:System.Windows.Markup.XamlReader.Load%2A?displayProperty=nameWithType>、<xref:System.Windows.Markup.XamlWriter.Save%2A?displayProperty=nameWithType>)」を参照してください。  
  
- [ファイルを開く] ダイアログボックス (<xref:Microsoft.Win32.OpenFileDialog?displayProperty=nameWithType>を参照)。  
  
 次の表は、インターネットゾーンのアクセス許可セットの制限内で実行するのが安全ではない WPF 機能の概要を示しています。  
  
 表 2: 部分信頼では安全でない WPF 機能  
  
|機能領域|機能|  
|------------------|-------------|  
|全般|ウィンドウ (アプリケーション定義のウィンドウおよびダイアログボックス)<br /><br /> SaveFileDialog<br /><br /> ファイル システム<br /><br /> レジストリへのアクセス<br /><br /> ドラッグ アンド ドロップ<br /><br /> XAML シリアル化 (XamlWriter 経由)<br /><br /> UIAutomation クライアント<br /><br /> ソースウィンドウへのアクセス (HwndHost)<br /><br /> 完全な音声サポート<br /><br /> Windows フォームの相互運用性|  
|効果|ビットマップ効果<br /><br /> イメージのエンコード|  
|編集中|リッチテキスト形式のクリップボード<br /><br /> XAML の完全なサポート|  
  
<a name="Partial_Trust_Programming"></a>   
## <a name="partial-trust-programming"></a>部分信頼プログラミング  
 XBAP アプリケーションの場合、既定のアクセス許可セットを超えるコードの動作は、セキュリティゾーンによって異なります。 ユーザーがアプリケーションをインストールしようとすると警告が表示されることもあります。 ユーザーは、インストールを続行するか取り消すかを選択できます。 次の表は、各セキュリティ ゾーンのアプリケーション動作と、完全な信頼を受け取るアプリケーションで行う必要のある操作を示しています。  
  
|セキュリティ ゾーン|動作|完全信頼を受け取るための操作|  
|-------------------|--------------|------------------------|  
|ローカル コンピューター|完全な信頼を自動的に受け取る|対処は必要ありません。|  
|イントラネットおよび信頼済みサイト|完全な信頼のプロンプトを表示する|プロンプトにソースが表示されるように、証明書を使用して XBAP に署名します。|  
|インターネット|"信頼されていません" というメッセージが表示され、失敗する|証明書を使用して XBAP に署名します。|  
  
> [!NOTE]
> 前の表で説明した動作は、ClickOnce の信頼されたデプロイモデルに従っていない完全信頼 Xbap 用です。  
  
 一般に、許可されたアクセス許可を超える可能性のあるコードは、スタンドアロンとブラウザーの両方でホストされるアプリケーションの間で共有される共通コードである可能性があります。 CA と WPF は、このシナリオを管理するためのいくつかの手法を提供します。  
  
<a name="Detecting_Permissions_using_CAS"></a>   
### <a name="detecting-permissions-using-cas"></a>CA を使用してアクセス許可を検出する  
 場合によっては、ライブラリアセンブリ内の共有コードをスタンドアロンアプリケーションと Xbap の両方で使用できます。 このような場合、コードは、アプリケーションの付与されたアクセス許可セットよりも多くのアクセス許可を必要とする可能性がある機能を実行する可能性があります。 アプリケーションでは、Microsoft .NET Framework セキュリティを使用して特定のアクセス許可があるかどうかを検出できます。 具体的には、必要なアクセス許可のインスタンスの <xref:System.Security.CodeAccessPermission.Demand%2A> メソッドを呼び出すことによって、特定のアクセス許可があるかどうかをテストできます。 これを次の例に示します。これには、ファイルをローカルディスクに保存できるかどうかをクエリするコードが含まれています。  
  
 [!code-csharp[PartialTrustSecurityOverviewSnippets#DetectPermsCODE1](~/samples/snippets/csharp/VS_Snippets_Wpf/PartialTrustSecurityOverviewSnippets/CSharp/FileHandling.cs#detectpermscode1)]
 [!code-vb[PartialTrustSecurityOverviewSnippets#DetectPermsCODE1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PartialTrustSecurityOverviewSnippets/VisualBasic/FileHandling.vb#detectpermscode1)]  
[!code-csharp[PartialTrustSecurityOverviewSnippets#DetectPermsCODE2](~/samples/snippets/csharp/VS_Snippets_Wpf/PartialTrustSecurityOverviewSnippets/CSharp/FileHandling.cs#detectpermscode2)]
[!code-vb[PartialTrustSecurityOverviewSnippets#DetectPermsCODE2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PartialTrustSecurityOverviewSnippets/VisualBasic/FileHandling.vb#detectpermscode2)]  
  
 アプリケーションに必要なアクセス許可がない場合、<xref:System.Security.CodeAccessPermission.Demand%2A> を呼び出すと、セキュリティ例外がスローされます。 それ以外の場合は、アクセス許可が付与されています。 `IsPermissionGranted` はこの動作をカプセル化し、必要に応じて `true` または `false` を返します。  
  
<a name="Graceful_Degradation_of_Functionality"></a>   
### <a name="graceful-degradation-of-functionality"></a>機能の正常な低下  
 コードに必要な操作を実行するためのアクセス許可があるかどうかを検出するには、別のゾーンから実行できるコードを使用することが重要です。 ゾーンを検出することは、可能であれば、ユーザーに代替手段を提供する方がはるかに優れています。 たとえば、完全信頼アプリケーションでは、通常、ユーザーが必要な場所にファイルを作成できます。一方、部分信頼アプリケーションでは、分離ストレージ内のファイルのみを作成できます。 ファイルを作成するコードが、完全信頼 (スタンドアロン) アプリケーションと部分信頼 (ブラウザーホスト) アプリケーションの両方で共有されるアセンブリに存在し、両方のアプリケーションでユーザーにファイルの作成を許可する場合、共有コードは、適切な場所にファイルを作成する前に、部分的または完全な信頼で実行しています。 次のコードは、両方を示しています。  
  
 [!code-csharp[PartialTrustSecurityOverviewSnippets#DetectPermsGracefulCODE1](~/samples/snippets/csharp/VS_Snippets_Wpf/PartialTrustSecurityOverviewSnippets/CSharp/FileHandlingGraceful.cs#detectpermsgracefulcode1)]
 [!code-vb[PartialTrustSecurityOverviewSnippets#DetectPermsGracefulCODE1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PartialTrustSecurityOverviewSnippets/VisualBasic/FileHandlingGraceful.vb#detectpermsgracefulcode1)]  
[!code-csharp[PartialTrustSecurityOverviewSnippets#DetectPermsGracefulCODE2](~/samples/snippets/csharp/VS_Snippets_Wpf/PartialTrustSecurityOverviewSnippets/CSharp/FileHandlingGraceful.cs#detectpermsgracefulcode2)]
[!code-vb[PartialTrustSecurityOverviewSnippets#DetectPermsGracefulCODE2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PartialTrustSecurityOverviewSnippets/VisualBasic/FileHandlingGraceful.vb#detectpermsgracefulcode2)]  
  
 多くの場合、部分信頼の代替を見つけることができます。  
  
 イントラネットなどの管理された環境では、カスタムマネージフレームワークをクライアントベース全体にグローバルアセンブリキャッシュ (GAC) にインストールできます。 これらのライブラリは、完全信頼を必要とするコードを実行できます。また、<xref:System.Security.AllowPartiallyTrustedCallersAttribute> を使用した部分信頼のみを許可されているアプリケーションから参照できます (詳細については、「[セキュリティ](security-wpf.md)と[WPF のセキュリティ方針-プラットフォームセキュリティ](wpf-security-strategy-platform-security.md)」を参照してください)。  
  
<a name="Browser_Host_Detection"></a>   
### <a name="browser-host-detection"></a>ブラウザーホストの検出  
 アクセス許可を確認する必要がある場合は、CA を使用してアクセス許可を確認することが適切な方法です。 ただし、この手法は通常の処理の一部として例外をキャッチすることに依存しますが、一般的には推奨されず、パフォーマンスの問題が発生する可能性があります。 代わりに、XAML ブラウザーアプリケーション (XBAP) がインターネットゾーンのサンドボックス内でのみ実行される場合は、<xref:System.Windows.Interop.BrowserInteropHelper.IsBrowserHosted%2A?displayProperty=nameWithType> プロパティを使用できます。このプロパティは、XAML ブラウザーアプリケーション (Xbap) に対して true を返します。  
  
> [!NOTE]
> <xref:System.Windows.Interop.BrowserInteropHelper.IsBrowserHosted%2A> は、アプリケーションがブラウザーで実行されているかどうかを識別するだけで、アプリケーションが実行されているアクセス許可のセットは区別されません。  
  
<a name="Managing_Permissions"></a>   
## <a name="managing-permissions"></a>権限の管理  
 既定では、Xbap は部分信頼 (既定のインターネットゾーンアクセス許可セット) を使用して実行されます。 ただし、アプリケーションの要件によっては、アクセス許可のセットを既定値から変更することができます。 たとえば、Xbap がローカルイントラネットから起動した場合、次の表に示すように、拡張されたアクセス許可セットを利用できます。  
  
 表 3: LocalIntranet とインターネットのアクセス許可  
  
|権限|Attribute|イントラネット|インターネット|  
|----------------|---------------|-------------------|--------------|  
|DNS|DNS サーバーへのアクセス|はい|いいえ|  
|環境変数|Read|はい|いいえ|  
|ファイルダイアログ|[ファイル]|はい|はい|  
|ファイルダイアログ|制限なし|はい|いいえ|  
|分離ストレージ|ユーザーによるアセンブリの分離|はい|いいえ|  
|分離ストレージ|不明な分離|はい|はい|  
|分離ストレージ|無制限のユーザークォータ|はい|いいえ|  
|メディア|安全なオーディオ、ビデオ、および画像|はい|はい|  
|印刷|既定の印刷|はい|いいえ|  
|印刷|安全な印刷|はい|はい|  
|リフレクション|Pdb|はい|いいえ|  
|Security|マネージコードの実行|はい|はい|  
|Security|付与されたアクセス許可のアサート|はい|いいえ|  
|ユーザー インターフェイス|制限なし|はい|いいえ|  
|ユーザー インターフェイス|セーフトップレベルウィンドウ|はい|はい|  
|ユーザー インターフェイス|独自のクリップボード|はい|はい|  
|Web ブラウザー|HTML への安全なフレームナビゲーション|はい|はい|  
  
> [!NOTE]
> 切り取りと貼り付けは、ユーザーが開始したときに部分信頼でのみ許可されます。  
  
 アクセス許可を増やす必要がある場合は、プロジェクトの設定と ClickOnce アプリケーションマニフェストを変更する必要があります。 詳細については、「[WPF XAML ブラウザー アプリケーションの概要](./app-development/wpf-xaml-browser-applications-overview.md)」をご覧ください。 次のドキュメントも役に立つ場合があります。  
  
- [Mage.exe (マニフェスト生成および編集ツール)](../tools/mage-exe-manifest-generation-and-editing-tool.md)。  
  
- [Mageui.exe (マニフェスト生成および編集ツール、グラフィカルクライアント)](../tools/mageui-exe-manifest-generation-and-editing-tool-graphical-client.md)。  
  
- [ClickOnce アプリケーションをセキュリティで保護](/visualstudio/deployment/securing-clickonce-applications)する。  
  
 XBAP が完全な信頼を必要とする場合は、同じツールを使用して、要求されたアクセス許可を増やすことができます。 XBAP は、ローカルコンピューター、イントラネット、またはブラウザーの信頼されたサイトまたは許可されたサイトに記載されている URL から、完全な信頼を取得するだけです。 アプリケーションがイントラネットまたは信頼済みサイトからインストールされている場合、ユーザーは、昇格されたアクセス許可を通知する標準の ClickOnce プロンプトを受け取ります。 ユーザーは、インストールを続行するか取り消すかを選択できます。  
  
 または、すべてのセキュリティゾーンからの完全信頼配置に ClickOnce 信頼された配置モデルを使用することもできます。 詳細については、「[信頼されたアプリケーションの展開の概要](/visualstudio/deployment/trusted-application-deployment-overview)と[セキュリティ](security-wpf.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [セキュリティ](security-wpf.md)
- [WPF のセキュリティ方針 - プラットフォーム セキュリティ](wpf-security-strategy-platform-security.md)
- [WPF のセキュリティ方針 - セキュリティ エンジニアリング](wpf-security-strategy-security-engineering.md)
