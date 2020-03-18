---
title: アプリケーション開発
ms.date: 01/26/2018
helpviewer_keywords:
- WPF [WPF], about application development
- application development [WPF], about
ms.assetid: 2996ce5e-81e9-49ae-881b-952db3dd1b7e
ms.openlocfilehash: b0bdf49e0bb3d9bfa3fc4e7fd94aa68ee4ea0bb3
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636394"
---
# <a name="application-development"></a>アプリケーション開発
<a name="introduction"></a>Windows Presentation Foundation (WPF) は、次の種類のアプリケーションを開発するために使用できるプレゼンテーションフレームワークです。  
  
- スタンドアロンアプリケーション (クライアントコンピューターにインストールされて実行される実行可能アセンブリとして構築された従来のスタイルの Windows アプリケーション)。  
  
- XAML ブラウザーアプリケーション (Xbap) (実行可能アセンブリとして構築され、Microsoft Internet Explorer や Mozilla Firefox などの Web ブラウザーによってホストされるナビゲーションページで構成されるアプリケーション)。  
  
- カスタム コントロール ライブラリ (再利用可能なコントロールを含む被実行可能アセンブリ)。  
  
- クラス ライブラリ (再利用可能なクラスを含む非実行可能アセンブリ)。  
  
> [!NOTE]
> Windows サービスで WPF 型を使用するのは避けることを強くお勧めします。 Windows サービスでこれらの機能を使用すると、期待どおりの動作が得られない場合があります。  
  
 この一連のアプリケーションをビルドするために、WPF はサービスのホストを実装します。 このトピックでは、これらのサービスの概要を説明し、詳細情報へのリンクを示します。  

<a name="Application_Management"></a>   
## <a name="application-management"></a>アプリケーション構成の管理  
 実行可能な WPF アプリケーションでは、通常、次の機能を含む主要な機能セットが必要です。  
  
- 共通アプリケーション インフラストラクチャを作成し、管理する (エントリ ポイント メソッドと、システム メッセージおよび入力メッセージを受け取るための Windows メッセージ ループの作成を含む)。  
  
- アプリケーションの有効期間を追跡し、これと相互作用する。  
  
- コマンド ライン パラメーターを取得し、処理する。  
  
- アプリケーション スコープのプロパティと [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] リソースを共有する。  
  
- 未処理の例外を検出し、処理する。  
  
- 終了コードを返す。  
  
- スタンドアロン アプリケーションのウィンドウを管理する。  
  
- XAML ブラウザーアプリケーション (Xbap) のナビゲーションの追跡と、ナビゲーションウィンドウとフレームを使用したスタンドアロンアプリケーション。  
  
 これらの機能は <xref:System.Windows.Application> クラスで実装します。このクラスをアプリケーションに追加するには、*アプリケーション定義*を使用します。  
  
 詳細については、「[アプリケーション管理の概要](application-management-overview.md)」を参照してください。  
  
<a name="WPF_Application_Resource__Content__and_Data_Files"></a>   
## <a name="wpf-application-resource-content-and-data-files"></a>WPF アプリケーションのリソース ファイル、コンテンツ ファイル、およびデータ ファイル  
 WPF は、リソース、コンテンツ、データという3種類の非実行可能データファイルをサポートする、埋め込みリソース用の Microsoft .NET Framework のコアサポートを拡張します。 詳細については、「[WPF アプリケーションのリソース、コンテンツ ファイル、およびデータ ファイル](wpf-application-resource-content-and-data-files.md)」を参照してください。  
  
 WPF 非実行可能データファイルのサポートの重要な要素は、一意の URI を使用してそれらを識別し、読み込む機能です。 詳細については、「[WPF におけるパック URI](pack-uris-in-wpf.md)」を参照してください。  
  
<a name="Windows_and_Dialog_Boxes"></a>   
## <a name="windows-and-dialog-boxes"></a>ウィンドウとダイアログ ボックス  
 ユーザーは、windows を使用して WPF スタンドアロンアプリケーションと対話します。 ウィンドウの目的は、アプリケーションのコンテンツをホストし、コンテンツとやり取りするためにユーザーが利用できるアプリケーション機能を公開することです。 WPF では、windows は次の機能をサポートする <xref:System.Windows.Window> クラスによってカプセル化されます。  
  
- ウィンドウを作成し、表示する。  
  
- 所有者と所有ウィンドウの関係を確立する。  
  
- ウィンドウの外観を構成する (サイズ、位置、アイコン、タイトル バーのテキスト、境界など)。  
  
- ウィンドウの有効期間を追跡し、これと相互作用する。  
  
 詳細については、「[WPF ウィンドウの概要](wpf-windows-overview.md)」を参照してください。  
  
 <xref:System.Windows.Window> では、ダイアログ ボックスと呼ばれる特別な種類のウィンドウを作成できます。 モーダル ダイアログ ボックスとモードレス ダイアログ ボックスの両方の種類のダイアログ ボックスを作成できます。  
  
 利便性と、アプリケーション間での再利用性と一貫したユーザーエクスペリエンスの利点を実現するために、WPF では、<xref:Microsoft.Win32.OpenFileDialog>、<xref:Microsoft.Win32.SaveFileDialog>、<xref:System.Windows.Controls.PrintDialog>という3つのコモンウィンドウダイアログボックスが公開されています。  
  
 メッセージ ボックスは、重要な情報をテキストでユーザーに表示し、単純な [はい]、[いいえ]、[OK]、[キャンセル] の応答を求めるために使用する特別なダイアログ ボックスです。 メッセージ ボックスを作成および表示するには <xref:System.Windows.MessageBox> クラスを使用します。  
  
 詳細については、「[ダイアログ ボックスの概要](dialog-boxes-overview.md)」を参照してください。  
  
<a name="Navigation"></a>   
## <a name="navigation"></a>Navigation  
 WPF では、ページ (<xref:System.Windows.Controls.Page>) とハイパーリンク (<xref:System.Windows.Documents.Hyperlink>) を使用した Web スタイルのナビゲーションがサポートされています。 ナビゲーションは、次のようなさまざまな方法で実装できます。  
  
- Web ブラウザーでホストされるスタンドアロンのページ。  
  
- Web ブラウザーでホストされている XBAP にコンパイルされたページ。  
  
- スタンドアロン アプリケーションにコンパイルされ、ナビゲーション ウィンドウ (<xref:System.Windows.Navigation.NavigationWindow>) によってホストされるページ。  
  
- フレーム (<xref:System.Windows.Controls.Frame>) によってホストされるページ。スタンドアロンページでホストされる場合もあれば、XBAP またはスタンドアロンアプリケーションにコンパイルされたページの場合もあります。  
  
 ナビゲーションを容易にするために、WPF では次のものが実装されています。  
  
- <xref:System.Windows.Navigation.NavigationService>、<xref:System.Windows.Controls.Frame>、<xref:System.Windows.Navigation.NavigationWindow>、Xbap によってアプリケーション内でのナビゲーションをサポートするために使用されるナビゲーション要求を処理するための共有ナビゲーションエンジンです。  
  
- ナビゲーションを開始するためのナビゲーション メソッド。  
  
- ナビゲーションの有効期間を追跡し、これと相互作用するためのナビゲーション イベント。  
  
- 履歴を使用した前方や後方へのナビゲーションの記憶。履歴は、調べたり、操作したりすることもできます。  
  
 詳細については、「[ナビゲーションの概要](navigation-overview.md)」を参照してください。  
  
 WPF では、構造化ナビゲーションと呼ばれる特殊な種類のナビゲーションもサポートされています。 構造化ナビゲーションを使用すると、呼び出し元関数との一貫性が保たれた、構造化されて予測可能な形式でデータを返す 1 つ以上のページを呼び出す事ができます。 この機能は <xref:System.Windows.Navigation.PageFunction%601> クラスに依存します。このクラスの詳細については、「[構造化ナビゲーションの概要](structured-navigation-overview.md)」を参照してください。 また、<xref:System.Windows.Navigation.PageFunction%601> を使用すると、複雑なナビゲーション トポロジを簡単に作成することもできます。詳細については、「[ナビゲーション トポロジの概要](navigation-topologies-overview.md)」を参照してください。  
  
<a name="Hosting"></a>   
## <a name="hosting"></a>ホスティング  
 Xbap は、Microsoft Internet Explorer または Firefox でホストできます。 ホストのモデルによって考慮事項や制約が異なります。詳細については、「[ホスティング](hosting-wpf-applications.md)」を参照してください。  
  
<a name="Build_and_Deploy"></a>   
## <a name="build-and-deploy"></a>ビルドと配置  
 単純な WPF アプリケーションはコマンドラインコンパイラを使用してコマンドプロンプトから構築できますが、WPF は Visual Studio と統合して、開発およびビルドプロセスを簡略化する追加のサポートを提供します。 詳細については、「[WPF アプリケーションのビルド](building-a-wpf-application-wpf.md)」を参照してください。  
  
 ビルドするアプリケーションの種類によって、選択する配置オプションが異なります。 詳細については、「[WPF アプリケーションの配置](deploying-a-wpf-application-wpf.md)」を参照してください。  
  
<a name="related_topics"></a>   
## <a name="related-topics"></a>関連トピック  
  
|[タイトル]|説明|  
|-----------|-----------------|  
|[アプリケーション管理の概要](application-management-overview.md)|アプリケーションの有効期間、ウィンドウ、アプリケーション リソース、ナビゲーションの管理など、<xref:System.Windows.Application> クラスの概要について説明します。|  
|[WPF のウィンドウ](windows-in-wpf-applications.md)|<xref:System.Windows.Window> クラスおよびダイアログ ボックスの使い方など、アプリケーション内のウィンドウ管理の詳細について説明します。|  
|[ナビゲーションの概要](navigation-overview.md)|アプリケーション内のページ間でのナビゲーション管理の概要について説明します。|  
|[ホスティング](hosting-wpf-applications.md)|XAML ブラウザーアプリケーション (Xbap) の概要について説明します。|  
|[ビルドと配置](building-and-deploying-wpf-applications.md)|WPF アプリケーションをビルドして配置する方法について説明します。|  
|[Visual Studio での WPF の概要](../getting-started/introduction-to-wpf-in-vs.md)|WPF の主な機能について説明します。|  
|[チュートリアル: 初めての WPF デスクトップ アプリケーション](../getting-started/walkthrough-my-first-wpf-desktop-application.md)|ページ ナビゲーション、レイアウト、コントロール、イメージ、スタイル、バインディングを使用する WPF アプリケーションの作成方法を示すチュートリアルです。|
