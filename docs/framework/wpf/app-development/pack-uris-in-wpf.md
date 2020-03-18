---
title: パックの Uri
ms.date: 03/30/2017
helpviewer_keywords:
- pack URI scheme [WPF]
- loading embedded resources [WPF]
- URIs (Uniform Resource Identifiers)
- Uniform Resource Identifiers (URIs)
- loading non-resource files
- application management [WPF]
ms.assetid: 43adb517-21a7-4df3-98e8-09e9cdf764c4
ms.openlocfilehash: a98c97a4aa95fb956a2ca6d417e009a281a938b6
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77124482"
---
# <a name="pack-uris-in-wpf"></a>WPF におけるパッケージの URI

Windows Presentation Foundation (WPF) では、次のようなさまざまな方法でファイルを識別し、読み込むために uniform resource identifier (Uri) が使用されます。

- アプリケーションを初めて起動するときに表示する [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] を指定します。

- イメージの読み込み。

- ページへの移動。

- 実行可能でないデータ ファイルの読み込み。

さらに、Uri を使用して、次のようなさまざまな場所からファイルを識別し、読み込むことができます。

- 現在のアセンブリ。

- 参照アセンブリ。

- アセンブリに対して相対的な位置。

- アプリケーションの起点サイト。

これらの場所からこれらの種類のファイルを識別し、読み込むための一貫したメカニズムを提供するために、WPF では、*パック URI スキーム*の拡張性が活用されています。 このトピックでは、スキームの概要について説明します。また、マークアップとコードの両方からパック Uri を使用する方法を説明する前に、さまざまなシナリオ用のパック Uri を作成する方法について説明します。

<a name="The_Pack_URI_Scheme"></a>

## <a name="the-pack-uri-scheme"></a>パック URI スキーム

パッケージ URI スキームは、コンテンツを整理および識別するためのモデルを記述する[Open パッケージング規則](https://www.ecma-international.org/publications/standards/Ecma-376.htm)(OPC) 仕様によって使用されます。 このモデルの主要な要素はパッケージとパーツで、*パッケージ*は1つ以上の論理*部分*の論理コンテナーです。 この概念を次の図に示します。

![パッケージとパーツのダイアグラム](./media/pack-uris-in-wpf/wpf-package-parts-diagram.png)

パーツを識別するために、OPC 仕様は、RFC 2396 (Uniform Resource Identifier (URI): Generic 構文) の機能拡張を利用して、パック URI スキームを定義します。

URI によって指定されるスキームは、そのプレフィックスによって定義されます。http、ftp、および file は、よく知られている例です。 パック URI スキームでは、スキームとして "pack" が使用され、authority と path の2つのコンポーネントが含まれています。 パック URI の形式を次に示します。

pack://*authority*/*パス*

*Authority*は、パーツが含まれているパッケージの種類を指定し、*パス*はパッケージ内のパーツの場所を指定します。

この概念を次の図に示します。

![パッケージ、権限、およびパスの間のリレーションシップ](./media/pack-uris-in-wpf/wpf-relationship-diagram.png)

パッケージとパーツは、アプリケーションとファイルに似ています。つまり、アプリケーション (パッケージ) は、次のような 1 つ以上のファイル (パーツ) を含むことができます。

- ローカル アセンブリにコンパイルされるリソース ファイル。

- 参照センブリにコンパイルされるリソース ファイル。

- 参照元センブリにコンパイルされるリソース ファイル。

- コンテンツ ファイル。

- 起点サイト ファイル。

WPF では、これらの種類のファイルにアクセスするために、application:///と siteoforigin:///の2つの機関がサポートされています。 application:/// オーソリティは、リソース ファイルやコンテンツ ファイルなど、コンパイル時に既知のアプリケーション データ ファイルを識別します。 siteoforigin:/// オーソリティは、起点サイト ファイルを識別します。 各オーソリティのスコープを次の図に示します。

![Pack URI のダイアグラム](./media/pack-uris-in-wpf/wpf-pack-uri-scheme.png)

> [!NOTE]
> パック URI の機関コンポーネントは、パッケージを指す埋め込み URI であり、RFC 2396 に準拠している必要があります。 さらに、"/" 文字は "," 文字に置き換える必要があり、"%" や "?" などの予約文字はエスケープする必要があります。 詳細については、OPC を参照してください。

以下のセクションでは、これらの2つの機関を使用して、リソース、コンテンツ、および起点サイトファイルを識別するための適切なパスと共に、パック Uri を作成する方法について説明します。

<a name="Resource_File_Pack_URIs___Local_Assembly"></a>

## <a name="resource-file-pack-uris"></a>リソース ファイルのパック URI

リソースファイルは MSBuild `Resource` 項目として構成され、アセンブリにコンパイルされます。 WPF は、ローカルアセンブリにコンパイルされるか、ローカルアセンブリから参照されるアセンブリにコンパイルされるリソースファイルを識別するために使用できる、パック Uri の構築をサポートしています。

<a name="Local_Assembly_Resource_File"></a>

### <a name="local-assembly-resource-file"></a>ローカル アセンブリ リソース ファイル

ローカルアセンブリにコンパイルされるリソースファイルのパック URI は、次の権限とパスを使用します。

- **オーソリティ**: application:///。

- **パス**: ローカル アセンブリのプロジェクト フォルダーのルートに対して相対的なパスを含むリソース ファイルの名前。

次の例は、ローカルアセンブリのプロジェクトフォルダーのルートにある [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] リソースファイルのパック URI を示しています。

`pack://application:,,,/ResourceFile.xaml`

次の例は、ローカルアセンブリのプロジェクトフォルダーのサブフォルダーにある、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] リソースファイルのパック URI を示しています。

`pack://application:,,,/Subfolder/ResourceFile.xaml`

<a name="Resource_File_Pack_URIs___Referenced_Assembly"></a>

### <a name="referenced-assembly-resource-file"></a>参照アセンブリ リソース ファイル

参照アセンブリにコンパイルされるリソースファイルのパック URI は、次の権限とパスを使用します。

- **オーソリティ**: application:///。

- **パス**: 参照アセンブリにコンパイルされるリソース ファイルの名前。 パスは、次の書式に従う必要があります。

  *Assemblyshortname*{ *;バージョン*] { *;PublicKey*]、コンポーネント/*パス*

  - **AssemblyShortName**: 参照アセンブリの短い名前。

  - **;Version** (省略可能): リソース ファイルを含む参照アセンブリのバージョン。 これは、同じ短い名前を持つ 2 つ以上の参照アセンブリが読み込まれるときに使用されます。

  - **;PublicKey** (省略可能): 参照アセンブリの署名に使用された公開鍵。 これは、同じ短い名前を持つ 2 つ以上の参照アセンブリが読み込まれるときに使用されます。

  - **;component**: 参照されるアセンブリが、ローカル アセンブリから参照されることを指定します。

  - **/Path**: 参照アセンブリのプロジェクト フォルダーのルートに対して相対的なパスを含むリソース ファイルの名前。

次の例は、参照アセンブリのプロジェクトフォルダーのルートにある [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] リソースファイルのパック URI を示しています。

`pack://application:,,,/ReferencedAssembly;component/ResourceFile.xaml`

次の例は、参照アセンブリのプロジェクトフォルダーのサブフォルダーにある、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] リソースファイルのパック URI を示しています。

`pack://application:,,,/ReferencedAssembly;component/Subfolder/ResourceFile.xaml`

次の例では、バージョン固有の参照アセンブリのプロジェクトフォルダーのルートフォルダーにある [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] リソースファイルのパック URI を示しています。

`pack://application:,,,/ReferencedAssembly;v1.0.0.1;component/ResourceFile.xaml`

参照されるアセンブリリソースファイルのパック URI 構文は、application:///authority でのみ使用できます。 たとえば、WPF では次のことがサポートされていません。

`pack://siteoforigin:,,,/SomeAssembly;component/ResourceFile.xaml`

<a name="Content_File_Pack_URIs"></a>

## <a name="content-file-pack-uris"></a>コンテンツ ファイルのパック URI

コンテンツファイルのパック URI は、次の証明機関とパスを使用します。

- **オーソリティ**: application:///。

- **パス**: アプリケーションのメインの実行可能アセンブリのファイル システム位置に対して相対的なパスを含む、コンテンツ ファイルの名前。

次の例は、実行可能アセンブリと同じフォルダーにある [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] コンテンツファイルのパック URI を示しています。

`pack://application:,,,/ContentFile.xaml`

次の例は、アプリケーションの実行可能アセンブリに対して相対的なサブフォルダーにある [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] コンテンツファイルのパック URI を示しています。

`pack://application:,,,/Subfolder/ContentFile.xaml`

> [!NOTE]
> HTML コンテンツファイルに移動することはできません。 URI スキームでは、起点サイトにある HTML ファイルへの移動のみがサポートされています。

<a name="The_siteoforigin_____Authority"></a>

## <a name="site-of-origin-pack-uris"></a>起点サイトのパック URI

起点サイトファイルのパック URI は、次の権限とパスを使用します。

- **オーソリティ**: siteoforigin:///。

- **パス**: 実行可能アセンブリの起動元の位置に対して相対的なパスを含む起点サイト ファイルの名前。

次の例は、実行可能アセンブリの起動元の場所に格納されている元の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] サイトのパッケージ URI を示しています。

`pack://siteoforigin:,,,/SiteOfOriginFile.xaml`

次の例は、アプリケーションの実行可能アセンブリの起動元の場所を基準としたサブフォルダーに格納されている、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] サイトの起点サイトファイルのパック URI を示しています。

`pack://siteoforigin:,,,/Subfolder/SiteOfOriginFile.xaml`

<a name="Page_Files"></a>

## <a name="page-files"></a>ページ ファイル

MSBuild `Page` 項目として構成された XAML ファイルは、リソースファイルと同じ方法でアセンブリにコンパイルされます。 そのため、リソースファイルのパック Uri を使用して、MSBuild `Page` 項目を識別できます。

通常、MSBuild`Page` 項目として構成される [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルの種類は、次のいずれかのルート要素となります。

- <xref:System.Windows.Window?displayProperty=nameWithType>

- <xref:System.Windows.Controls.Page?displayProperty=nameWithType>

- <xref:System.Windows.Navigation.PageFunction%601?displayProperty=nameWithType>

- <xref:System.Windows.ResourceDictionary?displayProperty=nameWithType>

- <xref:System.Windows.Documents.FlowDocument?displayProperty=nameWithType>

- <xref:System.Windows.Controls.UserControl?displayProperty=nameWithType>

<a name="Absolute_vs_Relative_Pack_URIs"></a>

## <a name="absolute-vs-relative-pack-uris"></a>絶対パック Uri と相対パッケージ Uri

完全修飾パック URI には、スキーム、権限、およびパスが含まれており、これは絶対パック URI と見なされます。 開発者のための単純化として、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] の要素では、通常、パスのみを含む、相対パック URI を使用して適切な属性を設定できます。

たとえば、ローカルアセンブリ内のリソースファイルの次の絶対パック URI について考えてみます。

`pack://application:,,,/ResourceFile.xaml`

このリソースファイルを参照する相対パック URI は、次のようになります。

`/ResourceFile.xaml`

> [!NOTE]
> 起点サイトファイルはアセンブリに関連付けられていないため、絶対パック Uri でのみ参照できます。

既定では、相対パック URI は、参照を含むマークアップまたはコードの位置を基準として相対的に見なされます。 ただし、先頭に円記号が使用されている場合、相対パック URI 参照は、アプリケーションのルートを基準として相対的に考慮されます。 たとえば、次のようなプロジェクト構造を考えてみます。

`App.xaml`

`Page2.xaml`

`\SubFolder`

`+ Page1.xaml`

`+ Page2.xaml`

Page1 に*ルート*\SubFolder\Page2.xaml を参照する uri が含まれている場合、参照は次の相対パック URI を使用できます。

`Page2.xaml`

Page1 に、*ルート*の url が含まれている場合、この参照では、次の相対パック uri を使用できます。

`/Page2.xaml`

<a name="Pack_URI_Resolution"></a>

## <a name="pack-uri-resolution"></a>パック URI の解決

パック Uri の形式により、さまざまな種類のファイルのパック Uri を同じように表示できます。 たとえば、次の絶対パック URI を考えてみます。

`pack://application:,,,/ResourceOrContentFile.xaml`

この絶対パック URI は、ローカルアセンブリまたはコンテンツファイル内のリソースファイルを参照できます。 これは、次の相対 URI にも当てはまります。

`/ResourceOrContentFile.xaml`

WPF は、パック URI が参照するファイルの種類を特定するために、次のヒューリスティックを使用して、ローカルアセンブリおよびコンテンツファイル内のリソースファイルの Uri を解決します。

1. パッケージの URI に一致する <xref:System.Windows.Resources.AssemblyAssociatedContentFileAttribute> 属性のアセンブリメタデータをプローブします。

2. <xref:System.Windows.Resources.AssemblyAssociatedContentFileAttribute> 属性が見つかった場合、パック URI のパスはコンテンツファイルを参照します。

3. <xref:System.Windows.Resources.AssemblyAssociatedContentFileAttribute> 属性が見つからない場合は、ローカルアセンブリにコンパイルされる set リソースファイルを調べます。

4. パック URI のパスに一致するリソースファイルが見つかった場合、パック URI のパスはリソースファイルを参照します。

5. リソースが見つからない場合、内部で作成された <xref:System.Uri> は無効です。

URI 解決は、次を参照する Uri には適用されません。

- 参照アセンブリ内のコンテンツファイル: これらのファイルの種類は、WPF ではサポートされていません。

- 参照アセンブリ内の埋め込みファイル: 参照されるアセンブリの名前と `;component` サフィックスの両方が含まれているため、それらを識別する Uri は一意です。

- 起点サイトファイル: siteoforigin:///機関を含むパック Uri によって識別できる唯一のファイルであるため、それらを識別する Uri は一意です。

パッケージ URI 解決が可能な1つの単純化は、コードがリソースファイルとコンテンツファイルの場所から多少独立していることです。 たとえば、ローカルアセンブリ内のリソースファイルがコンテンツファイルとして再構成されている場合、そのリソースのパック URI は、パック URI を使用するコードと同じように変わりません。

<a name="Programming_with_Pack_URIs"></a>

## <a name="programming-with-pack-uris"></a>パック URI を使用したプログラミング

多くの WPF クラスは、次のような、パック Uri を使用して設定できるプロパティを実装しています。

- <xref:System.Windows.Application.StartupUri%2A?displayProperty=nameWithType>

- <xref:System.Windows.Controls.Frame.Source%2A?displayProperty=nameWithType>

- <xref:System.Windows.Navigation.NavigationWindow.Source%2A?displayProperty=nameWithType>

- <xref:System.Windows.Documents.Hyperlink.NavigateUri%2A?displayProperty=nameWithType>

- <xref:System.Windows.Window.Icon%2A?displayProperty=nameWithType>

- <xref:System.Windows.Controls.Image.Source%2A?displayProperty=nameWithType>

これらのプロパティは、マークアップとコードのどちらからでも設定できます。 このセクションでは、両方の基本構造について説明してから、一般的なシナリオの例を示します。

<a name="Using_Pack_URIs_in_Markup"></a>

### <a name="using-pack-uris-in-markup"></a>マークアップでのパック URI の使用

パック URI をマークアップで指定するには、属性の要素にパック URI を設定します。 次に例を示します。

`<element attribute="pack://application:,,,/File.xaml" />`

表1は、マークアップで指定できるさまざまな絶対パック Uri を示しています。

表 1: マークアップでの絶対パック URI

|ファイル|絶対パック URI|
|----------|-------------------------------------------------------------------------------------------------------------------------|
|リソース ファイル - ローカル アセンブリ|`"pack://application:,,,/ResourceFile.xaml"`|
|サブフォルダー内のリソース ファイル - ローカル アセンブリ|`"pack://application:,,,/Subfolder/ResourceFile.xaml"`|
|リソース ファイル - 参照アセンブリ|`"pack://application:,,,/ReferencedAssembly;component/ResourceFile.xaml"`|
|参照アセンブリのサブフォルダー内のリソース ファイル|`"pack://application:,,,/ReferencedAssembly;component/Subfolder/ResourceFile.xaml"`|
|バージョン管理された参照アセンブリ内のリソース ファイル|`"pack://application:,,,/ReferencedAssembly;v1.0.0.0;component/ResourceFile.xaml"`|
|コンテンツ ファイル|`"pack://application:,,,/ContentFile.xaml"`|
|サブフォルダー内のコンテンツ ファイル|`"pack://application:,,,/Subfolder/ContentFile.xaml"`|
|起点サイト ファイル|`"pack://siteoforigin:,,,/SOOFile.xaml"`|
|サブフォルダー内の起点サイト ファイル|`"pack://siteoforigin:,,,/Subfolder/SOOFile.xaml"`|

表2は、マークアップで指定できるさまざまな相対パック Uri を示しています。

表 2: マークアップでの相対パック URI

|ファイル|相対パック URI|
|----------|-------------------------------------------------------------------------------------------------------------------------|
|ローカル アセンブリ内のリソース ファイル|`"/ResourceFile.xaml"`|
|ローカル アセンブリのサブフォルダー内のリソース ファイル|`"/Subfolder/ResourceFile.xaml"`|
|参照アセンブリ内のリソース ファイル|`"/ReferencedAssembly;component/ResourceFile.xaml"`|
|参照アセンブリのサブフォルダー内のリソース ファイル|`"/ReferencedAssembly;component/Subfolder/ResourceFile.xaml"`|
|コンテンツ ファイル|`"/ContentFile.xaml"`|
|サブフォルダー内のコンテンツ ファイル|`"/Subfolder/ContentFile.xaml"`|

<a name="Using_Pack_URIs_in_Code"></a>

### <a name="using-pack-uris-in-code"></a>コードでのパック URI の使用

コードでパック URI を指定するには、<xref:System.Uri> クラスをインスタンス化し、パック URI をパラメーターとしてコンストラクターに渡します。 これを次の例に示します。

```csharp
Uri uri = new Uri("pack://application:,,,/File.xaml");
```

既定では、<xref:System.Uri> クラスは、パック Uri を絶対値と見なします。 その結果、<xref:System.Uri> クラスのインスタンスが、相対パック URI を使用して作成されたときに例外が発生します。

```csharp
Uri uri = new Uri("/File.xaml");
```

幸いにも、<xref:System.Uri> クラスコンストラクターの <xref:System.Uri.%23ctor%28System.String%2CSystem.UriKind%29> オーバーロードは <xref:System.UriKind> 型のパラメーターを受け取り、パック URI が絶対か相対かを指定できます。

```csharp
// Absolute URI (default)
Uri absoluteUri = new Uri("pack://application:,,,/File.xaml", UriKind.Absolute);
// Relative URI
Uri relativeUri = new Uri("/File.xaml",
                        UriKind.Relative);
```

指定されたパック URI がいずれかであることが確実である場合は、<xref:System.UriKind.Absolute> または <xref:System.UriKind.Relative> のみを指定する必要があります。 実行時にユーザーがパック URI を入力したときなど、使用されているパック URI の種類がわからない場合は、代わりに <xref:System.UriKind.RelativeOrAbsolute> を使用します。

```csharp
// Relative or Absolute URI provided by user via a text box
TextBox userProvidedUriTextBox = new TextBox();
Uri uri = new Uri(userProvidedUriTextBox.Text, UriKind.RelativeOrAbsolute);
```

表3は、<xref:System.Uri?displayProperty=nameWithType>を使用してコードで指定できる、さまざまな相対パック Uri を示しています。

表 3: コードでの絶対パック URI

|ファイル|絶対パック URI|
|----------|-------------------------------------------------------------------------------------------------------------------------|
|リソース ファイル - ローカル アセンブリ|`Uri uri = new Uri("pack://application:,,,/ResourceFile.xaml", UriKind.Absolute);`|
|サブフォルダー内のリソース ファイル - ローカル アセンブリ|`Uri uri = new Uri("pack://application:,,,/Subfolder/ResourceFile.xaml", UriKind.Absolute);`|
|リソース ファイル - 参照アセンブリ|`Uri uri = new Uri("pack://application:,,,/ReferencedAssembly;component/ResourceFile.xaml", UriKind.Absolute);`|
|参照アセンブリのサブフォルダー内のリソース ファイル|`Uri uri = new Uri("pack://application:,,,/ReferencedAssembly;component/Subfolder/ResourceFile.xaml", UriKind.Absolute);`|
|バージョン管理された参照アセンブリ内のリソース ファイル|`Uri uri = new Uri("pack://application:,,,/ReferencedAssembly;v1.0.0.0;component/ResourceFile.xaml", UriKind.Absolute);`|
|コンテンツ ファイル|`Uri uri = new Uri("pack://application:,,,/ContentFile.xaml", UriKind.Absolute);`|
|サブフォルダー内のコンテンツ ファイル|`Uri uri = new Uri("pack://application:,,,/Subfolder/ContentFile.xaml", UriKind.Absolute);`|
|起点サイト ファイル|`Uri uri = new Uri("pack://siteoforigin:,,,/SOOFile.xaml", UriKind.Absolute);`|
|サブフォルダー内の起点サイト ファイル|`Uri uri = new Uri("pack://siteoforigin:,,,/Subfolder/SOOFile.xaml", UriKind.Absolute);`|

表4は、<xref:System.Uri?displayProperty=nameWithType>を使用してコードで指定できる、さまざまな相対パック Uri を示しています。

表 4: コードでの相対パック URI

|ファイル|相対パック URI|
|----------|-------------------------------------------------------------------------------------------------------------------------|
|リソース ファイル - ローカル アセンブリ|`Uri uri = new Uri("/ResourceFile.xaml", UriKind.Relative);`|
|サブフォルダー内のリソース ファイル - ローカル アセンブリ|`Uri uri = new Uri("/Subfolder/ResourceFile.xaml", UriKind.Relative);`|
|リソース ファイル - 参照アセンブリ|`Uri uri = new Uri("/ReferencedAssembly;component/ResourceFile.xaml", UriKind.Relative);`|
|サブフォルダー内のリソース ファイル - 参照アセンブリ|`Uri uri = new Uri("/ReferencedAssembly;component/Subfolder/ResourceFile.xaml", UriKind.Relative);`|
|コンテンツ ファイル|`Uri uri = new Uri("/ContentFile.xaml", UriKind.Relative);`|
|サブフォルダー内のコンテンツ ファイル|`Uri uri = new Uri("/Subfolder/ContentFile.xaml", UriKind.Relative);`|

<a name="Common_Pack_URI_Scenarios"></a>

### <a name="common-pack-uri-scenarios"></a>一般的なパック URI のシナリオ

前のセクションでは、リソース、コンテンツ、および起点サイトファイルを識別するために、パック Uri を作成する方法について説明しました。 WPF では、これらの構造はさまざまな方法で使用されます。次のセクションでは、いくつかの一般的な使用方法について説明します。

<a name="Specifying_the_UI_to_Show_when_an_Application_Starts"></a>

#### <a name="specifying-the-ui-to-show-when-an-application-starts"></a>アプリケーションの起動時に表示する UI の指定

<xref:System.Windows.Application.StartupUri%2A> は、WPF アプリケーションが起動されたときに表示する最初の [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] を指定します。 スタンドアロンアプリケーションの場合は、次の例に示すように、[!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] をウィンドウにすることができます。

[!code-xaml[PackURIOverviewSnippets#StartupUriWindow](~/samples/snippets/csharp/VS_Snippets_Wpf/PackURIOverviewSnippets/CS/Copy of App.xaml#startupuriwindow)]

次の例に示すように、スタンドアロンアプリケーションと XAML ブラウザーアプリケーション (Xbap) では、初期 UI としてページを指定することもできます。

[!code-xaml[PackURIOverviewSnippets#StartupUriPage](~/samples/snippets/csharp/VS_Snippets_Wpf/PackURIOverviewSnippets/CS/App.xaml#startupuripage)]

アプリケーションがスタンドアロンアプリケーションであり、<xref:System.Windows.Application.StartupUri%2A>でページが指定されている場合、WPF はページをホストするための <xref:System.Windows.Navigation.NavigationWindow> を開きます。 Xbap の場合、ページはホストブラウザーに表示されます。

<a name="Navigating_to_a_Page"></a>

#### <a name="navigating-to-a-page"></a>ページへのナビゲート

次の例は、ページにナビゲートする方法を示しています。

[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml1)]
[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml2)]
[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml3)]

WPF で移動するさまざまな方法の詳細については、「[ナビゲーションの概要](navigation-overview.md)」を参照してください。

<a name="Specifying_a_Window_Icon"></a>

#### <a name="specifying-a-window-icon"></a>ウィンドウ アイコンの指定

次の例は、URI を使用してウィンドウのアイコンを指定する方法を示しています。

[!code-xaml[WindowIconSnippets#WindowIconSetXAML](~/samples/snippets/xaml/VS_Snippets_Wpf/WindowIconSnippets/XAML/MainWindow.xaml#windowiconsetxaml)]

詳細については、<xref:System.Windows.Window.Icon%2A> を参照してください。

<a name="Loading_Image__Audio__and_Video_Files"></a>

#### <a name="loading-image-audio-and-video-files"></a>イメージ ファイル、オーディオ ファイル、およびビデオ ファイルの読み込み

WPF では、次の例に示すように、アプリケーションでさまざまな種類のメディアを使用して、そのすべてを識別し、パック Uri で読み込むことができます。

[!code-xaml[MediaPlayerVideoSample#VideoPackURIAtSOO](~/samples/snippets/csharp/VS_Snippets_Wpf/MediaPlayerVideoSample/CS/HomePage.xaml#videopackuriatsoo)]

[!code-xaml[MediaPlayerAudioSample#AudioPackURIAtSOO](~/samples/snippets/csharp/VS_Snippets_Wpf/MediaPlayerAudioSample/CS/HomePage.xaml#audiopackuriatsoo)]

[!code-xaml[ImageSample#ImagePackURIContent](~/samples/snippets/csharp/VS_Snippets_Wpf/ImageSample/CS/HomePage.xaml#imagepackuricontent)]

メディアコンテンツの操作の詳細については、「[グラフィックスとマルチメディア](../graphics-multimedia/index.md)」を参照してください。

<a name="Loading_a_Resource_Dictionary_from_the_Site_of_Origin"></a>

#### <a name="loading-a-resource-dictionary-from-the-site-of-origin"></a>起点サイトからのリソース ディクショナリの読み込み

リソースディクショナリ (<xref:System.Windows.ResourceDictionary>) は、アプリケーションテーマをサポートするために使用できます。 テーマを作成し、管理する方法の 1 つは、複数のテーマをリソース ディクショナリとして作成して、アプリケーションの起点サイトに配置することです。 これにより、アプリケーションを再コンパイルして再配置しなくても、テーマの追加と交信が可能です。 これらのリソースディクショナリは、次の例に示すように、パック Uri を使用して識別および読み込むことができます。

[!code-xaml[ResourceDictionarySnippets#ResourceDictionaryPackURI](~/samples/snippets/csharp/VS_Snippets_Wpf/ResourceDictionarySnippets/CS/App.xaml#resourcedictionarypackuri)]

WPF のテーマの概要については、「[スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)」を参照してください。

## <a name="see-also"></a>参照

- [WPF アプリケーションのリソース ファイル、コンテンツ ファイル、およびデータ ファイル](wpf-application-resource-content-and-data-files.md)
