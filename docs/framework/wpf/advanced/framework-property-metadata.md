---
title: フレームワーク プロパティ メタデータ
ms.date: 03/30/2017
helpviewer_keywords:
- metadata [WPF], framework properties
- framework property metadata [WPF]
ms.assetid: 9962f380-b885-4b61-a62e-457397083fea
ms.openlocfilehash: 3e40dfbcbe88f424337ee5a32fb3e3aaaddd68d0
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73460455"
---
# <a name="framework-property-metadata"></a>フレームワーク プロパティ メタデータ
フレームワーク プロパティ メタデータのオプションは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] アーキテクチャの WPF フレームワーク レベルにあると見なされるオブジェクト要素のプロパティに対して報告されます。 一般に、WPF フレームワークレベルの指定では、レンダリング、データバインディング、およびプロパティシステムの調整などの機能が [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プレゼンテーション Api と実行可能ファイルによって処理されます。 フレームワーク プロパティ メタデータがこれらのシステムによって照会されて、特定の要素プロパティに対する機能固有の特性が決まります。  

<a name="prerequisites"></a>   
## <a name="prerequisites"></a>必要条件  
 このトピックでは、ユーザーが [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] クラスの既存の依存関係プロパティの使用という観点から依存関係プロパティを理解し、「[依存関係プロパティの概要](dependency-properties-overview.md)」トピックを通読していることを前提としています。 また、「[依存関係プロパティのメタデータ](dependency-property-metadata.md)」を読んでいる必要もあります。  
  
<a name="What_Is_Communicated_by_Framework_Property"></a>   
## <a name="what-is-communicated-by-framework-property-metadata"></a>フレームワーク プロパティ メタデータによる通知内容  
 フレームワーク プロパティ メタデータは、次のように分類できます。  
  
- 要素 (<xref:System.Windows.FrameworkPropertyMetadata.AffectsArrange%2A>、<xref:System.Windows.FrameworkPropertyMetadata.AffectsMeasure%2A>、<xref:System.Windows.FrameworkPropertyMetadata.AffectsRender%2A>) に影響を与えるレポートレイアウトプロパティ。 プロパティがそれぞれの側面に影響する場合は、メタデータにこれらのフラグを設定できます。また、クラスに <xref:System.Windows.FrameworkElement.MeasureOverride%2A> / <xref:System.Windows.FrameworkElement.ArrangeOverride%2A> メソッドを実装して、レイアウトシステムに特定のレンダリング動作と情報を提供することもできます。 通常、そのような実装では、依存関係プロパティのメタデータでこれらのレイアウト プロパティのいずれかが true であると、プロパティの無効化があるかどうかがチェックされます。無効化があった場合にのみ、新しいレイアウト パスの要求が必要となります。  
  
- 要素の親要素 (<xref:System.Windows.FrameworkPropertyMetadata.AffectsParentArrange%2A>、<xref:System.Windows.FrameworkPropertyMetadata.AffectsParentMeasure%2A>) に影響を与えるレポートレイアウトプロパティ。 これらのフラグが既定で設定される例としては、<xref:System.Windows.Documents.FixedPage.Left%2A?displayProperty=nameWithType> と <xref:System.Windows.Documents.Paragraph.KeepWithNext%2A?displayProperty=nameWithType>があります。  
  
- <xref:System.Windows.FrameworkPropertyMetadata.Inherits%2A>. 既定では、依存関係プロパティは値を継承しません。 <xref:System.Windows.FrameworkPropertyMetadata.OverridesInheritanceBehavior%2A> を使用すると、継承の経路をビジュアルツリーに移動することもできます。これは、いくつかのコントロールの複合シナリオに必要です。  
  
    > [!NOTE]
    > プロパティ値のコンテキストにおける "継承" という用語は、依存関係プロパティに固有の事項を意味します。つまり、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] プロパティ システムの WPF フレームワーク レベルの機能によって、実際の依存関係プロパティ値を子要素が親要素から継承できることを意味します。 派生型を通じたマネージド コードの型およびメンバーの継承とは直接関係はありません。 詳細については、「[プロパティ値の継承](property-value-inheritance.md)」を参照してください。  
  
- レポートデータバインドの特性 (<xref:System.Windows.FrameworkPropertyMetadata.IsNotDataBindable%2A>、<xref:System.Windows.FrameworkPropertyMetadata.BindsTwoWayByDefault%2A>)。 既定では、フレームワークの依存関係プロパティは、一方向のバインディング動作を持つデータ バインディングをサポートします。 データバインディングを無効にすることもできます (柔軟性と拡張性を目的としているため、既定の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] Api では、このようなプロパティの例は多くありません)。 コンポーネント間でコントロールの動作を関連付けるプロパティ (<xref:System.Windows.Controls.MenuItem.IsSubmenuOpen%2A> など) や、ユーザーにとっては双方向のバインディングが一般的で期待されるシナリオであるプロパティ (<xref:System.Windows.Controls.TextBox.Text%2A> など) に対して、双方向のバインディングを設定できます。 データ バインディング関連のメタデータを変更した場合に影響を受けるのは既定値だけです。この既定値は、バインディングごとにいつでも変更できます。 バインディング モードおよびバインディング全般の詳細については、「[データ バインドの概要](../../../desktop-wpf/data/data-binding-overview.md)」を参照してください。  
  
- ジャーナル (<xref:System.Windows.FrameworkPropertyMetadata.Journal%2A>) をサポートするアプリケーションまたはサービスによってプロパティをジャーナル処理するかどうかを報告します。 一般的な要素に対しては、ジャーナリングは既定で有効になりませんが、特定のユーザー入力コントロールに対しては選択的に有効になります。 これは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のジャーナリングの実装を含む、ジャーナリング サービスによって読み取られるプロパティです。通常は、複数のナビゲーション ステップにわたって永続化する必要があるユーザー コントロール (一覧におけるユーザー選択など) に設定します。 ジャーナリングの詳細については、「[ナビゲーションの概要](../app-development/navigation-overview.md)」を参照してください。  
  
<a name="Reading_FrameworkPropertyMetadata"></a>   
## <a name="reading-frameworkpropertymetadata"></a>FrameworkPropertyMetadata の読み取り  
 上にリンクされた各プロパティは、<xref:System.Windows.FrameworkPropertyMetadata> が直接的な基本クラス <xref:System.Windows.UIPropertyMetadata>に追加する特定のプロパティです。 これらのプロパティの既定値はいずれも `false` です。 これらのプロパティの値がわかっているプロパティのメタデータ要求では、返されたメタデータを <xref:System.Windows.FrameworkPropertyMetadata>にキャストし、必要に応じて個々のプロパティの値を確認する必要があります。  
  
<a name="Specifying_Metadata"></a>   
## <a name="specifying-metadata"></a>メタデータの指定  
 新しい依存関係プロパティの登録にメタデータを適用するために新しいメタデータインスタンスを作成する場合、使用するメタデータクラスを選択できます。基本 <xref:System.Windows.PropertyMetadata>、または <xref:System.Windows.FrameworkPropertyMetadata>などの派生クラスです。 通常、<xref:System.Windows.FrameworkPropertyMetadata>を使用する必要があります。これは特に、プロパティがプロパティシステムと、レイアウトやデータバインディングなどの [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 関数との相互作用を持つ場合に使用します。 より高度なシナリオを実現するためのもう1つのオプションは、<xref:System.Windows.FrameworkPropertyMetadata> から派生させ、メンバーに追加情報を含む独自のメタデータレポートクラスを作成することです。 または、<xref:System.Windows.PropertyMetadata> または <xref:System.Windows.UIPropertyMetadata> を使用して、実装の機能のサポートレベルを伝えることもできます。  
  
 既存のプロパティ (<xref:System.Windows.DependencyProperty.AddOwner%2A> または <xref:System.Windows.DependencyProperty.OverrideMetadata%2A> 呼び出し) の場合は、常に元の登録で使用されているメタデータ型でオーバーライドする必要があります。  
  
 <xref:System.Windows.FrameworkPropertyMetadata> インスタンスを作成する場合は、次の2つの方法で、フレームワークのプロパティ特性を伝える特定のプロパティの値をそのメタデータに設定できます。  
  
1. `flags` パラメーターを許可する <xref:System.Windows.FrameworkPropertyMetadata> コンストラクターシグネチャを使用します。 このパラメーターには、<xref:System.Windows.FrameworkPropertyMetadataOptions> 列挙フラグのすべての必要な組み合わせ値を入力する必要があります。  
  
2. `flags` パラメーターを指定せずにいずれかの署名を使用し、<xref:System.Windows.FrameworkPropertyMetadata> の各レポートのブール型プロパティを `true` に設定します。 この場合、この依存関係プロパティを持つすべての要素の構築前に、これらのプロパティを設定する必要があります。`flags` パラメーターを使用せずに引き続きメタデータを読み込めるよう、これらのブール型プロパティは読み書き可能な状態ですが、プロパティの使用前にメタデータを事実上シールする必要があります。 そのため、メタデータの要求後にこれらのプロパティを設定しようとすると、無効な操作となります。  
  
<a name="Framework_Property_Metadata_Merge_Behavior"></a>   
## <a name="framework-property-metadata-merge-behavior"></a>フレームワーク プロパティ メタデータのマージ動作  
 フレームワーク プロパティ メタデータをオーバーライドすると、さまざまなメタデータ特性がマージされるか置き換えられます。  
  
- <xref:System.Windows.PropertyMetadata.PropertyChangedCallback%2A> がマージされます。 新しい <xref:System.Windows.PropertyMetadata.PropertyChangedCallback%2A>を追加すると、そのコールバックがメタデータに格納されます。 オーバーライドで <xref:System.Windows.PropertyMetadata.PropertyChangedCallback%2A> を指定しなかった場合、<xref:System.Windows.PropertyMetadata.PropertyChangedCallback%2A> の値は、メタデータで指定された最も近い先祖から参照として昇格されます。  
  
- <xref:System.Windows.PropertyMetadata.PropertyChangedCallback%2A> の実際のプロパティシステムの動作では、階層内のすべてのメタデータ所有者の実装が保持され、テーブルに追加されます。プロパティシステムによる実行順序により、最も深い派生クラスのコールバックが呼び出されることになります。まずは。 継承されたコールバックは、それらをメタデータに配置したクラスによって所有されていると見なされ、それぞれ 1 回だけ実行されます。  
  
- <xref:System.Windows.PropertyMetadata.DefaultValue%2A> が置き換えられます。 オーバーライドで <xref:System.Windows.PropertyMetadata.PropertyChangedCallback%2A> を指定しなかった場合、<xref:System.Windows.PropertyMetadata.DefaultValue%2A> の値はメタデータで指定された最も近い先祖から取得されます。  
  
- <xref:System.Windows.PropertyMetadata.CoerceValueCallback%2A> の実装は置き換えられます。 新しい <xref:System.Windows.PropertyMetadata.CoerceValueCallback%2A>を追加すると、そのコールバックがメタデータに格納されます。 オーバーライドで <xref:System.Windows.PropertyMetadata.CoerceValueCallback%2A> を指定しなかった場合、<xref:System.Windows.PropertyMetadata.CoerceValueCallback%2A> の値は、メタデータで指定された最も近い先祖から参照として昇格されます。  
  
- プロパティシステムの動作では、直前のメタデータ内の <xref:System.Windows.PropertyMetadata.CoerceValueCallback%2A> のみが呼び出されます。 階層内の他の <xref:System.Windows.PropertyMetadata.CoerceValueCallback%2A> 実装への参照は保持されません。  
  
- <xref:System.Windows.FrameworkPropertyMetadataOptions> 列挙型のフラグは、ビットごとの OR 演算として結合されます。  <xref:System.Windows.FrameworkPropertyMetadataOptions>を指定した場合、元のオプションは上書きされません。  オプションを変更するには、<xref:System.Windows.FrameworkPropertyMetadata>に対応するプロパティを設定します。 たとえば、元の <xref:System.Windows.FrameworkPropertyMetadata> オブジェクトで <xref:System.Windows.FrameworkPropertyMetadataOptions.NotDataBindable?displayProperty=nameWithType> フラグが設定されている場合、<xref:System.Windows.FrameworkPropertyMetadata.IsNotDataBindable%2A?displayProperty=nameWithType> を `false`に設定することによって変更できます。  
  
 この動作は <xref:System.Windows.FrameworkPropertyMetadata.Merge%2A>によって実装され、派生メタデータクラスでオーバーライドできます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.DependencyProperty.GetMetadata%2A>
- [依存関係プロパティのメタデータ](dependency-property-metadata.md)
- [依存関係プロパティの概要](dependency-properties-overview.md)
- [カスタム依存関係プロパティ](custom-dependency-properties.md)
