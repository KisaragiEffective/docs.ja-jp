---
title: PropertyPath の XAML 構文
ms.date: 03/30/2017
helpviewer_keywords:
- PropertyPath object [WPF]
- XAML [WPF], PropertyPath object
ms.assetid: 0e3cdf07-abe6-460a-a9af-3764b4fd707f
ms.openlocfilehash: 17c8982a66960626a5d049fa2da90f5f2d995d14
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559769"
---
# <a name="propertypath-xaml-syntax"></a>PropertyPath の XAML 構文

<xref:System.Windows.PropertyPath> オブジェクトは、<xref:System.Windows.PropertyPath> 型を値として受け取るさまざまなプロパティを設定するための複雑なインライン [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 構文をサポートしています。 このトピックでは、バインドとアニメーションの構文に適用される <xref:System.Windows.PropertyPath> 構文について説明します。

<a name="where"></a>

## <a name="where-propertypath-is-used"></a>PropertyPath を使用する場所

<xref:System.Windows.PropertyPath> は、いくつかの [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 機能で使用される共通のオブジェクトです。 共通の <xref:System.Windows.PropertyPath> を使用したプロパティパス情報の伝達にもかかわらず、<xref:System.Windows.PropertyPath> が型として使用される各機能領域の使用方法は異なります。 そのため、機能ごとに構文を説明する方が実際的です。

主に、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は <xref:System.Windows.PropertyPath> を使用してオブジェクトデータソースのプロパティを走査するためのオブジェクトモデルパスを記述し、対象となるアニメーションのターゲットパスを記述します。

<xref:System.Windows.Setter.Property%2A?displayProperty=nameWithType> などの一部のスタイルおよびテンプレートプロパティは、<xref:System.Windows.PropertyPath>に似た修飾プロパティ名を受け取ります。 ただし、これは真の <xref:System.Windows.PropertyPath>ではありません。代わりに、修飾された*所有者です。* WPF [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサによって有効にされるプロパティ文字列形式の使用法は、<xref:System.Windows.DependencyProperty>の型コンバーターと組み合わせて使用します。

<a name="databinding_s"></a>

## <a name="propertypath-for-objects-in-data-binding"></a>データ バインディングにおけるオブジェクトの PropertyPath

データ バインディングは [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の機能であり、依存関係プロパティのターゲット値にバインドできます。 ただし、このようなデータ バインディングのソースが依存関係プロパティである必要はなく、適切なデータ プロバイダーで認識される任意のプロパティ型でかまいません。 プロパティパスは、共通言語ランタイム (CLR) オブジェクトとそのプロパティからバインディングソースを取得するために使用される <xref:System.Windows.Data.ObjectDataProvider>に特に使用されます。

XML へのデータバインドでは、<xref:System.Windows.Data.Binding>で <xref:System.Windows.Data.Binding.Path%2A> が使用されないため、<xref:System.Windows.PropertyPath>は使用されません。 代わりに、<xref:System.Windows.Data.Binding.XPath%2A> を使用し、データの XML ドキュメントオブジェクトモデル (DOM) に有効な XPath 構文を指定します。 <xref:System.Windows.Data.Binding.XPath%2A> も文字列として指定されていますが、ここには記載されていません。「 [XMLDataProvider と XPath クエリを使用して XML データにバインドする](../data/how-to-bind-to-xml-data-using-an-xmldataprovider-and-xpath-queries.md)」を参照してください。

データ バインディングにおけるプロパティ パスを理解するうえで鍵となるのは、個々のプロパティ値をバインドの対象にすることも、リストまたはコレクションを使用するターゲット プロパティにバインドすることもできるということです。 コレクション内のデータ項目の数に応じて拡張される <xref:System.Windows.Controls.ListBox> をバインドする場合は、コレクション内のデータ項目の数に応じてプロパティのパスでコレクションオブジェクトを参照する必要があります。 データバインディングエンジンは、データソースとして使用されるコレクションをバインディングターゲットの型に自動的に一致させます。これにより、項目配列に <xref:System.Windows.Controls.ListBox> を設定するなどの動作が行われます。

<a name="singlecurrent"></a>

### <a name="single-property-on-the-immediate-object-as-data-context"></a>データ コンテキストとしての直接のオブジェクト上の単一のプロパティ

```xml
<Binding Path="propertyName" .../>
```

*propertyName*は、<xref:System.Windows.Data.Binding.Path%2A> の使用について、現在の <xref:System.Windows.FrameworkElement.DataContext%2A> 内のプロパティの名前に解決される必要があります。 バインドがソースを更新する場合、そのプロパティは読み書き可能であり、ソース オブジェクトは変更可能である必要があります。

<a name="singleindex"></a>

### <a name="single-indexer-on-the-immediate-object-as-data-context"></a>データ コンテキストとしての直接のオブジェクト上の単一のインデクサー

```xml
<Binding Path="[key]" .../>
```

`key` には、ディクショナリまたはハッシュ テーブルに対する型指定されたインデックス、または配列の整数インデックスを指定する必要があります。 また、キーの値は、適用先のプロパティに直接バインドできる型である必要があります。 たとえば、文字列キーと文字列値を含むハッシュテーブルをこの方法で使用して、<xref:System.Windows.Controls.TextBox>のテキストにバインドすることができます。 キーがコレクションまたはサブインデックスを指す場合は、この構文を使用して、ターゲット コレクション プロパティにバインドできます。 それ以外の場合は、`<Binding Path="[key].propertyName" .../>` などの構文を通じて、特定のプロパティを参照する必要があります。

必要に応じて、インデックスの型を指定できます。 インデックス付きプロパティパスのこの側面の詳細については、「<xref:System.Windows.Data.Binding.Path%2A?displayProperty=nameWithType>」を参照してください。

<a name="multipleindirect"></a>

### <a name="multiple-property-indirect-property-targeting"></a>複数プロパティ (間接的なプロパティのターゲット設定)

```xml
<Binding Path="propertyName.propertyName2" .../>
```

`propertyName` は、現在の <xref:System.Windows.FrameworkElement.DataContext%2A>のプロパティの名前に解決される必要があります。 パス プロパティ `propertyName` と `propertyName2` は、リレーションシップ内に存在する任意のプロパティにすることができます。`propertyName2` は、`propertyName` の値である型に存在するプロパティです。

<a name="singleattached"></a>

### <a name="single-property-attached-or-otherwise-type-qualified"></a>添付または型修飾された単一のプロパティ

```xml
<object property="(ownerType.propertyName)" .../>
```

かっこは、<xref:System.Windows.PropertyPath> 内のこのプロパティを、部分修飾を使用して構築する必要があることを示します。 XML 名前空間を使用して、適切なマッピングを含む型を検出できます。 `ownerType` は、各アセンブリの <xref:System.Windows.Markup.XmlnsDefinitionAttribute> 宣言を通じて、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサがアクセスできる型を検索します。 ほとんどのアプリケーションは、`http://schemas.microsoft.com/winfx/2006/xaml/presentation` 名前空間にマップされた既定の XML 名前空間を持ちます。そのため、プレフィックスは通常、カスタム型や、名前空間の外部の型に限って必要です。  `propertyName` は、`ownerType` に存在するプロパティの名前に解決される必要があります。 この構文は、通常、次のいずれかの場合に使用されます。

- 指定されたターゲット型を持たないスタイルまたはテンプレート内の [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] でパスが指定されている場合。 通常、これ以外のケースでの修飾子の使用は無効です。スタイルやテンプレート以外のケースでは、プロパティは型ではなくインスタンス上に存在します。

- プロパティが添付プロパティの場合。

- 静的プロパティにバインドしている場合。

ストーリーボードターゲットとして使用するには、`propertyName` として指定されたプロパティが <xref:System.Windows.DependencyProperty>である必要があります。

<a name="sourcetraversal"></a>

### <a name="source-traversal-binding-to-hierarchies-of-collections"></a>ソースの走査 (コレクションの階層へのバインド)

```xml
<object Path="propertyName/propertyNameX" .../>
```

この構文で、/ は階層的なデータ ソース オブジェクト内を移動するために使用されます。また、/ 文字を連続で使用することによる階層内での複数ステップの移動がサポートされています。 ソースの走査は現在のレコード ポインター位置に対応しており、これはデータをビューの UI と同期することによって決定されます。 階層的なデータ ソース オブジェクトのバインドおよびデータ バインディングにおける現在のレコード ポインターの概念の詳細については、「[階層データでマスター詳細パターンを使用する](../data/how-to-use-the-master-detail-pattern-with-hierarchical-data.md)」または「[データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)」を参照してください。

> [!NOTE]
> 、一見、では、この構文は XPath に似ています。 XML データソースにバインドするための true XPath 式は、<xref:System.Windows.Data.Binding.Path%2A> 値として使用されません。代わりに、相互排他的な <xref:System.Windows.Data.Binding.XPath%2A> プロパティに使用する必要があります。

### <a name="collection-views"></a>コレクション ビュー

名前付きコレクション ビューを参照するには、コレクション ビュー名の前にハッシュ記号 (`#`) を付けます。

### <a name="current-record-pointer"></a>現在のレコード ポインター

コレクション ビューまたはマスター詳細データ バインディング シナリオの現在のレコード ポインターを参照するには、パス文字列の先頭にスラッシュ (`/`) を追加します。 スラッシュより後ろのパスは、現在のレコード ポインターから開始して走査されます。

### <a name="multiple-indexers"></a>複数のインデクサー

```xaml
<object Path="[index1,index2...]" .../>
```

または

```xaml
<object Path="propertyName[index,index2...]" .../>
```

あるオブジェクトが複数のインデクサーをサポートする場合、それらのインデクサーは、配列参照構文のように、順番に指定できます。 該当のオブジェクトは、現在のコンテキスト、または複数のインデックス オブジェクトが含まれるプロパティの値です。

既定では、インデクサー値は、基になるオブジェクトの特性を使用して型指定されます。 必要に応じて、インデックスの型を指定できます。 インデクサーの入力の詳細については、「<xref:System.Windows.Data.Binding.Path%2A?displayProperty=nameWithType>」を参照してください。

<a name="mixing"></a>

### <a name="mixing-syntaxes"></a>構文の混合

上記の各構文は、混在させることができます。 たとえば、次に示すのは、<xref:System.Windows.Media.SolidColorBrush> オブジェクトのピクセルグリッド配列を含む `ColorGrid` プロパティの特定の x、y にある色へのプロパティパスを作成する例です。

```xml
<Rectangle Fill="{Binding ColorGrid[20,30].SolidColorBrushResult}" .../>
```

### <a name="escapes-for-property-path-strings"></a>プロパティ パス文字列のエスケープ

特定のビジネス オブジェクトでは、正しく解析するために、プロパティ パス文字列にエスケープ シーケンスが必要な場合があります。 このような文字の多くには、通常ビジネス オブジェクトを定義するために使用される言語において、名前付けの相互作用に関する同様の問題があるため、エスケープが必要になることはほとんどありません。

- インデクサー([ ]) 内部では、キャレット文字 (^) は次の文字をエスケープします。

- XML 言語定義で特別な意味を持つ特定の文字を (XML エンティティを使用して) エスケープする必要があります。 「&」をエスケープするには `&` を使用します。 使用する`>`終了タグをエスケープする">"です。

- マークアップ拡張機能の処理に関する WPF XAML パーサーの動作で特別な意味を持つ文字を、(バックスラッシュ `\` を使用して) エスケープする必要があります。

  - 円記号 (`\`) はエスケープ文字そのものです。

  - 等号 (`=`) は、プロパティ名とプロパティ値を区切ります。

  - コンマ (`,`) は、複数のプロパティを区切ります。

  - 右中かっこ (`}`) は、マークアップ拡張機能の終了を示します。

> [!NOTE]
> 厳密に言うと、これらのエスケープはストーリーボードのプロパティ パスに役立ちますが、通常は既存の WPF オブジェクトのオブジェクト モデルを走査するので、エスケープは不要です。

<a name="databinding_sa"></a>

## <a name="propertypath-for-animation-targets"></a>アニメーション ターゲットの PropertyPath

アニメーションの target プロパティは、<xref:System.Windows.Freezable> またはプリミティブ型のいずれかを受け取る依存関係プロパティである必要があります。 ただし、型のターゲット プロパティと最終的なアニメーション プロパティは異なるオブジェクト上に存在できます。 アニメーションの場合は、プロパティ パスを使用して、プロパティ値内のオブジェクトとプロパティのリレーションシップを走査することによって、名前付きのアニメーション ターゲット オブジェクトのプロパティと目的のターゲット アニメーション プロパティの間の接続が定義されます。

<a name="general"></a>

### <a name="general-object-property-considerations-for-animations"></a>アニメーションに関するオブジェクトとプロパティの一般的な注意事項

一般的なアニメーションの概念の詳細については、「[ストーリーボードの概要](../graphics-multimedia/storyboards-overview.md)」および「[アニメーションの概要](../graphics-multimedia/animation-overview.md)」を参照してください。

アニメーション化する値の型またはプロパティは、<xref:System.Windows.Freezable> 型またはプリミティブである必要があります。 パスを開始するプロパティは、指定された <xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> の種類に存在する依存関係プロパティの名前に解決される必要があります。

既に固定されている <xref:System.Windows.Freezable> をアニメーション化するための複製をサポートするには、<xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> によって指定されたオブジェクトが <xref:System.Windows.FrameworkElement> または <xref:System.Windows.FrameworkContentElement> の派生クラスである必要があります。

<a name="singlestepanim"></a>

### <a name="single-property-on-the-target-object"></a>ターゲット オブジェクト上の単一プロパティ

```xml
<animation Storyboard.TargetProperty="propertyName" .../>
```

`propertyName` は、指定された <xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> 型に存在する依存関係プロパティの名前に解決される必要があります。

<a name="indirectanim"></a>

### <a name="indirect-property-targeting"></a>間接的なプロパティのターゲット設定

```xml
<animation Storyboard.TargetProperty="propertyName.propertyName2" .../>
```

`propertyName` は、<xref:System.Windows.Freezable> 値型または指定された <xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> 型に存在するプリミティブであるプロパティである必要があります。

`propertyName2` は、`propertyName` の値であるオブジェクトに存在する依存関係プロパティの名前である必要があります。 言い換えると、`propertyName2` は `propertyName` <xref:System.Windows.DependencyProperty.PropertyType%2A>である型の依存関係プロパティとして存在する必要があります。

適用されるスタイルとテンプレートにより、アニメーションの間接的なターゲット設定が必要です。 アニメーションを対象とするには、ターゲットオブジェクトの <xref:System.Windows.Media.Animation.Storyboard.TargetName%2A> が必要です。この名前は、 [x:Name](../../../desktop-wpf/xaml-services/xname-directive.md)または <xref:System.Windows.FrameworkElement.Name%2A>によって確立されます。 テンプレート要素とスタイル要素にも名前を付けることができますが、これらの名前はスタイルとテンプレートの名前スコープ内でのみ有効です。 (テンプレートとスタイルで名前スコープがアプリケーションマークアップと共有された場合、名前を一意にすることはできません。 スタイルとテンプレートは、文字どおりインスタンス間で共有され、重複する名前が perpetuate ます。)したがって、アニメーション化する要素の個々のプロパティがスタイルまたはテンプレートからのものである場合は、スタイルテンプレートではない名前付き要素インスタンスを使用して開始し、そのプロパティに到達するようにスタイルまたはテンプレートのビジュアルツリーを対象にする必要があります。アニメーションを作成します。

たとえば、<xref:System.Windows.Controls.Panel> の <xref:System.Windows.Controls.Panel.Background%2A> プロパティは、テーマテンプレートに由来する完全な <xref:System.Windows.Media.Brush> (実際は <xref:System.Windows.Media.SolidColorBrush>) です。 <xref:System.Windows.Media.Brush> を完全にアニメーション化するには、BrushAnimation (おそらく、<xref:System.Windows.Media.Brush> の種類ごとに1つ) が必要であり、そのような型は存在しません。 ブラシをアニメーション化するには、代わりに特定の <xref:System.Windows.Media.Brush> の種類のプロパティをアニメーション化します。 <xref:System.Windows.Media.Animation.ColorAnimation> を適用するには、<xref:System.Windows.Media.SolidColorBrush> から <xref:System.Windows.Media.SolidColorBrush.Color%2A> にアクセスする必要があります。 この例のプロパティ パスは `Background.Color` です。

<a name="attachedanim"></a>

### <a name="attached-properties"></a>添付プロパティ

```xml
<animation Storyboard.TargetProperty="(ownerType.propertyName)" .../>
```

かっこは、<xref:System.Windows.PropertyPath> 内のこのプロパティを、部分修飾を使用して構築する必要があることを示します。 XML 名前空間を使用して型を検出できます。 `ownerType` は、各アセンブリの <xref:System.Windows.Markup.XmlnsDefinitionAttribute> 宣言を通じて、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサがアクセスできる型を検索します。 ほとんどのアプリケーションは、`http://schemas.microsoft.com/winfx/2006/xaml/presentation` 名前空間にマップされた既定の XML 名前空間を持ちます。そのため、プレフィックスは通常、カスタム型や、名前空間の外部の型に限って必要です。 `propertyName` は、`ownerType` に存在するプロパティの名前に解決される必要があります。 `propertyName` として指定されたプロパティは、<xref:System.Windows.DependencyProperty>である必要があります。 (アタッチされた [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] すべてのプロパティは依存関係プロパティとして実装されるため、この問題はカスタム添付プロパティに関してのみ問題になります)。

<a name="indexanim"></a>

### <a name="indexers"></a>インデクサー

```xml
<animation Storyboard.TargetProperty="propertyName.propertyName2[index].propertyName3" .../>
```

ほとんどの依存関係プロパティまたは <xref:System.Windows.Freezable> 型では、インデクサーはサポートされていません。 そのため、アニメーション パス内でインデクサーを使用するのは、名前付きターゲット上でチェーンを開始するプロパティと、最終的にアニメーション化されるプロパティの間の中間位置に限られます。 提供される構文の中で、これは `propertyName2` です。 たとえば、中間プロパティが `RenderTransform.Children[1].Angle`などのプロパティパスで <xref:System.Windows.Media.TransformGroup>などのコレクションである場合は、インデクサーの使用が必要になることがあります。

<a name="ppincode"></a>

## <a name="propertypath-in-code"></a>コード内の PropertyPath

<xref:System.Windows.PropertyPath>の作成方法を含む <xref:System.Windows.PropertyPath>のコード使用方法については、<xref:System.Windows.PropertyPath>のリファレンストピックを参照してください。

一般に、<xref:System.Windows.PropertyPath> は2つの異なるコンストラクターを使用するように設計されています。1つはバインディングの使用状況と最も単純なアニメーションの使用で、もう1つは複雑なアニメーションの使用用です。 バインディングの使用には <xref:System.Windows.PropertyPath.%23ctor%28System.Object%29> 署名を使用します。この場合、オブジェクトは文字列です。 1ステップのアニメーションパスには <xref:System.Windows.PropertyPath.%23ctor%28System.Object%29> 署名を使用します。この場合、オブジェクトは <xref:System.Windows.DependencyProperty>です。 複雑なアニメーションには <xref:System.Windows.PropertyPath.%23ctor%28System.String%2CSystem.Object%5B%5D%29> 署名を使用します。 後者のコンストラクターでは、先頭のパラメーター用のトークン文字列と、トークン文字列内の位置に設定されるオブジェクトの配列を使用して、プロパティ パスのリレーションシップを定義します。

## <a name="see-also"></a>関連項目

- <xref:System.Windows.PropertyPath>
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [ストーリーボードの概要](../graphics-multimedia/storyboards-overview.md)
