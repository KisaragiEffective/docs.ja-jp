---
title: グローバリゼーションとローカリゼーションの概要
ms.date: 03/30/2017
helpviewer_keywords:
- globalization [WPF], about globalization
- localization [WPF], about localization
ms.assetid: 56e5a5c8-6c96-4d19-b8e1-a5be1dc564af
ms.openlocfilehash: ba49b3ec0f6edebff6278f4e90ae22baba9f1edf
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452670"
---
# <a name="wpf-globalization-and-localization-overview"></a>WPF のグローバリゼーションおよびローカリゼーションの概要

製品の利用可能性を1つの言語のみに制限すると、お客様の潜在顧客ベースを世界の75億人口の一部に限定できます。 アプリケーションを世界中のユーザーに提供する場合、製品のコスト効率に優れたローカライズ方法の1つは、より多くの顧客にリーチするための最良で経済的な方法の1つです。

この概要では、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]でのグローバリゼーションとローカライズについて説明します。 グローバリゼーションとは、複数の場所で実行されるアプリケーションの設計および開発です。 たとえば、グローバリゼーションでは、異なるカルチャのユーザーに対して、ローカライズされたユーザーインターフェイスと地域データをサポートしています。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、自動レイアウト、サテライトアセンブリ、ローカライズされた属性、コメントなど、グローバリゼーションのデザイン機能が用意されています。

ローカライズとは、アプリケーションがサポートする特定のカルチャに合わせて、アプリケーションリソースをローカライズされたバージョンに変換することです。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]でローカライズする場合は、<xref:System.Windows.Markup.Localizer> 名前空間の Api を使用します。 これらの Api は、 [LocBaml ツールサンプル](https://github.com/microsoft/WPF-Samples/tree/master/Tools/LocBaml)コマンドラインツールを活用しています。 LocBaml を構築して使用する方法の詳細については、「[アプリケーションをローカライズ](how-to-localize-an-application.md)する」を参照してください。

## <a name="best-practices-for-globalization-and-localization-in-wpf"></a>WPF でのグローバリゼーションとローカライズのベストプラクティス

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] に組み込まれているグローバリゼーションおよびローカリゼーションの機能を最大限に活用するには、このセクションで説明する UI デザインとローカリゼーション関連のヒントに従ってください。

### <a name="best-practices-for-wpf-ui-design"></a>WPF UI デザインのベストプラクティス

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]ベースの [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]を設計する場合は、次のベストプラクティスを実装することを検討してください。

- [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]で [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を作成します。コードに [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を作成しないようにします。 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]を使用して [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を作成する場合は、組み込みのローカライズ Api を使用して公開します。

- コンテンツのレイアウトに絶対位置と固定サイズを使用することは避けてください。代わりに、相対サイズまたは自動サイズ設定を使用します。

  - <xref:System.Windows.Window.SizeToContent%2A> を使用し、幅と高さを `Auto`に設定したままにします。

  - [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]s をレイアウトするために <xref:System.Windows.Controls.Canvas> を使用しないでください。

  - <xref:System.Windows.Controls.Grid> とそのサイズ共有機能を使用します。

- ローカライズされたテキストには多くの領域が必要になるため、余白に余分なスペースを用意します。 余分なスペースを使用すると、使用できる文字数を増やすことができます。

- クリッピングを回避するには、<xref:System.Windows.Controls.TextBlock> の <xref:System.Windows.Controls.TextBlock.TextWrapping%2A> を有効にします。

- `xml:lang` 属性を設定します。 この属性は、特定の要素とその子要素のカルチャを表します。 このプロパティの値により、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]のいくつかの機能の動作が変更されます。 たとえば、ハイフネーション、スペルチェック、数字の置換、複雑なスクリプトの整形、およびフォントフォールバックの動作を変更します。 [XAML での xml: lang の処理](../../../desktop-wpf/xaml-services/xml-language-handling.md)の設定の詳細については、「 [WPF のグローバリゼーション](globalization-for-wpf.md)」を参照してください。

- さまざまな言語で使用されるフォントをより適切に制御できるように、カスタマイズされた複合フォントを作成します。 既定では、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は Windows\Fonts ディレクトリ内の GlobalUserInterface フォントを使用します。

- テキストを右から左に記述するカルチャでローカライズできるナビゲーションアプリケーションを作成する場合は、ページが <xref:System.Windows.Navigation.NavigationWindow>から <xref:System.Windows.FlowDirection> を継承しないように、すべてのページの <xref:System.Windows.FlowDirection> を明示的に設定します。

- ブラウザーの外部でホストされているスタンドアロンナビゲーションアプリケーションを作成する場合は、最初のアプリケーションの <xref:System.Windows.Application.StartupUri%2A> をページではなく <xref:System.Windows.Navigation.NavigationWindow> に設定します (たとえば、`<Application StartupUri="NavigationWindow.xaml">`)。 このデザインでは、ウィンドウとナビゲーションバーの <xref:System.Windows.FlowDirection> を変更できます。 詳細と例については、「[グローバリゼーションのホームページのサンプル](https://github.com/microsoft/WPF-Samples/tree/master/Globalization%20and%20Localization/GlobalizationHomepage)」を参照してください。

### <a name="best-practices-for-wpf-localization"></a>WPF のローカライズのベストプラクティス

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]ベースのアプリケーションをローカライズする場合は、次のベストプラクティスを実装することを検討してください。

- ローカライズコメントを使用して、ローカライザーに追加のコンテキストを提供します。

- ローカリゼーション属性を使用して、要素の <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> プロパティを選択的に省略するのではなく、ローカリゼーションを制御します。 詳細については[、「ローカリゼーションの属性とコメント](localization-attributes-and-comments.md)」を参照してください。

- `msbuild -t:updateuid` と `-t:checkuid` を使用して、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]の <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> プロパティを追加および確認します。 <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> のプロパティを使用して、開発とローカリゼーションの間の変更を追跡します。 <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> のプロパティは、新しい開発変更をローカライズするのに役立ちます。 <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> のプロパティを [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]に手動で追加した場合、タスクは通常、単調で正確ではなくなります。

  - ローカライズを開始した後で <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> のプロパティを編集または変更しないでください。

  - 重複する <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> プロパティは使用しないでください (コピーと貼り付けコマンドを使用する場合は、この点に注意してください)。

  - AssemblyInfo の `UltimateResourceFallback` の場所を設定します。\* して、適切なフォールバック言語を指定します (`[assembly: NeutralResourcesLanguage("en-US",   UltimateResourceFallbackLocation.Satellite)]`など)。

    プロジェクトファイルの `<UICulture>` タグを省略して、メインアセンブリにソース言語を含める場合は、`UltimateResourceFallback` の場所をサテライトではなくメインアセンブリとして設定します (たとえば、`[assembly: NeutralResourcesLanguage("en-US", UltimateResourceFallbackLocation.MainAssembly)]`)。

## <a name="localize-a-wpf-application"></a>WPF アプリケーションのローカライズ

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションをローカライズする場合、いくつかのオプションがあります。 たとえば、アプリケーション内のローカライズ可能なリソースを XML ファイルにバインドしたり、ローカライズ可能なテキストを resx テーブルに格納したり、ローカライザーに [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] ファイルを使用させることができます。 このセクションでは、XAML の BAML 形式を使用するローカライズワークフローについて説明します。これには、いくつかの利点があります。

- をビルドした後でローカライズできます。

- 開発時にローカライズできるように、以前のバージョンの XAML の baml 形式のローカライズを使用して、新しいバージョンの XAML 形式に更新できます。

- XAML の BAML 形式は [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]のコンパイル済みの形式であるため、コンパイル時に元のソース要素とセマンティクスを検証することができます。

### <a name="localization-build-process"></a>ローカリゼーションビルドプロセス

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションを開発する場合、ローカリゼーションのビルドプロセスは次のようになります。

- 開発者は [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションを作成し、globalizes します。 プロジェクトファイルでは、アプリケーションをコンパイルするときに、言語に依存しないメインアセンブリが生成されるように、開発者が `<UICulture>en-US</UICulture>` を設定します。 このアセンブリには、ローカライズ可能なすべてのリソースを含むサテライトファイルがあります。 必要に応じて、ローカライズ Api がメインアセンブリからの抽出をサポートしているため、ソース言語をメインアセンブリに保持できます。

- ファイルがビルドにコンパイルされると、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] は、XAML の BAML 形式に変換されます。 カルチャに依存しない `MyDialog.exe` とカルチャに依存する (英語) `MyDialog.resources.dll` ファイルは、英語圏の顧客にリリースされます。

### <a name="localization-workflow"></a>ローカライズワークフロー

ローカライズプロセスは、unlocalized `MyDialog.resources.dll` ファイルがビルドされた後に開始されます。 元の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] の [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] の要素とプロパティは、<xref:System.Windows.Markup.Localizer>の Api を使用して、XAML の BAML 形式からキーと値のペアに抽出されます。 ローカライザーは、キーと値のペアを使用してアプリケーションをローカライズします。 ローカライズが完了したら、新しい値から新しい. resource .dll を生成することができます。

キーと値のペアのキーは、開発者によって元の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]に配置された `x:Uid` 値です。 これらの `x:Uid` 値を使用すると、ローカリゼーション時に開発者とローカライザーの間で発生した変更を API で追跡およびマージできます。 たとえば、ローカライザーがローカライズを開始した後に、開発者が [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を変更した場合、開発の変更を既に完了しているローカリゼーション作業とマージして、最小限の翻訳作業が失われるようにすることができます。

次の図は、XAML の BAML 形式に基づく一般的なローカライズワークフローを示しています。 この図は、開発者がアプリケーションを英語で書いていることを前提としています。 開発者は、WPF アプリケーションを作成して globalizes します。 プロジェクトファイルでは、開発者が `<UICulture>en-US</UICulture>` を設定して、ビルド時に、ローカライズ可能なすべてのリソースを含むサテライトと共に、言語に依存しないメインアセンブリが生成されるようにします。 または、WPF のローカライズ Api がメインアセンブリからの抽出をサポートしているため、メインアセンブリにソース言語を保持することもできます。 ビルドプロセスの後、XAML は BAML にコンパイルされます。 カルチャに依存しない MyDialog .exe は、英語のお客様に送付されます。

![ローカライズワークフローを示す図。](./media/wpf-globalization-and-localization-overview/localization-workflow.png)

![Unlocalized ワークフローを示す図。](./media/wpf-globalization-and-localization-overview/unlocalized-workflow.png)

## <a name="examples-of-wpf-localization"></a>WPF のローカライズの例

このセクションには、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] アプリケーションをビルドおよびローカライズする方法を理解するのに役立つ、ローカライズされたアプリケーションの例が含まれています。

### <a name="run-dialog-box-example"></a>[実行] ダイアログボックスの例

次の図は、 **[実行]** ダイアログボックスのサンプルの出力を示しています。

**英語：**

![英語版の [実行] ダイアログボックスを示すスクリーンショット。](./media/wpf-globalization-and-localization-overview/run-dialog-box-english.png)

**ドイツ語：**

![ドイツ語版の [実行] ダイアログボックスを示すスクリーンショット。](./media/wpf-globalization-and-localization-overview/run-dialog-box-german.png)

**[グローバル実行] ダイアログボックスのデザイン**

この例では、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] と [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]を使用して、 **[実行]** ダイアログボックスを作成します。 このダイアログボックスは、Microsoft Windows の スタート メニューから使用できる **実行** ダイアログボックスに相当します。

グローバルダイアログボックスを作成するためのいくつかの特徴を次に示します。

**自動レイアウト**

*Window1.xaml で、次のようにします。*

`<Window SizeToContent="WidthAndHeight">`

前のウィンドウプロパティは、コンテンツのサイズに応じてウィンドウのサイズを自動的に変更します。 このプロパティは、ローカライズ後にサイズが大きくなるコンテンツがウィンドウに表示されないようにします。また、ローカライズ後にコンテンツのサイズを小さくすると、不要な領域が削除されます。

`<Grid x:Uid="Grid_1">`

[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ローカライズ Api が正常に機能するためには <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> プロパティが必要です。

これらは、[!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)]の開発とローカライズの間の変更を追跡するために [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ローカリゼーション Api によって使用されます。 <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> プロパティを使用すると、新しいバージョンの [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]の古いローカライズにマージできます。 コマンドシェルで `msbuild -t:updateuid RunDialog.csproj` を実行して、<xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> プロパティを追加します。 これは、プロパティを手動で追加すると、通常は時間がかかり、正確さが低下するため、<xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> プロパティを追加するための推奨される方法です。 `msbuild -t:checkuid RunDialog.csproj`を実行して <xref:System.Windows.Markup.Localizer.BamlLocalizableResourceKey.Uid%2A> プロパティが正しく設定されていることを確認できます。

[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] は <xref:System.Windows.Controls.Grid> コントロールを使用して構成されています。これは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]で自動レイアウトを活用するための便利なコントロールです。 ダイアログボックスは、3つの行と5つの列に分割されることに注意してください。 行と列の定義のいずれかが固定サイズではありません。そのため、各セルに配置されている [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] の要素は、ローカリゼーション時のサイズの増減に合わせて調整できます。

[!code-xaml[GlobalizationRunDialog#GridColumnDef](~/samples/snippets/csharp/VS_Snippets_Wpf/GlobalizationRunDialog/CS/Window1.xaml#gridcolumndef)]

**Open:** label および <xref:System.Windows.Controls.ComboBox> が配置されている最初の2つの列は、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 合計幅の10% を使用します。

[!code-xaml[GlobalizationRunDialog#GridColumnDef2](~/samples/snippets/csharp/VS_Snippets_Wpf/GlobalizationRunDialog/CS/Window1.xaml#gridcolumndef2)]

この例では、<xref:System.Windows.Controls.Grid>の共有サイズ設定機能を使用していることに注意してください。 最後の3つの列は、同じ <xref:System.Windows.Controls.DefinitionBase.SharedSizeGroup%2A>に配置することで、これを利用します。 これにより、プロパティの名前と同じように、列が同じサイズを共有できるようになります。 "Browse..."長い文字列 "Durchsuchen..." にローカライズされているので、すべてのボタンの幅が大きくなります。小さい "OK" ボタンと、大規模な "Durchsuchen..." を指定する必要はありません。;.

**xml: lang**

`xml:lang="en-US"`

[XAML での xml: lang の処理](../../../desktop-wpf/xaml-services/xml-language-handling.md)が、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]のルート要素に配置されていることに注意してください。 このプロパティは、指定された要素とその子のカルチャを記述します。 この値は [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のいくつかの機能で使用されるため、ローカライズ時に適切に変更する必要があります。 この値によって、ハイフネーションおよびスペルチェックの単語に使用する言語辞書が変更されます。 また、数字の表示や、フォントフォールバックシステムが使用するフォントを選択する方法にも影響します。 最後に、プロパティは数値の表示方法に影響を及ぼし、複雑なスクリプトで記述されたテキストの整形方法に影響します。 既定値は "en-us" です。

**サテライトリソースアセンブリのビルド**

*.Csproj:*

`.csproj` ファイルを編集し、次のタグを無条件 `<PropertyGroup>`に追加します。

`<UICulture>en-US</UICulture>`

`UICulture` 値が追加されていることに注目してください。 これが en-us などの有効な <xref:System.Globalization.CultureInfo> 値に設定されている場合、プロジェクトをビルドすると、ローカライズ可能なすべてのリソースを含むサテライトアセンブリが生成されます。

`<Resource Include="RunIcon.JPG">`

`<Localizable>False</Localizable>`

`</Resource>`

`RunIcon.JPG` は、すべてのカルチャで同じように表示される必要があるため、ローカライズする必要はありません。 `Localizable` は `false` に設定されるため、サテライトアセンブリではなく、言語に依存しないメインアセンブリに保持されます。 コンパイルされていないすべてのリソースの既定値は、`true`に設定 `Localizable` ます。

**[実行] ダイアログのローカライズ**

**解析**

アプリケーションをビルドした後、ローカライズするための最初の手順として、ローカライズ可能なリソースをサテライトアセンブリから解析します。 このトピックでは、 [Locbaml ツールサンプル](https://github.com/microsoft/WPF-Samples/tree/master/Tools/LocBaml)にある locbaml ツールのサンプルを使用します。 LocBaml は、ローカライズプロセスに適したローカリゼーションツールの構築を開始するのに役立つサンプルツールでのみあることに注意してください。 LocBaml を使用して、次を実行して解析します。 **locbaml/Parse RunDialog. .resources/out:** "rundialog. .resources. .RESOURCES. .csv" ファイルを生成します。

**地域**

Unicode をサポートしているお気に入りの CSV エディターを使用して、このファイルを編集します。 ローカライズカテゴリが "なし" のすべてのエントリを除外します。 次のエントリが表示されます。

|リソースキー|ローカライズのカテゴリ|値|
|-|-|-|
|Button_1:.... $Content|ボタン|OK|
|Button_2:.... $Content|ボタン|キャンセル|
|Button_3:System.Windows.Controls.Button.$Content|ボタン|参照...|
|ComboBox_1:System.Windows.Controls.ComboBox.$Content|コンボ ボックス||
|TextBlock_1: system.string. $Content のようになります。|Text|プログラム、フォルダー、ドキュメント、またはインターネットリソースの名前を入力すると、Windows によってファイルが開きます。|
|TextBlock_2: system.string. $Content のようになります。|Text|[オープン]:|
|Window_1:System.Windows.Window.Title|タイトル|ラン|

アプリケーションをドイツ語にローカライズするには、次の変換が必要です。

|リソースキー|ローカライズのカテゴリ|値|
|-|-|-|
|Button_1:.... $Content|ボタン|OK|
|Button_2:.... $Content|ボタン|Abbrechen|
|Button_3:System.Windows.Controls.Button.$Content|ボタン|Durchsuchen...|
|ComboBox_1:System.Windows.Controls.ComboBox.$Content|コンボ ボックス||
|TextBlock_1: system.string. $Content のようになります。|Text|Geben Sie den namen eines programms、Ordners、Dokuments 順番 einer internetresource an。|
|TextBlock_2: system.string. $Content のようになります。|Text|Öffnen:|
|Window_1:System.Windows.Window.Title|タイトル|ラン|

**Generate**

ローカリゼーションの最後の手順では、新たにローカライズされたサテライトアセンブリを作成します。 これは、次の LocBaml コマンドを使用して実現できます。

**LocBaml/generate RunDialog. .resources. .resources:... .resources. .resources. .CSV:./cul: デデュープ**

ドイツ語のウィンドウでは、このリソース .dll がメインアセンブリの横にある de 以外のフォルダーに配置されている場合、このリソースは en-us フォルダーにあるものではなく、自動的に読み込まれます。 これをテストするためにドイツ語版の Windows がインストールされていない場合は、使用している Windows の任意のカルチャ (`en-US`など) にカルチャを設定し、元のリソース DLL を置き換えます。

**サテライトリソースの読み込み**

|MyDialog.exe|en-US\MyDialog.resources.dll|de-DE\MyDialog.resources.dll|
|------------------|------------------------------------|------------------------------------|
|コード|元の英語の BAML|ローカライズされた BAML|
|カルチャに依存しないリソース|英語のその他のリソース|ドイツ語にローカライズされたその他のリソース|

.NET は、アプリケーションの <xref:System.Threading.Thread.CurrentUICulture%2A?displayProperty=nameWithType>に基づいて、読み込むサテライトリソースアセンブリを自動的に選択します。 これは、既定では、Windows OS のカルチャになります。 ドイツ語版の Windows を使用している場合は、 *de-DE\MyDialog.resources.dll*ファイルが読み込まれます。 英語版の Windows を使用している場合は、 *en-US\MyDialog.resources.dll*ファイルが読み込まれます。 プロジェクトの*AssemblyInfo*ファイルで `NeutralResourcesLanguage` 属性を指定することによって、アプリケーションの最終的なフォールバックリソースを設定できます。 たとえば、次のように指定します。

`[assembly: NeutralResourcesLanguage("en-US", UltimateResourceFallbackLocation.Satellite)]`

その後、 *en-US\MyDialog.resources.dll*ファイルは、 *De-DE\MyDialog.resources.dll*または*DE\MyDialog.resources.dll*のいずれのファイルも使用できない場合に、ドイツ語のウィンドウと共に使用されます。

### <a name="microsoft-saudi-arabia-homepage"></a>Microsoft サウジアラビアホームページ

次の図は、英語とアラビアのホームページを示しています。 これらのグラフィックスを生成する完全なサンプルについては、「[グローバリゼーションのホームページのサンプル](https://github.com/microsoft/WPF-Samples/tree/master/Globalization%20and%20Localization/GlobalizationHomepage)」を参照してください。

**英語：**

![英語のホームページを示すスクリーンショット。](./media/wpf-globalization-and-localization-overview/english-home-page-sample.jpg)

**アラビア語：**

![アラビアのホームページを示すスクリーンショット。](./media/wpf-globalization-and-localization-overview/arabic-home-page-sample.jpg)

### <a name="designing-a-global-microsoft-home-page"></a>グローバルな Microsoft ホームページを設計する

Microsoft サウジアラビア web サイトのこのモックアップでは、RightToLeft 言語用に提供されているグローバリゼーション機能について説明しています。 ヘブライ語やアラビア語などの言語では、右から左への読み取り順序が使用されているため、多くの場合、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] のレイアウトは、英語などの左から右に記述する言語とはまったく異なるレイアウトになることがあります。 左から右に記述する言語から右から左の言語へのローカライズや、その逆の変換は、非常に困難な場合があります。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、このようなローカライズをはるかに簡単にするように設計されています。

**System.windows.flowdirection>**

*ホームページ:*

[!code-xaml[GlobalizationHomepage#Homepage](~/samples/snippets/csharp/VS_Snippets_Wpf/GlobalizationHomepage/CS/Homepage.xaml#homepage)]

<xref:System.Windows.Controls.Page>の <xref:System.Windows.FrameworkElement.FlowDirection%2A> プロパティに注目してください。 このプロパティを <xref:System.Windows.FlowDirection.RightToLeft> に変更すると、<xref:System.Windows.Controls.Page> とその子要素の <xref:System.Windows.FrameworkElement.FlowDirection%2A> が変更されるため、アラビア語のユーザーが期待するように、この [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] のレイアウトが右から左へと反転されます。 任意の要素に対して明示的な <xref:System.Windows.FrameworkElement.FlowDirection%2A> を指定することで、継承動作をオーバーライドできます。 <xref:System.Windows.FrameworkElement.FlowDirection%2A> プロパティは、<xref:System.Windows.FrameworkElement> またはドキュメント関連の要素で使用でき、<xref:System.Windows.FlowDirection.LeftToRight>の暗黙的な値を持ちます。

ルート <xref:System.Windows.FrameworkElement.FlowDirection%2A> が変更されたときに、背景のグラデーションブラシが正しく反転していることを確認します。

**System.windows.flowdirection> = "LeftToRight"**

![グラデーションフローを左から右に示すスクリーンショット。](./media/wpf-globalization-and-localization-overview/gradient-flow-left-right.png)

**System.windows.flowdirection> = "RightToLeft"**

![右から左へのグラデーションフローを示すスクリーンショット。](./media/wpf-globalization-and-localization-overview/gradient-flow-right-left.png)

**パネルとコントロールに固定ディメンションを使用しない**

ホームページを参照してください。一番上の <xref:System.Windows.Controls.DockPanel>の [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 全体に対して指定された固定幅と高さ以外には、その他の固定ディメンションはありません。 ソーステキストより長い可能性のあるローカライズされたテキストをクリップしないように、固定ディメンションを使用しないようにします。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] パネルとコントロールは、含まれているコンテンツに基づいて自動的にサイズが変更されます。 ほとんどのコントロールには、より詳細な制御を設定できる最小値と最大値もあります (たとえば、MinWidth = "20")。 <xref:System.Windows.Controls.Grid>では、'\*' を使用して相対幅と高さを設定することも (たとえば、`Width="0.25*"`)、セルサイズの共有機能を使用することもできます。

**ローカリゼーションコメント**

コンテンツがあいまいになり、翻訳が困難な場合があります。 開発者またはデザイナーは、ローカライズコメントを使用してローカライザーに追加のコンテキストとコメントを提供できます。 たとえば、次のようなローカライズは、文字 '&#124;' の使用法を明確にします。

[!code-xaml[GlobalizationHomepage#LocalizationComment](~/samples/snippets/csharp/VS_Snippets_Wpf/GlobalizationHomepage/CS/Homepage.xaml#localizationcomment)]

このコメントは TextBlock_1 のコンテンツに関連付けられ、LocBaml ツール (「[アプリケーションのローカライズ](how-to-localize-an-application.md)」を参照) の場合は、出力 .csv ファイルの TextBlock_1 行の6番目の列に表示されます。

|リソースキー|カテゴリ|できる|修正|コメント|値|
|-|-|-|-|-|-|
|TextBlock_1: system.string. $Content のようになります。|Text|TRUE|TRUE|この文字は、装飾規則として使用されます。|&#124;|

コメントは、次の構文を使用して、任意の要素のコンテンツまたはプロパティに配置できます。

[!code-xaml[GlobalizationHomepage#LocalizationCommentsProp](~/samples/snippets/csharp/VS_Snippets_Wpf/GlobalizationHomepage/CS/Homepage.xaml#localizationcommentsprop)]

**ローカリゼーション属性**

多くの場合、開発者またはローカライズマネージャーは、ローカライザーの読み取りと変更を制御する必要があります。 たとえば、ローカライザーが会社の名前や法律上の表現を翻訳したくない場合があります。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には、要素のコンテンツまたはプロパティの読みやすさ、変更可能性、カテゴリを設定できる属性が用意されています。この属性を使用して、ローカライズツールで要素のロック、非表示、または並べ替えを行うことができます。 詳細については、<xref:System.Windows.Localization.Attributes%2A> を参照してください。 このサンプルでは、LocBaml ツールはこれらの属性の値のみを出力します。 すべてのコントロールにこれらの属性の既定値がありますが、ではこれらの属性をオーバーライドできます。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] たとえば、次の例では、`TextBlock_1` の既定のローカリゼーション属性をオーバーライドし、コンテンツを読み取り可能に設定しますが、ローカライザーでは変更できないように設定します。

[!code-xaml[LocalizationComAtt#LocalizationAttributes](~/samples/snippets/csharp/VS_Snippets_Wpf/LocalizationComAtt/CSharp/Attributes.xaml#localizationattributes)]

読みやすさと変更可能性属性に加えて、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] には共通の UI カテゴリ (<xref:System.Windows.LocalizationCategory>) の列挙型が用意されています。これを使用すると、ローカライザーにより多くのコンテキストを与えることができます。 プラットフォームコントロールの既定のカテゴリ [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] でもオーバーライドできます。

[!code-xaml[LocalizationComAtt#LocalizationAttributesOverridden](~/samples/snippets/csharp/VS_Snippets_Wpf/LocalizationComAtt/CSharp/Attributes.xaml#localizationattributesoverridden)]

によって提供さ [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 既定のローカリゼーション属性は、コードを使用してオーバーライドすることもできます。これにより、カスタムコントロールに適切な既定値を正しく設定できます。 例 :

```csharp
[Localizability(Readability = Readability.Readable, Modifiability=Modifiability.Unmodifiable, LocalizationCategory.None)]
public class CorporateLogo : TextBlock
{
    // ...
}
```

[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] で設定されたインスタンスごとの属性は、カスタムコントロールのコードで設定した値よりも優先されます。 属性とコメントの詳細については、「[ローカリゼーション属性とコメント](localization-attributes-and-comments.md)」を参照してください。

**フォントフォールバックと複合フォント**

特定のコードポイントの範囲をサポートしていないフォントを指定した場合 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]、compositefont は、Windows\Fonts ディレクトリにあるグローバルユーザーインターフェイスを使用しているものに自動的にフォールバックします。 複合フォントは他のフォントと同様に動作し、要素の `FontFamily` を設定することによって明示的に使用できます (たとえば、`FontFamily="Global User Interface"`)。 独自の複合フォントを作成し、特定のコードポイントの範囲と言語に使用するフォントを指定することで、独自のフォントフォールバック設定を指定できます。

複合フォントの詳細については、「<xref:System.Windows.Media.FontFamily>」を参照してください。

**Microsoft ホームページのローカライズ**

このアプリケーションをローカライズするには、[実行] ダイアログの例と同じ手順に従います。 アラビア語用のローカライズされた .csv ファイルは、[グローバリゼーションホームページのサンプル](https://github.com/microsoft/WPF-Samples/tree/master/Globalization%20and%20Localization/GlobalizationHomepage)でご利用いただけます。
