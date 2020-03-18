---
title: Freezable オブジェクトの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Freezable objects [WPF], description
- unfreezing Freezable objects [WPF]
- classes [WPF], Freezable
ms.assetid: 89c71692-4f43-4057-b611-67c6a8a863a2
ms.openlocfilehash: b1887afd19407898d8de1d92252e29778899fb89
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095191"
---
# <a name="freezable-objects-overview"></a>Freezable オブジェクトの概要

このトピックでは、<xref:System.Windows.Freezable> オブジェクトを効果的に使用および作成する方法について説明します。これにより、アプリケーションのパフォーマンスを向上させるための特別な機能が提供されます。 Freezable オブジェクトには、ブラシ、ペン、変換、ジオメトリ、アニメーションなどが含まれます。

<a name="whatisafreezable"></a>

## <a name="what-is-a-freezable"></a>Freezable オブジェクトとは

<xref:System.Windows.Freezable> は、固定されていない2つの状態を持つ特殊な種類のオブジェクトです。 固定されていない場合、<xref:System.Windows.Freezable> は他のオブジェクトと同様に動作するように見えます。 固定されていない場合は、<xref:System.Windows.Freezable> を変更できなくなります。

<xref:System.Windows.Freezable> には、オブジェクトに対する変更をオブザーバーに通知するための <xref:System.Windows.Freezable.Changed> イベントが用意されています。 <xref:System.Windows.Freezable> を固定すると、変更通知のリソースを消費する必要がなくなるため、パフォーマンスが向上します。 凍結された <xref:System.Windows.Freezable> はスレッド間で共有することもできますが、固定されていない <xref:System.Windows.Freezable> は使用できません。

<xref:System.Windows.Freezable> クラスには多くのアプリケーションがありますが、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 内の <xref:System.Windows.Freezable> のオブジェクトのほとんどは、グラフィックスサブシステムに関連しています。

<xref:System.Windows.Freezable> クラスを使用すると、特定のグラフィックスシステムオブジェクトを簡単に使用できるようになり、アプリケーションのパフォーマンスを向上させることができます。 <xref:System.Windows.Freezable> から継承する型の例には、<xref:System.Windows.Media.Brush>、<xref:System.Windows.Media.Transform>、および <xref:System.Windows.Media.Geometry> クラスが含まれます。 アンマネージ リソースが含まれているため、システムはこれらのオブジェクトの変更を監視し、変更時に対応するアンマネージ リソースを更新する必要があります。 グラフィックス システムのオブジェクトを実際には変更しない場合でも、変更される場合に備えて、システムはいくらかのリソースを使ってオブジェクトを監視する必要があります。

たとえば、<xref:System.Windows.Media.SolidColorBrush> ブラシを作成し、それを使用してボタンの背景を描画するとします。

[!code-csharp[freezablesample_procedural#FrozenExamplePart1](~/samples/snippets/csharp/VS_Snippets_Wpf/freezablesample_procedural/CSharp/freezablesample.cs#frozenexamplepart1)]
[!code-vb[freezablesample_procedural#FrozenExamplePart1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/freezablesample_procedural/visualbasic/freezablesample.vb#frozenexamplepart1)]

ボタンがレンダリングされると、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] グラフィックスサブシステムは、指定した情報を使用して、ピクセルのグループを描画し、ボタンの外観を作成します。 単色ブラシを使用してボタンを描画する方法について説明しますが、単色ブラシが実際に描画するのではありません。 グラフィックス システムは、ボタンやブラシに対応する高速で低レベルのオブジェクトを生成し、それが実際に画面に表示されます。

ブラシを変更する場合は、これらの低レベルのオブジェクトを再び生成する必要があります。 Freezable クラスは、ブラシに対応する低レベル オブジェクトを検索して変更時に更新する機能を付与します。 この機能が有効のとき、ブラシは「非フリーズ状態」であると言われます。

Freezable の <xref:System.Windows.Freezable.Freeze%2A> メソッドを使用すると、この自己更新機能を無効にすることができます。 このメソッドを使用すると、ブラシは「フリーズ状態」つまり変更不可能な状態になります。

> [!NOTE]
> すべての Freezable オブジェクトがフリーズできるわけではありません。 <xref:System.InvalidOperationException>がスローされないようにするには、Freezable オブジェクトの <xref:System.Windows.Freezable.CanFreeze%2A> プロパティの値を調べて、凍結を試行する前に固定できるかどうかを判断します。

[!code-csharp[freezablesample_procedural#FrozenExamplePart2](~/samples/snippets/csharp/VS_Snippets_Wpf/freezablesample_procedural/CSharp/freezablesample.cs#frozenexamplepart2)]
[!code-vb[freezablesample_procedural#FrozenExamplePart2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/freezablesample_procedural/visualbasic/freezablesample.vb#frozenexamplepart2)]

Freezable オブジェクトを変更する必要がなくなったときにそれをフリーズすることには、パフォーマンス上の利点があります。 この例のブラシをフリーズすれば、 ブラシが変更されないと分かっているので、グラフィックス システムはさらなる最適化をすることができます。

> [!NOTE]
> 便宜上、Freezable オブジェクトは、明示的にフリーズしない限りフリーズされません。

<a name="frozenfreezables"></a>

## <a name="using-freezables"></a>Freezable オブジェクトの使用

非フリーズの Freezable オブジェクトは他の型のオブジェクトと同様に使用できます。 次の例では、ボタンの背景を描画するために使用された <xref:System.Windows.Media.SolidColorBrush> の色が黄色から赤に変更されています。 グラフィックス システムは、次の画面更新時に、ボタンの色を黄色から赤に自動的に更新します。

[!code-csharp[freezablesample_procedural#UnFrozenExampleShort](~/samples/snippets/csharp/VS_Snippets_Wpf/freezablesample_procedural/CSharp/freezablesample.cs#unfrozenexampleshort)]
[!code-vb[freezablesample_procedural#UnFrozenExampleShort](~/samples/snippets/visualbasic/VS_Snippets_Wpf/freezablesample_procedural/visualbasic/freezablesample.vb#unfrozenexampleshort)]

### <a name="freezing-a-freezable"></a>Freezable オブジェクトをフリーズする

<xref:System.Windows.Freezable> 変更不可能な状態にするには、その <xref:System.Windows.Freezable.Freeze%2A> メソッドを呼び出します。 Freezable オブジェクトを格納しているオブジェクトをフリーズすると、それらのオブジェクトも同様にフリーズされます。 たとえば、<xref:System.Windows.Media.PathGeometry>を固定すると、その中に含まれる図形とセグメントも固定されます。

次のいずれかに該当する場合、Freezable をフリーズする**ことはできません**。

- アニメーション化されたまたは、データ バインドされたプロパティを持つ。

- 動的リソースによって設定されるプロパティを持つ。 (動的リソースの詳細については、「 [XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)」を参照してください)。

- これには、固定できない <xref:System.Windows.Freezable> サブオブジェクトが含まれます。

これらの条件が false で、<xref:System.Windows.Freezable>を変更する予定がない場合は、前に説明したパフォーマンス上の利点を得るために、これらの条件を固定する必要があります。

Freezable の <xref:System.Windows.Freezable.Freeze%2A> メソッドを呼び出すと、変更できなくなります。 固定されたオブジェクトを変更しようとすると、<xref:System.InvalidOperationException> がスローされます。 次のコードは、フリーズした後にブラシを変更しようとするので、例外をスローします。

[!code-csharp[freezablesample_procedural#ExceptionExample](~/samples/snippets/csharp/VS_Snippets_Wpf/freezablesample_procedural/CSharp/freezablesample.cs#exceptionexample)]
[!code-vb[freezablesample_procedural#ExceptionExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/freezablesample_procedural/visualbasic/freezablesample.vb#exceptionexample)]

この例外がスローされないようにするには、<xref:System.Windows.Freezable.IsFrozen%2A> メソッドを使用して、<xref:System.Windows.Freezable> が固定されているかどうかを確認します。

[!code-csharp[freezablesample_procedural#CheckIsFrozenExample](~/samples/snippets/csharp/VS_Snippets_Wpf/freezablesample_procedural/CSharp/freezablesample.cs#checkisfrozenexample)]
[!code-vb[freezablesample_procedural#CheckIsFrozenExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/freezablesample_procedural/visualbasic/freezablesample.vb#checkisfrozenexample)]

前のコード例では、<xref:System.Windows.Freezable.Clone%2A> メソッドを使用して、固定されたオブジェクトで変更可能なコピーを作成しました。 次のセクションでは、複製の作成について説明します。

> [!NOTE]
> フリーズした freezable はアニメーション化できないため、<xref:System.Windows.Media.Animation.Storyboard>でアニメーション化しようとすると、固定された <xref:System.Windows.Freezable> オブジェクトの変更可能な複製がアニメーションシステムによって自動的に作成されます。 複製によるパフォーマンスのオーバーヘッドを解消するために、アニメーション化する場合は、オブジェクトを解凍させないでください。 ストーリーボードを使用したアニメーション化の詳細については、「[ストーリーボードの概要](../graphics-multimedia/storyboards-overview.md)」を参照してください。

### <a name="freezing-from-markup"></a>マークアップからの凍結

マークアップで宣言された <xref:System.Windows.Freezable> オブジェクトを固定するには、`PresentationOptions:Freeze` 属性を使用します。 次の例では、<xref:System.Windows.Media.SolidColorBrush> がページリソースとして宣言され、固定されています。 次に、ボタンの背景を設定するために使用されます。

[!code-xaml[FreezableSample#FreezeFromMarkupWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/FreezableSample/CS/FreezeFromMarkupExample.xaml#freezefrommarkupwholepage)]

`Freeze` 属性を使用するには、[プレゼンテーションオプション] [名前空間: `http://schemas.microsoft.com/winfx/2006/xaml/presentation/options`] にマップする必要があります。 `PresentationOptions` は、この名前空間のマッピングに推奨されるプレフィックスです。

```xaml
xmlns:PresentationOptions="http://schemas.microsoft.com/winfx/2006/xaml/presentation/options"
```

すべての XAML リーダーがこの属性を認識しているわけではないため、 [mc: Ignorable 属性](mc-ignorable-attribute.md)を使用して `Presentation:Freeze` 属性を無視としてマークすることをお勧めします。

```xaml
xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
mc:Ignorable="PresentationOptions"
```

詳細については、「 [mc: Ignorable 属性](mc-ignorable-attribute.md)」ページを参照してください。

### <a name="unfreezing-a-freezable"></a>Freezable の "固定解除"

固定された <xref:System.Windows.Freezable> は、変更したり、凍結解除したりすることはできません。ただし、<xref:System.Windows.Freezable.Clone%2A> または <xref:System.Windows.Freezable.CloneCurrentValue%2A> 方法を使用して、凍結解除された複製を作成することができます。

次の例では、ボタンの背景がブラシで設定され、そのブラシが固定されています。 ブラシでは、<xref:System.Windows.Freezable.Clone%2A> メソッドを使用して、固定されていないコピーが作成されます。 複製が変更され、ボタンの背景を黄色から赤に変更するために使用されます。

[!code-csharp[freezablesample_procedural#CloneExample](~/samples/snippets/csharp/VS_Snippets_Wpf/freezablesample_procedural/CSharp/freezablesample.cs#cloneexample)]
[!code-vb[freezablesample_procedural#CloneExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/freezablesample_procedural/visualbasic/freezablesample.vb#cloneexample)]

> [!NOTE]
> 使用する複製方法に関係なく、アニメーションは新しい <xref:System.Windows.Freezable>にコピーされません。

<xref:System.Windows.Freezable.Clone%2A> メソッドと <xref:System.Windows.Freezable.CloneCurrentValue%2A> メソッドは、freezable のディープコピーを作成します。 フリーズ可能オブジェクトに他のフリーズされたフリーズ可能オブジェクトが含まれている場合は、それらも複製されて変更可能になります。 たとえば、固定された <xref:System.Windows.Media.PathGeometry> を複製して変更可能にする場合、含まれる図形とセグメントもコピーされ、変更可能になります。

<a name="createyourownfreezableclass"></a>

## <a name="creating-your-own-freezable-class"></a>独自の Freezable クラスを作成する

<xref:System.Windows.Freezable> から派生するクラスでは、次の機能が得られます。

- 特別な状態: 読み取り専用 (固定) と書き込み可能な状態。

- スレッドセーフ: 固定された <xref:System.Windows.Freezable> は、スレッド間で共有できます。

- 詳細な変更通知: 他の <xref:System.Windows.DependencyObject>とは異なり、Freezable オブジェクトは、サブプロパティ値が変更したときに変更通知を提供します。

- 簡単な複製: Freezable クラスには、ディープクローンを生成するいくつかのメソッドが既に実装されています。

<xref:System.Windows.Freezable> は <xref:System.Windows.DependencyObject>の一種であるため、依存関係プロパティシステムを使用します。 クラスのプロパティは依存関係プロパティである必要はありませんが、依存関係プロパティを使用すると、記述する必要のあるコードの量が少なくなります。これは、<xref:System.Windows.Freezable> クラスが依存関係プロパティを念頭に置いて設計されているためです。 依存関係プロパティシステムの詳細については、「[依存関係プロパティの概要](dependency-properties-overview.md)」を参照してください。

すべての <xref:System.Windows.Freezable> サブクラスは、<xref:System.Windows.Freezable.CreateInstanceCore%2A> メソッドをオーバーライドする必要があります。 クラスがすべてのデータに依存関係プロパティを使用している場合は、これで完了です。

クラスに依存関係のないプロパティのデータメンバーが含まれている場合は、次のメソッドもオーバーライドする必要があります。

- <xref:System.Windows.Freezable.CloneCore%2A>

- <xref:System.Windows.Freezable.CloneCurrentValueCore%2A>

- <xref:System.Windows.Freezable.GetAsFrozenCore%2A>

- <xref:System.Windows.Freezable.GetCurrentValueAsFrozenCore%2A>

- <xref:System.Windows.Freezable.FreezeCore%2A>

また、依存関係プロパティでないデータメンバーにアクセスして書き込む場合は、次の規則に従う必要があります。

- 非依存関係プロパティのデータメンバーを読み取る API の先頭で、<xref:System.Windows.Freezable.ReadPreamble%2A> メソッドを呼び出します。

- 非依存プロパティのデータメンバーを書き込む API の先頭で、<xref:System.Windows.Freezable.WritePreamble%2A> メソッドを呼び出します。 (API で <xref:System.Windows.Freezable.WritePreamble%2A> を呼び出した後に、依存関係のないプロパティのデータメンバーも読み取る場合は、<xref:System.Windows.Freezable.ReadPreamble%2A> を追加で呼び出す必要はありません)。

- 非依存プロパティのデータメンバーに書き込むメソッドを終了する前に、<xref:System.Windows.Freezable.WritePostscript%2A> メソッドを呼び出します。

クラスに <xref:System.Windows.DependencyObject> オブジェクトである非依存プロパティデータメンバーが含まれている場合は、メンバーを `null`に設定している場合でも、値のいずれかを変更するたびに <xref:System.Windows.Freezable.OnFreezablePropertyChanged%2A> メソッドを呼び出す必要があります。

> [!NOTE]
> 基本実装を呼び出すことで、オーバーライドする各 <xref:System.Windows.Freezable> メソッドを開始することが非常に重要です。

カスタム <xref:System.Windows.Freezable> クラスの例については、「[カスタムアニメーションのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/CustomAnimation)」を参照してください。

## <a name="see-also"></a>参照

- <xref:System.Windows.Freezable>
- [カスタム アニメーションのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/CustomAnimation)
- [依存関係プロパティの概要](dependency-properties-overview.md)
- [カスタム依存関係プロパティ](custom-dependency-properties.md)
