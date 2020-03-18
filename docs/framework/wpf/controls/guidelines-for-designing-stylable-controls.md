---
title: スタイルの設定が可能なコントロールを設計するためのガイドライン
ms.date: 03/30/2017
helpviewer_keywords:
- style design for controls [WPF]
- controls [WPF], style design
ms.assetid: c52dde45-a311-4531-af4c-853371c4d5f4
ms.openlocfilehash: 0fbb515afbeac05168ced6f0a99f50eb29a5c848
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459649"
---
# <a name="guidelines-for-designing-stylable-controls"></a>スタイルの設定が可能なコントロールを設計するためのガイドライン

このドキュメントは、スタイルの設定とテンプレートの作成を簡単に行うためのコントロールを設計する際に考慮すべき一連のベスト プラクティスをまとめたものです。 組み込みの [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コントロール セットのテーマのコントロールのスタイルの操作で試行錯誤を繰り返した結果、この一連のベスト プラクティスにたどり着きました。 スタイリングの成功の鍵は、スタイルそのものであると同様に、適切に設計されたオブジェクト モデルの機能であることが分かりました。 このドキュメントの対象読者は、スタイルの作成者ではなく、コントロールの作成者です。

<a name="Terminology"></a>

## <a name="terminology"></a>用語

「スタイルとテンプレート」は、コントロールの作成者がコントロールの視覚的な側面をコントロールのスタイルとテンプレートに委ねることができるようにする一連のテクノロジを表します。 この一連のテクノロジは次のとおりです。

- スタイル (プロパティ セッター、トリガー、およびストーリー ボードを含む)。

- リソース

- コントロール テンプレート

- データ テンプレート

スタイルとテンプレートの概要については、「[スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)」を参照してください。

<a name="Before_You_Start__Understanding_Your_Control"></a>

## <a name="before-you-start-understanding-your-control"></a>開始する前に: コントロールの理解

次のガイドラインに進む前に、コントロールの一般的な使用方法を理解し、これを定義する必要があります。 スタイル設定は、規則に従わないことが多い可能性セットに遭遇します。 (多くのアプリケーションで、多くの開発者によって) 広く使用されるように作成されるコントロールは、コントロールの視覚的な外観に広範な変更を加えるためにスタイル設定を使用できるという課題に直面しています。 実際には、スタイルのコントロールは、コントロールの作成者の意図に沿っていない場合すらあります。 スタイル設定によって提供される柔軟性は実質的に無制限であるため、一般的な使用方法の概念を使用して、決定を詳しく調査できます。

コントロールの一般的な使用方法を理解するには、コントロールの価値提案について考慮することをお勧めします。 作成したコントロールがテーブルにもたらすもので、その他のコントロールが提供できないものは何でしょうか。 一般的な使用方法は、特定の視覚的な外観を意味するのではなく、コントロールの原理とその使用法の妥当な一連の想定を意味します。 このことを理解すると、一般的な場合での、構成モデルとコントロールのスタイル定義の動作に関するある想定を行うことができます。 たとえば、<xref:System.Windows.Controls.ComboBox>の場合、一般的な使用法を理解しても、特定の <xref:System.Windows.Controls.ComboBox> の角が丸くなっているかどうかについての洞察は得られませんが、<xref:System.Windows.Controls.ComboBox> がポップアップウィンドウと、開いているかどうかを切り替えます。

<a name="General_Guidelines"></a>

## <a name="general-guidelines"></a>一般的なガイドライン

- **テンプレートのコントラクトは厳密に適用しないでください。** コントロールのテンプレートのコントラクトは、要素、コマンド、バインディングまたはトリガーで構成されます。また、コントロールが正しく動作するために必要なまたは期待されているプロパティ設定が含まれる場合もあります。

  - コントラクトを可能な限り最小限に抑えます。

  - 設計中 (つまり、設計ツールを使用時) にはコントロール テンプレートが未完成の状態であることが一般的です。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は「構成」状態のインフラストラクチャを提供しないため、コントロールは、このような状態が有効かもしれないという想定で構築する必要があります。

  - テンプレートのコントラクトの側面に従わない場合、例外をスローしないでください。 これらすべての点で、パネルの子が多すぎるまたは少なすぎる場合、例外をスローしないでください。

- **周辺機能をテンプレートのヘルパー要素に組み込んでください。** 各コントロールは、コア機能と真の価値提案に重点を置き、コントロールの一般的な使用方法で定義されている必要があります。 このために、テンプレート内で構成要素とヘルパー要素を使用し、周辺の動作およびビジュアル (つまり、コントロールのコア機能に関係のない動作とビジュアル) を有効にします。 ヘルパー要素は次の 3 つのカテゴリに分類されます。

  - **Standalone** ヘルパー型は、テンプレートで「匿名」で使用される、パブリックで再利用可能なコントロールまたはあるプリミティブです。つまり、ヘルパー要素もスタイル設定されたコントロールも他方を認識しません。 技術的には、任意の要素を匿名型にできますが、このコンテキストではこの用語は、対象となるシナリオを有効にする専用機能をカプセル化するこれらの型について説明します。

  - **Type-based** ヘルパー要素は、専用機能をカプセル化する新しい型です。 これらの要素は通常、一般的なコントロールまたはプリミティブより狭い範囲の機能を持つように設計されています。 Standalone ヘルパー要素とは異なり、Type-based ヘルパー要素は、これが使用されるコンテキストを認識し、通常これが属しているテンプレートを持つコントロールとデータを共有する必要があります。

  - **Named** ヘルパー要素は、コントロールがテンプレート内で名前で検索することを想定している一般的なコントロールまたはプリミティブです。 これらの要素には、テンプレート内で既知の名前が与えられ、コントロールが要素を検索し、プログラムでやり取りできるようにします。 特定の名前を持つ要素はテンプレート内に 1 つのみ存在できます。

  次の表は、現在コントロール スタイルで採用されているヘルパー要素を示しています (この一覧は完全ではありません)。

  |要素|[種類]|使用者|
  |-------------|----------|-------------|
  |<xref:System.Windows.Controls.ContentPresenter>|Type-based|<xref:System.Windows.Controls.Button>、<xref:System.Windows.Controls.CheckBox>、<xref:System.Windows.Controls.RadioButton>、<xref:System.Windows.Controls.Frame>など (すべての <xref:System.Windows.Controls.ContentControl> の種類)|
  |<xref:System.Windows.Controls.ItemsPresenter>|Type-based|<xref:System.Windows.Controls.ListBox>、<xref:System.Windows.Controls.ComboBox>、<xref:System.Windows.Controls.Menu>など (すべての <xref:System.Windows.Controls.ItemsControl> の種類)|
  |<xref:System.Windows.Controls.Primitives.ToolBarOverflowPanel>|Named|<xref:System.Windows.Controls.ToolBar>|
  |<xref:System.Windows.Controls.Primitives.Popup>|Standalone|<xref:System.Windows.Controls.ComboBox>、<xref:System.Windows.Controls.ToolBar>、<xref:System.Windows.Controls.Menu>、<xref:System.Windows.Controls.ToolTip>など|
  |<xref:System.Windows.Controls.Primitives.RepeatButton>|Named|<xref:System.Windows.Controls.Slider>、<xref:System.Windows.Controls.Primitives.ScrollBar>など|
  |<xref:System.Windows.Controls.Primitives.ScrollBar>|Named|<xref:System.Windows.Controls.ScrollViewer>|
  |<xref:System.Windows.Controls.ScrollViewer>|Standalone|<xref:System.Windows.Controls.ListBox>、<xref:System.Windows.Controls.ComboBox>、<xref:System.Windows.Controls.Menu>、<xref:System.Windows.Controls.Frame>など|
  |<xref:System.Windows.Controls.Primitives.TabPanel>|Standalone|<xref:System.Windows.Controls.TabControl>|
  |<xref:System.Windows.Controls.TextBox>|Named|<xref:System.Windows.Controls.ComboBox>|
  |<xref:System.Windows.Controls.Primitives.TickBar>|Type-based|<xref:System.Windows.Controls.Slider>|

- **ヘルパー要素の必要なユーザー指定のバインディングまたはプロパティ設定を最小限に抑えます**。 コントロール テンプレート内で正しく機能するために、ヘルパー要素が特定のバインディングまたはプロパティ設定を要求することが一般的です。 ヘルパー要素とテンプレート化されたコントロールがこれらの設定をできる限り多く確立する必要があります。 プロパティの設定やバインディングの確立を行うとき、ユーザーが設定した値をオーバーライドしないように注意してください。 具体的なベスト プラクティスは次のとおりです。

  - Named ヘルパー要素は親によって識別する必要があり、親はこのヘルパー要素に必要な設定を確立する必要があります。

  - Type-based ヘルパー要素は、自身に必要な設定を直接確立する必要があります。 これを行うと、`TemplatedParent` (使用するテンプレートのコントロールの型) などの使用する情報コンテキストをクエリするヘルパー要素が必要になる場合があります。 たとえば、<xref:System.Windows.Controls.ContentPresenter> は、<xref:System.Windows.Controls.ContentControl> 派生型で使用されている場合、その `TemplatedParent` の `Content` プロパティを <xref:System.Windows.Controls.ContentPresenter.Content%2A> プロパティに自動的にバインドします。

  - Standalone ヘルパー要素はこの方法では最適化できません。その理由は、定義上、ヘルパー要素もその親も他方を認識していないためです。

- **Name プロパティを使用してテンプレート内で要素にフラグを設定します**。 プログラムで要素にアクセスするためにそのスタイルで要素を検索する必要があるコントロールは、`Name` プロパティおよび `FindName` パラダイムを使用してこの操作を実行する必要があります。 コントロールは、要素が見つからない場合には例外をスローせず、その要素を必要としていた機能を安全に無効にします。

- **コントロールの状態と動作をスタイルで表現するためのベスト プラクティスを使用します。** コントロールの状態の変更と動作をスタイルで表すためのベスト プラクティスの順序付きリストを次に示します。 シナリオを有効にするリストの最初の項目を使用する必要があります。

  1. プロパティ バインディング 例: <xref:System.Windows.Controls.ComboBox.IsDropDownOpen%2A?displayProperty=nameWithType> と <xref:System.Windows.Controls.Primitives.ToggleButton.IsChecked%2A?displayProperty=nameWithType>間のバインド。

  2. トリガーされたプロパティの変更またはプロパティのアニメーション。 例: <xref:System.Windows.Controls.Button>のホバー状態。

  3. コマンド。 例: <xref:System.Windows.Controls.Primitives.ScrollBar>で  / <xref:System.Windows.Controls.Primitives.ScrollBar.LineDownCommand> を <xref:System.Windows.Controls.Primitives.ScrollBar.LineUpCommand>します。

  4. Standalone ヘルパー要素。 例: <xref:System.Windows.Controls.TabControl>での <xref:System.Windows.Controls.Primitives.TabPanel>。

  5. Type-based ヘルパー型。 例: <xref:System.Windows.Controls.Button>で <xref:System.Windows.Controls.ContentPresenter>、<xref:System.Windows.Controls.Slider>で <xref:System.Windows.Controls.Primitives.TickBar> ます。

  6. Named ヘルパー要素。 例: <xref:System.Windows.Controls.ComboBox>での <xref:System.Windows.Controls.TextBox>。

  7. Named ヘルパー型からのバブル イベント。 スタイル要素からバブル イベントをリッスンする場合、イベントを生成する要素を一意に識別できる必要があります。 例: <xref:System.Windows.Controls.ToolBar>での <xref:System.Windows.Controls.Primitives.Thumb>。

  8. カスタム `OnRender` 動作。 例: <xref:System.Windows.Controls.Button>での <xref:Microsoft.Windows.Themes.ButtonChrome>。

- **(テンプレートのトリガー) ではなくスタイルのトリガーを控えめに使用します**。 テンプレートの要素のプロパティに影響するトリガーは、テンプレートで宣言する必要があります。 コントロールのプロパティに影響するトリガー (`TargetName` 以外) は、テンプレートの変更がトリガーも破棄することがわかっていない限り、スタイルで宣言できます。

- **既存のスタイル パターンとの一貫性を保ちます。** 多くの場合、問題を解決する方法は複数あります。 可能な場合、既存のコントロール スタイル設定パターンとの一貫性を維持してください。 これは、同じ基本データ型 (たとえば、<xref:System.Windows.Controls.ContentControl>、<xref:System.Windows.Controls.ItemsControl>、<xref:System.Windows.Controls.Primitives.RangeBase>など) から派生したコントロールでは特に重要です。

- **プロパティを公開し、テンプレートを再設定せずに一般的なカスタマイズ シナリオを有効にします**。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] はプラグ可能/カスタマイズ可能な部分をサポートしないため、コントロールのユーザーは 2 つのカスタマイズ メソッドのみを使用できます。プロパティを直接設定するか、スタイルを使用してプロパティを設定するかです。 このことを念頭に置いて、このメソッドを使用しない場合にはテンプレートの再設定をしなければならなくなる、非常に一般的で優先度の高いカスタマイズのシナリオを対象とした、数に限りのあるプロパティに使用することが適切です。 カスタマイズのシナリオをいつ有効にし、どのように有効にするかについてのベスト プラクティスを次に示します。

  - 非常に一般的なカスタマイズをコントロールのプロパティとして公開し、テンプレートで使用する必要があります。

  - (まれではないが) あまり一般的ではないカスタマイズは、添付プロパティとして公開し、テンプレートで使用する必要があります。

  - 既知だがまれなカスタマイズでテンプレートの再設定が必要になることは容認されます。

<a name="Theme_Considerations"></a>

## <a name="theme-considerations"></a>テーマの注意事項

- **テーマ スタイルは、すべてのテーマで一貫性のあるプロパティのセマンティクスを持つようにする必要がありますが、その保証はありません**。 ドキュメントの一部として、コントロールは、コントロールのプロパティのセマンティクス、つまり、コントロールのプロパティの「意味」を説明するドキュメントが必要です。 たとえば、<xref:System.Windows.Controls.ComboBox> コントロールは <xref:System.Windows.Controls.ComboBox>内の <xref:System.Windows.Controls.Control.Background%2A> プロパティの意味を定義する必要があります。 コントロールの既定のスタイルは、すべてのテーマでそのドキュメントで定義されたセマンティクスに従おうとする必要があります。 一方コントロールのユーザーは、プロパティのセマンティクスがテーマごとに変わる可能性があることを認識する必要があります。 特定のケースでは、指定したプロパティが特定のテーマで必要な視覚上の制約下では表現できない場合があります。 (たとえば、クラシックのテーマには、多くのコントロールに対して `Thickness` の適用先にできる単一の境界線がありません。)

- **テーマ スタイルは、すべてのテーマで一貫性のあるトリガー セマンティクスを持つ必要はありません**。 トリガーまたはアニメーションを通してコントロール スタイルによって公開されている動作は、テーマごとに異なります。 コントロールのユーザーは、すべてのテーマで特定の動作を実現するために、コントロールが同じメカニズムを必ずしも使用しないことを認識している必要があります。 たとえば、1 つのテーマがアニメーションを使用してホバー動作を表現し、別のテーマがトリガーを使用する場合があります。 これにより、カスタマイズされたコントロールでの動作の保持に不整合が生じる可能性があります。 (たとえば、背景のプロパティの変更は、ホバー状態がトリガーを使用して表現されている場合、コントロールのホバー状態には影響しません。 ただし、ホバー状態がアニメーションを使用して実装されている場合、背景の変更がアニメーションを損なって修復できなくなり、状態遷移が発生する可能性があります。)

- **テーマ スタイルは、すべてのテーマで一貫性のある「レイアウト」セマンティクスを持つ必要はありません**。 たとえば、既定のスタイルは、コントロールがすべてのテーマで同じ量のサイズを占有することを保証する必要はなく、また、コントロールがすべてのテーマで同じコンテンツの余白/パディングを持つことを保証する必要もありません。

## <a name="see-also"></a>関連項目

- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [コントロールの作成の概要](control-authoring-overview.md)
