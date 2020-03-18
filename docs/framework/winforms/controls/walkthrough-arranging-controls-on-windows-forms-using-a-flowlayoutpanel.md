---
title: FlowLayoutPanel を使用してコントロールを整列する
ms.date: 03/30/2017
helpviewer_keywords:
- FlowLayoutPanel control [Windows Forms], walkthroughs
- Windows Forms controls, arranging
- controls [Windows Forms], arranging with FlowLayoutPanel
- layout [Windows Forms], walkthroughs
ms.assetid: a1744323-0316-49c2-992e-ebfc0a976b85
ms.openlocfilehash: 6df0a910ee346f319fbee835e5e632808630a99e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745405"
---
# <a name="walkthrough-arranging-controls-on-windows-forms-using-a-flowlayoutpanel"></a>チュートリアル : FlowLayoutPanel を使用した Windows フォーム上のコントロールの配置

アプリケーションによっては、フォームのサイズを変更したり、コンテンツのサイズが変化したりしたときに、それに応じて自動的にレイアウトを調整するフォームが必要です。 動的なレイアウトが必要であり、かつコードで <xref:System.Windows.Forms.Control.Layout> イベントを明示的に処理しない場合は、レイアウト パネルの使用をご検討ください。

<xref:System.Windows.Forms.FlowLayoutPanel> コントロールと <xref:System.Windows.Forms.TableLayoutPanel> コントロールを使用すると、コントロールをフォームに直感的な方法で配置できます。 これら 2 つのコントロールは、それぞれに含まれる子コントロールの相対位置を制御するための自動的で構成可能な機能を提供します。また、どちらも実行時に動的なレイアウト機能を提供するため、親フォームの寸法の変更に応じて子コントロールのサイズと位置を変更できます。 レイアウト パネルは他のレイアウト パネルの入れ子にすることができるため、高度なユーザー インターフェイスを実現できます。

<xref:System.Windows.Forms.TableLayoutPanel> は、HTML \<table > 要素と同様の機能を提供して、コンテンツをグリッドに配置します。 セルは行と列に配置され、それぞれに異なるサイズを設定できます。 詳細については、「 [Walkthrough: Arranging Controls on Windows Forms Using a TableLayoutPanel](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md)」を参照してください。

<xref:System.Windows.Forms.FlowLayoutPanel> はその内容を特定のフローの方向 (水平または垂直) に配置します。 ある行から次の行、またはある列から次の列に内容をラップすることができます。 また、ラップする代わりにクリップすることもできます。 このチュートリアルでは、以下のタスクを行います。

- Windows フォーム プロジェクトの作成

- コントロールの水平配置と垂直配置

- フロー方向の変更

- フロー中断の挿入

- パディングと余白を使用したコントロールの配置

- ツールボックスでのダブルクリックによるコントロールの挿入

- アウトラインの描画によるコントロールの挿入

- キャレットを使用したコントロールの挿入

- 別の親コントロールへの既存コントロールの再割り当て

完了すると、これらの重要なレイアウト機能によって果たす役割について理解できるようになります。

## <a name="create-the-project"></a>プロジェクトを作成する

1. Visual Studio で、"flowlayoutを example" という名前の Windows ベースのアプリケーションプロジェクトを作成します (**ファイル** > **新しい** > **プロジェクト** >  **C# Visual**または**Visual Basic** > **クラシックデスクトップ** > **Windows フォームアプリケーション**)。

2. **フォーム デザイナー**でフォームを選びます。

## <a name="arranging-controls-horizontally-and-vertically"></a>コントロールの水平配置と垂直配置
 <xref:System.Windows.Forms.FlowLayoutPanel> コントロールでは、コントロールを行や列に沿って配置でき、各コントロールの位置を正確に指定する必要がありません。

 <xref:System.Windows.Forms.FlowLayoutPanel> コントロールでは、親フォームの寸法の変更に応じて、子コントロールのサイズやフローを変更できます。

### <a name="to-arrange-controls-horizontally-and-vertically-using-a-flowlayoutpanel"></a>FlowLayoutPanel を使用してコントロールを水平方向または垂直方向に配置するには

1. <xref:System.Windows.Forms.FlowLayoutPanel> ツールボックス **から** コントロールをフォームにドラッグします。

2. <xref:System.Windows.Forms.Button> ツールボックス **から** コントロールを <xref:System.Windows.Forms.FlowLayoutPanel>にドラッグします。 このコントロールが <xref:System.Windows.Forms.FlowLayoutPanel> コントロールの左上隅に自動的に移動されることにご注意ください。

3. <xref:System.Windows.Forms.Button> ツールボックス **から別の** コントロールを <xref:System.Windows.Forms.FlowLayoutPanel>にドラッグします。 <xref:System.Windows.Forms.Button> コントロールが、最初の <xref:System.Windows.Forms.Button> コントロールの横の位置に自動的に移動することにご注意ください。 <xref:System.Windows.Forms.FlowLayoutPanel> が狭すぎて、2 つのコントロールが同じ行に入りきらない場合、新しい <xref:System.Windows.Forms.Button> コントロールは次の行に自動的に移動します。

4. <xref:System.Windows.Forms.Button> ツールボックス **からさらにいくつかの** コントロールを <xref:System.Windows.Forms.FlowLayoutPanel>にドラッグします。 次の行に折り返すまで、 <xref:System.Windows.Forms.Button> コントロールを続けて配置します。

5. <xref:System.Windows.Forms.FlowLayoutPanel> コントロールの <xref:System.Windows.Forms.FlowLayoutPanel.WrapContents%2A> プロパティの値を `false`に変更します。 子コントロールが次の行にフローしなくなることにご注意ください。 代わりに最初の行に移動してクリップされます。

6. <xref:System.Windows.Forms.FlowLayoutPanel> コントロールの <xref:System.Windows.Forms.FlowLayoutPanel.WrapContents%2A> プロパティの値を `true`に変更します。 子コントロールがまた次の行に折り返されるようになることにご注意ください。

7. すべての <xref:System.Windows.Forms.FlowLayoutPanel> コントロールが最初の列に移動するまで、 <xref:System.Windows.Forms.Button> コントロールの幅を縮小します。

8. すべての <xref:System.Windows.Forms.FlowLayoutPanel> コントロールが最初の行に移動するまで、 <xref:System.Windows.Forms.Button> コントロールの幅を拡大します。 幅の拡大に対応できるように、フォームのサイズ変更が必要になる場合があります。

## <a name="changing-flow-direction"></a>フロー方向の変更
 <xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A> プロパティを使用すると、コントロールの配置方向を変更できます。 子コントロールは左から右、右から左、上から下、または下から上に配置できます。

### <a name="to-change-the-flow-direction-in-a-flowlayoutpanel"></a>FlowLayoutPanel のフロー方向を変更するには

1. <xref:System.Windows.Forms.FlowLayoutPanel> コントロールの <xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A> プロパティの値を <xref:System.Windows.Forms.FlowDirection.TopDown>に変更します。 コントロールの高さに応じて、子コントロールが 1 つ以上の列に再配置されることにご注意ください。

2. <xref:System.Windows.Forms.FlowLayoutPanel> のサイズを変更し、その高さを <xref:System.Windows.Forms.Button> コントロールの列よりも小さくします。 <xref:System.Windows.Forms.FlowLayoutPanel> が、子コントロールを次の列にフローするように再配置することにご注意ください。 引き続き高さを小さくしていくと、子コントロールが次の列にフローしていくことにご注意ください。 <xref:System.Windows.Forms.FlowLayoutPanel> コントロールの <xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A> プロパティの値を <xref:System.Windows.Forms.FlowDirection.RightToLeft>に変更します。 子コントロールの位置が反転することにご注意ください。 <xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A> プロパティの値を <xref:System.Windows.Forms.FlowDirection.BottomUp>に変更したときのレイアウトをご確認ください。

## <a name="inserting-flow-breaks"></a>フロー中断の挿入
 <xref:System.Windows.Forms.FlowLayoutPanel> コントロールでは、その子コントロールに FlowBreak プロパティを指定できます。 FlowBreak プロパティの値を `true` に設定することで、 <xref:System.Windows.Forms.FlowLayoutPanel> コントロールを現在のフロー方向のコントロールにレイアウトすること、および次の行または列にラップすることを停止します。

### <a name="to-insert-flow-breaks"></a>フロー中断を挿入するには

1. <xref:System.Windows.Forms.FlowLayoutPanel> コントロールの <xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A> プロパティの値を <xref:System.Windows.Forms.FlowDirection.TopDown>に変更します。

2. 左端の列の中ほどにある、 <xref:System.Windows.Forms.Button> コントロールの 1 つを選びます。

3. <xref:System.Windows.Forms.Button> コントロールの FlowBreak プロパティの値を `true`に設定します。 列が区切られ、選んだ <xref:System.Windows.Forms.Button> コントロールよりも後のコントロールが次の列にフローすることにご注意ください。 <xref:System.Windows.Forms.Button> コントロールの FlowBreak プロパティの値を `false` に設定すると、元の動作に戻ります。

## <a name="positioning-controls-using-docking-and-anchoring"></a>ドッキングと固定を使用したコントロールの配置
 子コントロールのドッキング動作と固定動作は、他のコンテナー コントロールの動作と異なります。 ドッキングも固定も、フロー方向で最も大きいコントロールを基準として機能します。

### <a name="to-position-controls-using-docking-and-anchoring"></a>ドッキングと固定を使用してコントロールを配置するには

1. <xref:System.Windows.Forms.FlowLayoutPanel> コントロールがすべて 1 列に配置されるまで、 <xref:System.Windows.Forms.Button> のサイズを拡大します。

2. 1 番上にある <xref:System.Windows.Forms.Button> コントロールを選びます。 幅を拡大して、他の <xref:System.Windows.Forms.Button> コントロールの約 2 倍の幅にします。

3. 2 番目の <xref:System.Windows.Forms.Button> コントロールを選びます。 <xref:System.Windows.Forms.Control.Anchor%2A> プロパティの値を <xref:System.Windows.Forms.AnchorStyles.Right>に変更します。 選択したコントロールが移動し、その右側の境界線が 1 番目の <xref:System.Windows.Forms.Button> コントロールの右側の境界線と揃うことにご注意ください。

4. <xref:System.Windows.Forms.Control.Anchor%2A> プロパティの値を <xref:System.Windows.Forms.AnchorStyles.Right> および <xref:System.Windows.Forms.AnchorStyles.Left>に変更します。 サイズが変更され、1 番目の <xref:System.Windows.Forms.Button> コントロールと同じ幅になることにご注意ください。

5. 3 番目の <xref:System.Windows.Forms.Button> コントロールを選びます。 <xref:System.Windows.Forms.Control.Dock%2A> プロパティの値を <xref:System.Windows.Forms.DockStyle.Fill>に変更します。 サイズが変更され、1 番目の <xref:System.Windows.Forms.Button> コントロールと同じ幅になることにご注意ください。

## <a name="arranging-controls-using-padding-and-margins"></a>パディングと余白を使用したコントロールの配置
 <xref:System.Windows.Forms.FlowLayoutPanel> コントロールには、 <xref:System.Windows.Forms.Control.Padding%2A> と <xref:System.Windows.Forms.Control.Margin%2A> のプロパティを変更してコントロールを配置することもできます。

 <xref:System.Windows.Forms.Control.Padding%2A> プロパティを使用すると、 <xref:System.Windows.Forms.FlowLayoutPanel> コントロールのセル内でコントロールの配置を制御できます。 このプロパティは、子コントロールと <xref:System.Windows.Forms.FlowLayoutPanel> コントロールの境界との間隔を指定します。

 <xref:System.Windows.Forms.Control.Margin%2A> プロパティでは、コントロール間の間隔を制御できます。

### <a name="to-arrange-controls-using-the-padding-and-margin-properties"></a>Padding プロパティと Margin プロパティを使用してコントロールを配置するには

1. <xref:System.Windows.Forms.FlowLayoutPanel> コントロールの <xref:System.Windows.Forms.Control.Dock%2A> プロパティの値を <xref:System.Windows.Forms.DockStyle.Fill>に変更します。 フォームの大きさが十分にある場合、 <xref:System.Windows.Forms.Button> コントロールは <xref:System.Windows.Forms.FlowLayoutPanel> コントロールの最初の列に移動します。

2. <xref:System.Windows.Forms.FlowLayoutPanel> コントロールの <xref:System.Windows.Forms.Control.Padding%2A> プロパティの値を変更します。これを行うには、 <xref:System.Windows.Forms.Control.Padding%2A> [プロパティ] **ウィンドウで** エントリを展開し、 <xref:System.Windows.Forms.Padding.All%2A> プロパティを **20**に設定します。 詳細については、「[チュートリアル: 埋め込み、余白、AutoSize プロパティを使用した Windows フォームコントロールのレイアウト](windows-forms-controls-padding-autosize.md)」を参照してください。 子コントロールが <xref:System.Windows.Forms.FlowLayoutPanel> コントロールの中央に移動することにご注意ください。 <xref:System.Windows.Forms.Control.Padding%2A> プロパティの値を大きくすると、子コントロールが <xref:System.Windows.Forms.FlowLayoutPanel> コントロールの境界から離れます。

3. <xref:System.Windows.Forms.Button> 内の <xref:System.Windows.Forms.FlowLayoutPanel> コントロールをすべて選び、 <xref:System.Windows.Forms.Control.Margin%2A> プロパティの値を **20**に設定します。 <xref:System.Windows.Forms.Button> コントロール間の間隔が広がり、各コントロールが離れることにご注意ください。 すべての子コントロールを表示するには、 <xref:System.Windows.Forms.FlowLayoutPanel> コントロールのサイズを拡大する必要がある場合があります。

## <a name="inserting-controls-by-double-clicking-them-in-the-toolbox"></a>ツールボックスでのダブルクリックによるコントロールの挿入
 <xref:System.Windows.Forms.FlowLayoutPanel> ツールボックス **でコントロールをダブルクリックすると、** コントロールに内容を挿入できます。

### <a name="to-insert-controls-by-double-clicking-in-the-toolbox"></a>ツールボックスでダブルクリックしてコントロールを挿入するには

1. <xref:System.Windows.Forms.Button> ツールボックス **の**コントロール アイコンをダブルクリックします。 新しい <xref:System.Windows.Forms.Button> コントロールが <xref:System.Windows.Forms.FlowLayoutPanel> コントロールに表示されることにご注意ください。

2. **ツールボックス**でさらにいくつかのコントロールをダブルクリックします。 新しいコントロールが <xref:System.Windows.Forms.FlowLayoutPanel> コントロールに順次表示されることにご注意ください。

## <a name="inserting-a-control-by-drawing-its-outline"></a>アウトラインの描画によるコントロールの挿入
 セルにアウトラインを描画すると、コントロールを <xref:System.Windows.Forms.FlowLayoutPanel> コントロールに挿入し、サイズを指定できます。

### <a name="to-insert-a-control-by-drawing-its-outline"></a>アウトラインを描画してコントロールを挿入するには

1. **[ツールボックス]** で <xref:System.Windows.Forms.Button> コントロール アイコンをクリックします。 フォームにドラッグしないでください。

2. <xref:System.Windows.Forms.FlowLayoutPanel> コントロールにマウス ポインターを置きます。 ポインターが <xref:System.Windows.Forms.Button> コントロール アイコンが付いた十字カーソルに変わることにご注意ください。

3. マウス ボタンを押したままにします。

4. マウス ポインターをドラッグして、 <xref:System.Windows.Forms.Button> コントロールのアウトラインを描画します。 適切なサイズのアウトラインを描画したら、マウス ボタンを離します。 <xref:System.Windows.Forms.Button> コントロール上の次の空き位置に <xref:System.Windows.Forms.FlowLayoutPanel> コントロールが作成されることにご注意ください。

## <a name="inserting-controls-using-the-insertion-bar"></a>挿入バーを使用したコントロールの挿入
 <xref:System.Windows.Forms.FlowLayoutPanel> コントロールの特定の位置にコントロールを挿入できます。 <xref:System.Windows.Forms.FlowLayoutPanel> コントロールのクライアント領域にコントロールをドラッグすると、コントロールの挿入位置を示す挿入バーが表示されます。

### <a name="to-insert-a-control-using-the-caret"></a>キャレットを使用してコントロールを挿入するには

1. <xref:System.Windows.Forms.Button> ツールボックス **から** コントロールを <xref:System.Windows.Forms.FlowLayoutPanel> コントロールにドラッグし、2 つの <xref:System.Windows.Forms.Button> コントロールの間の領域をポイントします。 挿入バーが描画され、<xref:System.Windows.Forms.FlowLayoutPanel> コントロールにドロップされたときに <xref:System.Windows.Forms.Button> が配置される場所を示していることに注意してください。 新しい <xref:System.Windows.Forms.Button> コントロールを <xref:System.Windows.Forms.FlowLayoutPanel> コントロールにドロップする前に、マウス ポインターを移動して、挿入バーがどのように移動するかを確認します。

2. 新しい <xref:System.Windows.Forms.Button> コントロールを <xref:System.Windows.Forms.FlowLayoutPanel> コントロールにドロップします。 新しい <xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Margin%2A> プロパティの値が異なるため、他のコントロールとは位置が揃わないことにご注意ください。

## <a name="reassigning-existing-controls-to-a-different-parent"></a>別の親コントロールへの既存コントロールの再割り当て
 フォームに存在するコントロールを新しい <xref:System.Windows.Forms.FlowLayoutPanel> コントロールに割り当てることができます。

### <a name="to-reparent-existing-controls"></a>既存のコントロールの親を変更するには

1. <xref:System.Windows.Forms.Button> ツールボックス **から 3 つの** コントロールをフォームにドラッグします。 これらを互いに近づけて配置しますが、整列はさせません。

2. **[ツールボックス]** で <xref:System.Windows.Forms.FlowLayoutPanel> コントロール アイコンをクリックします。 フォームにドラッグしないでください。

3. マウス ポインターを 3 つの <xref:System.Windows.Forms.Button> コントロールに近づけます。 ポインターが <xref:System.Windows.Forms.FlowLayoutPanel> コントロール アイコンが付いた十字カーソルに変わることにご注意ください。

4. マウス ボタンを押したままにします。

5. マウス ポインターをドラッグして、 <xref:System.Windows.Forms.FlowLayoutPanel> コントロールのアウトラインを描画します。 3 つの <xref:System.Windows.Forms.Button> コントロールを囲むようにアウトラインを描画します。

6. マウスのボタンを離します。 3 つの <xref:System.Windows.Forms.Button> コントロールが <xref:System.Windows.Forms.FlowLayoutPanel> コントロールに挿入されることにご注意ください。

## <a name="next-steps"></a>次の手順
 レイアウト パネルとコントロールを組み合わせて使用すると、複雑なレイアウトを作成できます。 さらに理解を深めるには、次の操作を行うことをお勧めします。

- いずれかの <xref:System.Windows.Forms.Button> コントロールのサイズを拡大して、レイアウトの変化を確認します。

- レイアウト パネルには、別のレイアウト パネルを含めることができます。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールを既存のコントロールにドロップしてみます。

- <xref:System.Windows.Forms.FlowLayoutPanel> コントロールを親フォームにドッキングします。 フォームのサイズを変更し、レイアウトの変化を確認します。

- いずれかのコントロールの <xref:System.Windows.Forms.Control.Visible%2A> プロパティを `false` に設定し、これに呼応して <xref:System.Windows.Forms.FlowLayoutPanel> のフローがどのように変化するか確認します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.FlowLayoutPanel>
- <xref:System.Windows.Forms.TableLayoutPanel>
- [チュートリアル: TableLayoutPanel を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md)
- [チュートリアル: スナップ線を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-snaplines.md)
- [AutoSize プロパティの概要](autosize-property-overview.md)
- [方法: Windows フォーム上のコントロールをドッキングする](how-to-dock-controls-on-windows-forms.md)
- [方法: Windows フォームにコントロールを固定する](how-to-anchor-controls-on-windows-forms.md)
- [チュートリアル: Padding、Margin、および AutoSize プロパティを使用した Windows フォーム コントロールのレイアウト](windows-forms-controls-padding-autosize.md)
