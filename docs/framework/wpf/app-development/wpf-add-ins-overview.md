---
title: アドインの概要
ms.date: 03/30/2017
helpviewer_keywords:
- add-ins and XAML browser applications [WPF]
- add-ins overview [WPF]
- add-ins [WPF], performance
- add-ins [WPF], benefits
- .NET Framework add-in model [WPF]
- add-ins [WPF], user interface
- add-ins and the user interface [WPF]
- add-ins [WPF], architecture
- add-ins [WPF], limitations
ms.assetid: 00b4c776-29a8-4dba-b603-280a0cdc2ade
ms.openlocfilehash: 93904e308932ea41c736ca849ce0efb200502a7e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76738944"
---
# <a name="wpf-add-ins-overview"></a>WPF アドインの概要

<a name="Introduction"></a>.NET Framework には、アドインの機能拡張をサポートするアプリケーションを作成するために開発者が使用できるアドインモデルが含まれています。 このアドイン モデルを使用することで、アプリケーション機能に統合され、アプリケーション機能を拡張するアドインを作成できます。 場合によっては、アプリケーションがアドインによって提供されるユーザーインターフェイスを表示する必要もあります。このトピックでは、WPF が .NET Framework アドインモデルを補強して、これらのシナリオ、背後にあるアーキテクチャ、利点、および制限事項を有効にする方法について説明します。

<a name="Requirements"></a>

## <a name="prerequisites"></a>前提条件

.NET Framework アドインモデルに関する知識が必要です。 詳細については、「[アドインおよび拡張機能](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb384200(v%3dvs.100))」を参照してください。

<a name="AddInsOverview"></a>

## <a name="add-ins-overview"></a>アドインの概要

新しい機能を追加するためにアプリケーションを再コンパイルして再配置する手間を省くことを目的として、アプリケーションは機能拡張メカニズムを実装し、開発者 (ファーストパーティ、サードパーティのいずれも) が元のアプリケーションに統合される別のアプリケーションを作成できるようにしています。 こうした拡張機能をサポートする方法としては、アドイン ("アドオン"、"プラグイン" などとも呼ばれる) の使用が最も一般的です。 アドインによる機能拡張を公開する実際のアプリケーションとして、次のようなものが挙げられます。

- Internet Explorer のアドオン。

- Windows Media Player のプラグイン。

- Visual Studio のアドイン。

たとえば、Windows Media Player のアドイン モデルにおいて、サードパーティの開発者は、Windows Media Player の機能をさまざまな形で拡張する "プラグイン" を実装できます。たとえば、Windows Media Player でネイティブにサポートされていないメディア形式 (DVD、MP3 など) のデコーダーおよびエンコーダー、オーディオ効果、スキンなどを作成できます。 各アドイン モデルは、アプリケーション固有の機能を公開するためにビルドされますが、どのアドイン モデルにも共通するエンティティや動作がいくつかあります。

典型的な機能拡張ソリューションの主な 3 つのエンティティは、*コントラクト*、*アドイン*、および*ホスト アプリケーション*です。 コントラクトは、次の 2 つの方法で、アドインとホスト アプリケーションとの統合方法を定義します。

- アドインは、ホスト アプリケーションによって実装される機能と統合されます。

- ホスト アプリケーションで、アドインを統合するための機能を公開します。

アドインを使用するには、ホスト アプリケーションが実行時にアドインを検索して読み込む必要があります。 したがって、アドインをサポートするアプリケーションには次の機能が必要です。

- **探索**: ホスト アプリケーションによってサポートされるコントラクトに準拠しているアドインを検索します。

- **アクティブ化**: アドインを読み込んで実行し、アドインとの通信を確立します。

- **分離**: アプリケーション ドメインまたはアプリケーション プロセスを使用することで他から切り離すための境界を確立し、アドインの使用に伴うセキュリティおよび実行に関する潜在的な問題からアプリケーションを保護します。

- **通信**: アドインとホスト アプリケーションが分離境界を越えて通信し、メソッドを呼び出したりデータを渡したりできるようにします。

- **有効期間管理**: アプリケーション ドメインとアプリケーション プロセスの読み込みおよびアンロードを、クリーンで予測可能な方法で行います (「[アプリケーション ドメイン](../../app-domains/application-domains.md)」を参照)。

- **バージョン管理**: ホスト アプリケーションとアドインのどちらかに、新バージョンが作成された後も引き続き通信できるようにします。

このことからわかるように、堅牢なアドイン モデルの開発は簡単なことではありません。 このため、.NET Framework は、アドインモデルを構築するためのインフラストラクチャを提供します。

> [!NOTE]
> アドインの詳細については、「[アドインおよび拡張機能](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb384200(v%3dvs.100))」を参照してください。

<a name="NETFrameworkAddInModelOverview"></a>

## <a name="net-framework-add-in-model-overview"></a>.NET Framework アドイン モデルの概要

<xref:System.AddIn> 名前空間にある .NET Framework アドインモデルには、アドインの拡張機能の開発を簡略化するように設計された型のセットが含まれています。 .NET Framework アドインモデルの基本単位は*コントラクト*です。これは、ホストアプリケーションとアドインが相互に通信する方法を定義します。 コントラクトのホスト アプリケーション固有の*ビュー*を使用して、コントラクトはホスト アプリケーションに公開されます。 同様に、コントラクトのアドイン固有の*ビュー*がアドインに公開されます。 *アダプター*は、ホスト アプリケーションとアドインが、コントラクトの対応するビュー間で通信するために使用されます。 コントラクト、ビュー、およびアダプターはセグメントと見なされ、関連セグメントのセットが*パイプライン*を構成します。 パイプラインは、.NET Framework のアドインモデルが、検出、アクティベーション、セキュリティの分離、実行の分離 (アプリケーションドメインとプロセスの両方を使用)、通信、有効期間管理、バージョン管理をサポートする基盤です。

このサポート全体を使用して、開発者はホスト アプリケーションの機能を統合するアドインをビルドできます。 ただし、一部のシナリオでは、アドインによって提供されるユーザーインターフェイスをホストアプリケーションで表示する必要があります.NET Framework の各プレゼンテーションテクノロジにはユーザーインターフェイスを実装するための独自のモデルがあるため、.NET Framework アドインモデルは特定のプレゼンテーションテクノロジをサポートしていません。 代わりに、WPF は、アドインの UI サポートを使用して .NET Framework アドインモデルを拡張します。

<a name="WPFAddInModel"></a>

## <a name="wpf-add-ins"></a>WPF アドイン

WPF を .NET Framework アドインモデルと組み合わせて使用すると、ホストアプリケーションがアドインからユーザーインターフェイスを表示する必要があるさまざまなシナリオに対処できます。特に、これらのシナリオは、次の2つのプログラミングモデルを使用して WPF によって解決されます。

1. **アドインが UI を返す**。 アドインは、コントラクトで定義されているように、メソッド呼び出しを使用してホストアプリケーションに UI を返します。 このシナリオは、次の場合に使用されます。

    - アドインによって返される UI の外観は、動的に生成されたレポートなど、実行時にのみ存在するデータまたは条件に依存します。

    - アドインによって提供されるサービスの UI は、アドインを使用できるホストアプリケーションの UI とは異なります。

    - このアドインは、主にホストアプリケーションのサービスを実行し、UI を使用してホストアプリケーションにステータスを報告します。

2. **アドインが UI である**。 アドインは、コントラクトによって定義される UI です。 このシナリオは、次の場合に使用されます。

    - アドインは表示以外のサービス (広告など) を提供しない。

    - アドインによって提供されるサービスの UI は、そのアドインを使用できるすべてのホストアプリケーション (電卓やカラーピッカーなど) に共通です。

これらのシナリオでは、ホストアプリケーションドメインとアドインアプリケーションドメイン間で UI オブジェクトを渡すことができます。 .NET Framework アドインモデルは、アプリケーションドメイン間の通信にリモート処理を使用するため、これらの間で渡されるオブジェクトはリモート処理可能である必要があります。

リモート処理可能なオブジェクトとは、次の 1 つ以上に該当するクラスのインスタンスです。

- <xref:System.MarshalByRefObject> クラスから派生します。

- <xref:System.Runtime.Serialization.ISerializable> インターフェイスを実装します。

- には、<xref:System.SerializableAttribute> 属性が適用されています。

> [!NOTE]
> リモート処理可能な .NET Framework オブジェクトの作成の詳細については、「[オブジェクトのリモート](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/wcf3swha(v=vs.100))処理」を参照してください。

WPF の UI 型はリモート処理できません。 この問題を解決するために、WPF は .NET Framework アドインモデルを拡張して、アドインによって作成された WPF UI をホストアプリケーションから表示できるようにします。 このサポートは、WPF によって、<xref:System.AddIn.Contract.INativeHandleContract> インターフェイスと、<xref:System.AddIn.Pipeline.FrameworkElementAdapters> クラスによって実装される2つの静的メソッドである <xref:System.AddIn.Pipeline.FrameworkElementAdapters.ContractToViewAdapter%2A> と <xref:System.AddIn.Pipeline.FrameworkElementAdapters.ViewToContractAdapter%2A>の2つの型によって提供されます。 大まかに、これらの型とメソッドは次のように使用されます。

1. WPF では、アドインによって提供されるユーザーインターフェイスが、図形、コントロール、ユーザーコントロール、レイアウトパネル、ページなど、<xref:System.Windows.FrameworkElement>から直接または間接的に派生するクラスである必要があります。

2. アドインとホストアプリケーションの間で UI が渡されることがコントラクトで宣言されている場合は、<xref:System.AddIn.Contract.INativeHandleContract> として宣言する必要があります (<xref:System.Windows.FrameworkElement>ではありません)。<xref:System.AddIn.Contract.INativeHandleContract> は、分離境界を越えて渡すことができるアドイン UI のリモート処理可能な表現です。

3. アドインのアプリケーションドメインから渡される前に、<xref:System.AddIn.Pipeline.FrameworkElementAdapters.ViewToContractAdapter%2A>を呼び出すことによって、<xref:System.Windows.FrameworkElement> が <xref:System.AddIn.Contract.INativeHandleContract> としてパッケージ化されます。

4. ホストアプリケーションのアプリケーションドメインに渡された後、<xref:System.AddIn.Pipeline.FrameworkElementAdapters.ContractToViewAdapter%2A>を呼び出すことによって、<xref:System.AddIn.Contract.INativeHandleContract> を <xref:System.Windows.FrameworkElement> として再パッケージ化する必要があります。

<xref:System.AddIn.Contract.INativeHandleContract>、<xref:System.AddIn.Pipeline.FrameworkElementAdapters.ContractToViewAdapter%2A>、<xref:System.AddIn.Pipeline.FrameworkElementAdapters.ViewToContractAdapter%2A> の使用方法は、特定のシナリオによって異なります。 以降のセクションでは、各プログラミング モデルについて詳しく説明していきます。

<a name="ReturnUIFromAddInContract"></a>

## <a name="add-in-returns-a-user-interface"></a>ユーザー インターフェイスを返すアドイン

アドインがホストアプリケーションに UI を返すには、次のものが必要です。

1. .NET Framework[アドインおよび拡張機能](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb384200(v%3dvs.100))のドキュメントで説明されているように、ホストアプリケーション、アドイン、およびパイプラインを作成する必要があります。

2. コントラクトは <xref:System.AddIn.Contract.IContract> を実装する必要があります。また、UI を返すには、コントラクトが <xref:System.AddIn.Contract.INativeHandleContract>型の戻り値を持つメソッドを宣言する必要があります。

3. アドインとホストアプリケーションの間で渡される UI は、<xref:System.Windows.FrameworkElement>から直接または間接的に派生する必要があります。

4. アドインによって返される UI は、分離境界を越える前に、<xref:System.Windows.FrameworkElement> から <xref:System.AddIn.Contract.INativeHandleContract> に変換する必要があります。

5. 返される UI は、分離境界を越えた後に、<xref:System.AddIn.Contract.INativeHandleContract> から <xref:System.Windows.FrameworkElement> に変換される必要があります。

6. ホストアプリケーションには、返された <xref:System.Windows.FrameworkElement>が表示されます。

UI を返すアドインの実装方法を示す例については、「UI を[返すアドインを作成](how-to-create-an-add-in-that-returns-a-ui.md)する」を参照してください。

<a name="AddInIsAUI"></a>

## <a name="add-in-is-a-user-interface"></a>ユーザー インターフェイスであるアドイン

アドインが UI の場合は、次のものが必要です。

1. .NET Framework[アドインおよび拡張機能](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb384200(v%3dvs.100))のドキュメントで説明されているように、ホストアプリケーション、アドイン、およびパイプラインを作成する必要があります。

2. アドインのコントラクトインターフェイスは <xref:System.AddIn.Contract.INativeHandleContract>を実装する必要があります。

3. ホストアプリケーションに渡されるアドインは、<xref:System.Windows.FrameworkElement>から直接または間接的に派生する必要があります。

4. 分離境界を越える前に、アドインを <xref:System.Windows.FrameworkElement> から <xref:System.AddIn.Contract.INativeHandleContract> に変換する必要があります。

5. このアドインは、分離境界を越えた後に、<xref:System.AddIn.Contract.INativeHandleContract> から <xref:System.Windows.FrameworkElement> に変換する必要があります。

6. ホストアプリケーションには、返された <xref:System.Windows.FrameworkElement>が表示されます。

UI であるアドインの実装方法を示す例については、「UI である[アドインを作成](how-to-create-an-add-in-that-is-a-ui.md)する」を参照してください。

<a name="ReturningMultipleUIsFromAnAddIn"></a>

## <a name="returning-multiple-uis-from-an-add-in"></a>複数の UI を返すアドイン

多くの場合、アドインには、ホストアプリケーションが表示する複数のユーザーインターフェイスが用意されています。 たとえば、ui としてもホストアプリケーションに状態情報を提供する UI であるアドインを考えてみます。 このようなアドインは、[ユーザー インターフェイスを返すアドイン](#ReturnUIFromAddInContract)のモデルと[ユーザー インターフェイスであるアドイン](#AddInIsAUI)のモデルの両方の手法を組み合わせることで実装できます。

<a name="AddInsAndXBAPs"></a>

## <a name="add-ins-and-xaml-browser-applications"></a>アドインと XAML ブラウザー アプリケーション

ここまでの例では、ホスト アプリケーションはスタンドアロン アプリケーションとしてインストールされています。 ただし、XAML ブラウザーアプリケーション (Xbap) は、アドインをホストすることもできます。ただし、次のような追加のビルドと実装の要件があります。

- Xbap アプリケーションマニフェストは、クライアントコンピューターの ClickOnce アプリケーションキャッシュ (XBAP と同じフォルダー) に、パイプライン (フォルダーとアセンブリ) とアドインアセンブリをダウンロードするように特別に構成する必要があります。

- アドインの検出と読み込みを行う XBAP コードでは、この XBAP の ClickOnce アプリケーションキャッシュをパイプラインとアドインの場所として使用する必要があります。

- アドインが元のサイトに配置されている緩いファイルを参照する場合、XBAP は特別なセキュリティコンテキストにアドインを読み込む必要があります。Xbap でホストされている場合、アドインは、ホストアプリケーションの起点サイトにある圧縮されていないファイルのみを参照できます。

これらのタスクについて、次のサブセクションで詳しく説明します。

### <a name="configuring-the-pipeline-and-add-in-for-clickonce-deployment"></a>ClickOnce 配置のためのパイプラインとアドインの構成

Xbap は、ClickOnce 配置キャッシュ内の安全なフォルダーにダウンロードして実行します。 XBAP がアドインをホストするためには、パイプラインとアドインのアセンブリも安全なフォルダーにダウンロードする必要があります。 このためには、パイプラインとアドインのどちらのアセンブリもダウンロード対象に含まれるよう、アプリケーション マニフェストを構成する必要があります。 これは Visual Studio で最も簡単に行うことができますが、Visual Studio がパイプラインアセンブリを検出するためには、パイプラインとアドインのアセンブリをホスト XBAP プロジェクトのルートフォルダーに配置する必要があります。

そのため、最初の手順として、各パイプラインアセンブリプロジェクトとアドインアセンブリプロジェクトのビルド出力を設定して、パイプラインとアドインアセンブリを XBAP プロジェクトのルートにビルドします。 次の表は、ホスト XBAP プロジェクトと同じソリューションとルートフォルダーにあるパイプラインアセンブリプロジェクトとアドインアセンブリプロジェクトのビルド出力パスを示しています。

表 1: XBAP でホストされるパイプライン アセンブリのビルド出力パス

|パイプライン アセンブリ プロジェクト|ビルド出力パス|
|-------------------------------|-----------------------|
|コントラクト|`..\HostXBAP\Contracts\`|
|アドイン ビュー|`..\HostXBAP\AddInViews\`|
|アドイン側のアダプター|`..\HostXBAP\AddInSideAdapters\`|
|ホスト側のアダプター|`..\HostXBAP\HostSideAdapters\`|
|アドイン|`..\HostXBAP\AddIns\WPFAddIn1`|

次の手順では、次の手順に従って、パイプラインアセンブリとアドインアセンブリを Visual Studio で Xbap コンテンツファイルとして指定します。

1. ソリューション エクスプローラーで各パイプライン フォルダーを右クリックし、 **[プロジェクトに含める]** を選択して、パイプラインとアドインのアセンブリをプロジェクトに含めます。

2. **[プロパティ]** ウィンドウで、パイプライン アセンブリとアドイン アセンブリそれぞれについて、 **[ビルド アクション]** を **[コンテンツ]** に設定します。

最後に、パイプラインとアドインのどちらのアセンブリ ファイルもダウンロード対象に含まれるよう、アプリケーション マニフェストを構成します。 ファイルは、XBAP アプリケーションが占有する ClickOnce キャッシュ内のフォルダーのルートにあるフォルダーに配置する必要があります。 この構成は、Visual Studio で次の手順を実行することで実現できます。

1. XBAP プロジェクトを右クリックし、 **[プロパティ]** をクリックします。次に、 **[発行]** をクリックし、 **[アプリケーションファイル]** ボタンをクリックします。

2. **[アプリケーション ファイル]** ダイアログ ボックスで、各パイプラインとアドインの DLL の **[発行の状況]** を **[含める (自動)]** に設定し、各パイプラインとアドインの DLL の **[ダウンロード グループ]** を **[(必須)]** に設定します。

### <a name="using-the-pipeline-and-add-in-from-the-application-base"></a>アプリケーション ベースからのパイプラインとアドインの使用

パイプラインとアドインが ClickOnce 配置用に構成されている場合は、XBAP と同じ ClickOnce キャッシュフォルダーにダウンロードされます。 このパイプラインとアドインを XBAP から使用するには、XBAP コードでアプリケーションベースから取得する必要があります。 パイプラインとアドインを使用するための .NET Framework アドインモデルのさまざまな型およびメンバーは、このシナリオに対して特別なサポートを提供します。 まず、パスは <xref:System.AddIn.Hosting.PipelineStoreLocation.ApplicationBase> 列挙値によって識別されます。 この値を適切なアドイン メンバーのオーバーロードと併用することで、次のようなパイプラインを使用できます。

- <xref:System.AddIn.Hosting.AddInStore.FindAddIns%28System.Type%2CSystem.AddIn.Hosting.PipelineStoreLocation%29?displayProperty=nameWithType>

- <xref:System.AddIn.Hosting.AddInStore.FindAddIns%28System.Type%2CSystem.AddIn.Hosting.PipelineStoreLocation%2CSystem.String%5B%5D%29?displayProperty=nameWithType>

- <xref:System.AddIn.Hosting.AddInStore.Rebuild%28System.AddIn.Hosting.PipelineStoreLocation%29?displayProperty=nameWithType>

- <xref:System.AddIn.Hosting.AddInStore.Update%28System.AddIn.Hosting.PipelineStoreLocation%29?displayProperty=nameWithType>

### <a name="accessing-the-hosts-site-of-origin"></a>ホストの起点サイトへのアクセス

アドインが起点サイトのファイルを参照できるように、アドインはホスト アプリケーションと等価なセキュリティ分離を使用して読み込む必要があります。 このセキュリティレベルは <xref:System.AddIn.Hosting.AddInSecurityLevel.Host?displayProperty=nameWithType> 列挙値によって識別され、アドインがアクティブ化されるときに <xref:System.AddIn.Hosting.AddInToken.Activate%2A> メソッドに渡されます。

<a name="WPFAddInModelArchitecture"></a>

## <a name="wpf-add-in-architecture"></a>WPF アドイン アーキテクチャ

ここまで説明したように、WPF では、<xref:System.AddIn.Contract.INativeHandleContract>、<xref:System.AddIn.Pipeline.FrameworkElementAdapters.ViewToContractAdapter%2A> および <xref:System.AddIn.Pipeline.FrameworkElementAdapters.ContractToViewAdapter%2A>を使用して、<xref:System.Windows.FrameworkElement>から直接または間接的に派生したユーザーインターフェイスを実装する .NET Framework アドインを使用できます。 結果として、ホストアプリケーションには、ホストアプリケーションの UI から表示される <xref:System.Windows.FrameworkElement> が返されます。

単純な UI アドインシナリオでは、これは開発者が必要とするほど詳細です。 より複雑なシナリオ、特に、レイアウト、リソース、データバインディングなどの追加の WPF サービスを使用しようとするシナリオでは、その利点を理解するために、WPF が UI サポートを使用して .NET Framework アドインモデルを拡張する方法についての詳細な知識が必要です。および制限事項があります。

基本的に、WPF は、アドインからホストアプリケーションに UI を渡しません。wpf は、WPF の相互運用性を使用して、UI の Win32 ウィンドウハンドルを渡します。 そのため、アドインからの UI がホストアプリケーションに渡されると、次のようになります。

- アドイン側では、WPF は、ホストアプリケーションによって表示される UI のウィンドウハンドルを取得します。 ウィンドウハンドルは、<xref:System.Windows.Interop.HwndSource> から派生し <xref:System.AddIn.Contract.INativeHandleContract>を実装する内部 WPF クラスによってカプセル化されます。 このクラスのインスタンスは <xref:System.AddIn.Pipeline.FrameworkElementAdapters.ViewToContractAdapter%2A> によって返され、アドインのアプリケーションドメインからホストアプリケーションのアプリケーションドメインにマーシャリングされます。

- ホストアプリケーション側では、WPF は、<xref:System.Windows.Interop.HwndHost> から派生し <xref:System.AddIn.Contract.INativeHandleContract>を使用する内部 WPF クラスとして <xref:System.Windows.Interop.HwndSource> を再パッケージ化します。 このクラスのインスタンスは、ホストアプリケーションに <xref:System.AddIn.Pipeline.FrameworkElementAdapters.ContractToViewAdapter%2A> によって返されます。

ウィンドウハンドルによって識別されるユーザーインターフェイスを WPF ユーザーインターフェイスから表示する <xref:System.Windows.Interop.HwndHost> が存在します。 詳細については、「[WPF と Win32 の相互運用性](../advanced/wpf-and-win32-interoperation.md)」を参照してください。

要約すると、<xref:System.AddIn.Contract.INativeHandleContract>、<xref:System.AddIn.Pipeline.FrameworkElementAdapters.ViewToContractAdapter%2A>、および <xref:System.AddIn.Pipeline.FrameworkElementAdapters.ContractToViewAdapter%2A> が存在するので、WPF UI のウィンドウハンドルをホストアプリケーションに渡すことができます。このアプリケーションは、<xref:System.Windows.Interop.HwndHost> によってカプセル化され、ホストアプリケーションの UI に表示されます。

> [!NOTE]
> ホストアプリケーションは <xref:System.Windows.Interop.HwndHost>を取得するため、<xref:System.AddIn.Pipeline.FrameworkElementAdapters.ContractToViewAdapter%2A> によって返されたオブジェクトを、アドインによって実装されている型 (たとえば、<xref:System.Windows.Controls.UserControl>) に変換することはできません。

<xref:System.Windows.Interop.HwndHost> には、ホストアプリケーションがどのように使用できるかに影響するいくつかの制限があります。 ただし、WPF によって <xref:System.Windows.Interop.HwndHost> が拡張され、アドインシナリオ用にいくつかの機能を使用できます。 その利点と制約について、以下に説明します。

<a name="WPFAddInModelBenefits"></a>

## <a name="wpf-add-in-benefits"></a>WPF アドインの利点

WPF アドインのユーザーインターフェイスは <xref:System.Windows.Interop.HwndHost>から派生した内部クラスを使用してホストアプリケーションから表示されるため、これらのユーザーインターフェイスは、レイアウト、レンダリング、データバインディング、スタイル、テンプレート、リソースなどの WPF UI サービスに関して、<xref:System.Windows.Interop.HwndHost> の機能によって制限されます。 ただし、WPF は、次のような追加機能を使用して内部 <xref:System.Windows.Interop.HwndHost> サブクラスを補強します。

- ホストアプリケーションの UI とアドインの UI の間をタブ移動します。 "アドインは UI です" のプログラミングモデルでは、アドインが完全に信頼されているか、部分的に信頼されているかにかかわらず、アドイン側アダプターが <xref:System.AddIn.Pipeline.ContractBase.QueryContract%2A> をオーバーライドして、tab キーを有効にする必要があることに注意してください。

- ホストアプリケーションのユーザーインターフェイスから表示されるアドインユーザーインターフェイスのアクセシビリティ要件に従います。

- 複数のアプリケーションドメインのシナリオで、WPF アプリケーションを安全に実行できるようにします。

- アドインがセキュリティ分離 (部分信頼セキュリティサンドボックス) を使用して実行されている場合に、アドイン UI ウィンドウハンドルへの不正アクセスを防止します。 <xref:System.AddIn.Pipeline.FrameworkElementAdapters.ViewToContractAdapter%2A> を呼び出すと、次のセキュリティが保証します。

  - "アドインが UI を返す" プログラミングモデルでは、分離境界を越えてアドイン UI のウィンドウハンドルを渡す唯一の方法は、<xref:System.AddIn.Pipeline.FrameworkElementAdapters.ViewToContractAdapter%2A>を呼び出すことです。

  - "アドインは UI である" プログラミングモデルでは、アドイン側アダプターの <xref:System.AddIn.Pipeline.ContractBase.QueryContract%2A> をオーバーライドし、前の例に示すように <xref:System.AddIn.Pipeline.FrameworkElementAdapters.ViewToContractAdapter%2A> を呼び出す必要があります。これは、ホスト側アダプターからのアドイン側アダプターの `QueryContract` 実装を呼び出すのと同じです。

- アプリケーション ドメインの実行を何重にも保護します。 アプリケーション ドメインの制約に起因して、アドイン アプリケーション ドメインでスローされた未処理の例外は、分離境界が存在していても、アプリケーション全体のクラッシュにつながります。 ただし、WPF と .NET Framework アドインモデルでは、この問題を回避し、アプリケーションの安定性を向上させる簡単な方法が提供されます。 ホストアプリケーションが WPF アプリケーションである場合、UI を表示する WPF アドインによって、アプリケーションドメインが実行されているスレッドの <xref:System.Windows.Threading.Dispatcher> が作成されます。 WPF アドインの <xref:System.Windows.Threading.Dispatcher>の <xref:System.Windows.Threading.Dispatcher.UnhandledException> イベントを処理することによって、アプリケーションドメインで発生するすべてのハンドルされない例外を検出できます。 <xref:System.Windows.Threading.Dispatcher.CurrentDispatcher%2A> プロパティから <xref:System.Windows.Threading.Dispatcher> を取得できます。

<a name="WPFAddInModelLimitations"></a>

## <a name="wpf-add-in-limitations"></a>WPF アドインの制約

WPF が <xref:System.Windows.Interop.HwndSource>、<xref:System.Windows.Interop.HwndHost>、ウィンドウハンドルによって提供される既定の動作に追加されるだけではありませんが、ホストアプリケーションから表示されるアドインユーザーインターフェイスにも制限があります。

- ホストアプリケーションから表示されるアドインユーザーインターフェイスは、ホストアプリケーションのクリッピング動作を考慮しません。

- 相互運用性シナリオの*空域*の概念もアドインに適用されます (「[技術領域の概要](../advanced/technology-regions-overview.md)」を参照)。

- リソースの継承、データバインディング、コマンドのコマンドなど、ホストアプリケーションの UI サービスは、アドインのユーザーインターフェイスで自動的に使用できるわけではありません。 これらのサービスをアドインに提供するには、パイプラインを更新する必要があります。

- アドインの UI は、回転、拡大縮小、傾斜、または変換による影響を受けません (「[変換の概要](../graphics-multimedia/transforms-overview.md)」を参照してください)。

- <xref:System.Drawing> 名前空間からの描画操作によってレンダリングされるアドインユーザーインターフェイス内のコンテンツには、アルファブレンドを含めることができます。 ただし、アドイン UI と、それを含むホストアプリケーション UI は、どちらも100% 不透明である必要があります。言い換えると、両方の `Opacity` プロパティが1に設定されている必要があります。

- アドイン UI を含むホストアプリケーションのウィンドウの [<xref:System.Windows.Window.AllowsTransparency%2A>] プロパティが [`true`] に設定されている場合、アドインは非表示になります。 これは、アドイン UI が100% 不透明である (つまり、`Opacity` プロパティの値が 1) 場合でも同様です。

- アドイン UI は、同じトップレベルウィンドウ内の他の WPF 要素の上に表示される必要があります。

- <xref:System.Windows.Media.VisualBrush>を使用して、アドインの UI の一部をレンダリングすることはできません。 代わりに、アドインは、生成された UI のスナップショットを取得して、コントラクトによって定義されたメソッドを使用してホストアプリケーションに渡すことができるビットマップを作成することができます。

- アドイン UI の <xref:System.Windows.Controls.MediaElement> からメディアファイルを再生することはできません。

- アドイン UI 用に生成されたマウスイベントは、ホストアプリケーションでは受信も発生もされず、ホストアプリケーション UI の `IsMouseOver` プロパティの値は `false`になります。

- アドイン UI のコントロール間でフォーカスが移動すると、`GotFocus` イベントと `LostFocus` イベントは、ホストアプリケーションによって受信も発生もされません。

- アドイン UI を含むホストアプリケーションの部分は、印刷時に白で表示されます。

- アドイン UI によって作成されたすべてのディスパッチャー (<xref:System.Windows.Threading.Dispatcher>参照) は、ホストアプリケーションが実行を継続している場合に、所有者アドインがアンロードされる前に手動でシャットダウンする必要があります。 コントラクトは、アドインがアンロードされる前に、ホストアプリケーションがアドインを通知できるようにするメソッドを実装できます。これにより、アドインの UI はそのディスパッチャーをシャットダウンできます。

- アドイン UI が <xref:System.Windows.Controls.InkCanvas> であるか、または <xref:System.Windows.Controls.InkCanvas>が含まれている場合、アドインをアンロードすることはできません。

<a name="PerformanceOptimization"></a>

## <a name="performance-optimization"></a>パフォーマンスの最適化

既定では、複数のアプリケーションドメインを使用すると、各アプリケーションに必要なさまざまな .NET Framework アセンブリが、そのアプリケーションのドメインに読み込まれます。 その結果、新しいアプリケーション ドメインを作成してその中でアプリケーションを開始するために必要な時間がパフォーマンスに影響します。 ただし、アプリケーションドメインが既に読み込まれている場合は、アプリケーションドメイン間でアセンブリを共有するようにアプリケーションに指示することによって、開始時間を短縮する方法を .NET Framework ます。 これを行うには、<xref:System.LoaderOptimizationAttribute> 属性を使用します。これは、エントリポイントメソッド (`Main`) に適用する必要があります。 この場合、アプリケーション定義を実装するコードのみを使用する必要があります (「[アプリケーション管理の概要](application-management-overview.md)」を参照)。

## <a name="see-also"></a>参照

- <xref:System.LoaderOptimizationAttribute>
- [アドインおよび拡張機能](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb384200(v%3dvs.100))
- [アプリケーション ドメイン](../../app-domains/application-domains.md)
- [.NET Framework リモート処理の概要](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/kwdt6w2k(v=vs.100))
- [オブジェクトをリモート処理可能にする](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/wcf3swha(v=vs.100))
- [データ バインドに関する「方法」トピック](how-to-topics.md)
