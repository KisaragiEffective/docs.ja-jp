---
title: TableLayoutPanel を使用したコントロールの配置
ms.date: 03/30/2017
helpviewer_keywords:
- controls [Windows Forms], arranging with TableLayoutPanel
- TableLayoutPanel control [Windows Forms], walkthroughs
- Windows Forms controls, arranging
ms.assetid: d474885e-12cc-4ab7-b997-2a23a643049b
ms.openlocfilehash: 803a56f6416cf3b718890e96cf9f36ae6c4ee471
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76740324"
---
# <a name="walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel"></a>チュートリアル : TableLayoutPanel を使用した Windows フォーム上のコントロールの配置

アプリケーションによっては、フォームのサイズを変更したり、コンテンツのサイズが変化したりしたときに、それに応じて自動的にレイアウトを調整するフォームが必要です。 動的なレイアウトが必要であり、かつコードで <xref:System.Windows.Forms.Control.Layout> イベントを明示的に処理しない場合は、レイアウト パネルの使用をご検討ください。

<xref:System.Windows.Forms.FlowLayoutPanel> コントロールと <xref:System.Windows.Forms.TableLayoutPanel> コントロールを使用すると、コントロールをフォームに直感的な方法で配置できます。 これら 2 つのコントロールは、それぞれに含まれる子コントロールの相対位置を制御するための自動的で構成可能な機能を提供します。また、どちらも実行時に動的なレイアウト機能を提供するため、親フォームの寸法の変更に応じて子コントロールのサイズと位置を変更できます。 レイアウト パネルは他のレイアウト パネルの入れ子にすることができるため、高度なユーザー インターフェイスを実現できます。

<xref:System.Windows.Forms.FlowLayoutPanel> はその内容を特定のフローの方向 (水平または垂直) に配置します。 ある行から次の行、またはある列から次の列に内容をラップすることができます。 また、ラップする代わりにクリップすることもできます。 詳細については、「[チュートリアル: FlowLayoutPanel を使用した Windows フォームでのコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-flowlayoutpanel.md)」を参照してください。

<xref:System.Windows.Forms.TableLayoutPanel> は、HTML \<table > 要素と同様の機能を提供して、コンテンツをグリッドに配置します。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールを使用すると、個々のコントロールの位置を正確に指定する必要なく、グリッドレイアウトにコントロールを配置できます。 セルは行と列に配置され、それぞれに異なるサイズを設定できます。 セルは、行と列の間でマージできます。 セルには、フォームに含めることができるすべてのものを含めることができ、他のほとんどの点でコンテナーとして動作できます。

<xref:System.Windows.Forms.TableLayoutPanel> コントロールは、実行時に比例したサイズ変更機能も提供します。これにより、フォームのサイズ変更に合わせてレイアウトがスムーズに変化するようになります。 これにより、データ入力フォームやローカライズされたアプリケーションなどの目的に <xref:System.Windows.Forms.TableLayoutPanel> 制御が適しています。 詳細については、「[チュートリアル: データ入力用のサイズ変更可能な Windows フォームの作成](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/991eahec(v=vs.100))」および「[チュートリアル: ローカライズ可能な Windows フォームの作成](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/7k9fa71y(v=vs.100))」を参照してください。

一般に、レイアウト全体のコンテナーとして <xref:System.Windows.Forms.TableLayoutPanel> コントロールを使用しないでください。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールを使用して、レイアウトの一部に比例したサイズ変更機能を提供します。

このチュートリアルでは、以下のタスクを行います。

- Windows フォーム プロジェクトの作成

- 行と列のコントロールの配置

- 行と列のプロパティの設定

- コントロールを使用した行と列のスパニング

- オーバーフローの自動処理

- ツールボックスでのダブルクリックによるコントロールの挿入

- アウトラインの描画によるコントロールの挿入

- 別の親コントロールへの既存コントロールの再割り当て

終了すると、これらの重要なレイアウト機能が果たす役割について理解できます。

## <a name="creating-the-project"></a>プロジェクトの作成

最初にプロジェクトを作成し、フォームを設定します。

#### <a name="to-create-the-project"></a>プロジェクトを作成するには

1. "TableLayoutPanelExample" という名前の Windows アプリケーションプロジェクトを作成します。 詳細については、「[方法: Windows フォームアプリケーションプロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)する」を参照してください。

2. **Windows** **フォームデザイナー**でフォームを選択します。

## <a name="arranging-controls-in-rows-and-columns"></a>行と列のコントロールの配置

<xref:System.Windows.Forms.TableLayoutPanel> コントロールを使用すると、コントロールを簡単に行と列に配置できます。

#### <a name="to-arrange-controls-in-rows-and-columns-using-a-tablelayoutpanel"></a>TableLayoutPanel を使用して行と列のコントロールを配置するには

1. <xref:System.Windows.Forms.TableLayoutPanel> ツールボックス **から** コントロールをフォームにドラッグします。 既定では、<xref:System.Windows.Forms.TableLayoutPanel> コントロールには4つのセルがあることに注意してください。

2. **[ツールボックス]** から [<xref:System.Windows.Forms.Button>] コントロールを <xref:System.Windows.Forms.TableLayoutPanel> コントロールにドラッグして、いずれかのセルにドロップします。 <xref:System.Windows.Forms.Button> コントロールが、選択したセル内に作成されることに注意してください。

3. **[ツールボックス]** から3つの <xref:System.Windows.Forms.Button> コントロールを <xref:System.Windows.Forms.TableLayoutPanel> コントロールにドラッグして、各セルにボタンが含まれるようにします。

4. 2つの列の間の垂直方向のサイズ変更ハンドルを取得し、それを左に移動します。 最初の列の <xref:System.Windows.Forms.Button> コントロールのサイズがより小さい幅に変更され、2番目の列の <xref:System.Windows.Forms.Button> コントロールのサイズは変更されないことに注意してください。

5. 2つの列の間の垂直方向のサイズ変更ハンドルを取得し、右に移動します。 最初の列の <xref:System.Windows.Forms.Button> コントロールが元のサイズに戻り、2番目の列の <xref:System.Windows.Forms.Button> コントロールが右に移動することに注意してください。

6. 水平方向のサイズ変更ハンドルを上下に移動して、パネル内のコントロールに対する効果を確認します。

## <a name="positioning-controls-within-cells-using-docking-and-anchoring"></a>ドッキングと固定を使用したセル内でのコントロールの配置

<xref:System.Windows.Forms.TableLayoutPanel> 内の子コントロールの固定動作は、他のコンテナーコントロールの動作とは異なります。 子コントロールのドッキング動作は、他のコンテナーコントロールと同じです。

#### <a name="positioning-controls-within-cells"></a>セル内でのコントロールの配置

1. 最初の <xref:System.Windows.Forms.Button> コントロールを選択します。 <xref:System.Windows.Forms.Control.Dock%2A> プロパティの値を <xref:System.Windows.Forms.DockStyle.Fill>に変更します。 <xref:System.Windows.Forms.Button> コントロールが、セルに合わせて拡大されることに注意してください。

2. 他の <xref:System.Windows.Forms.Button> コントロールのいずれかを選択します。 <xref:System.Windows.Forms.Control.Anchor%2A> プロパティの値を <xref:System.Windows.Forms.AnchorStyles.Right>に変更します。 右側の境界線がセルの右境界の近くに移動されることに注意してください。 境界線間の距離は、<xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Margin%2A> プロパティとパネルの <xref:System.Windows.Forms.Control.Padding%2A> プロパティの合計です。

3. <xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Anchor%2A> プロパティの値を <xref:System.Windows.Forms.AnchorStyles.Right> および <xref:System.Windows.Forms.AnchorStyles.Left>に変更します。 コントロールのサイズはセルの幅に合わせて調整され、<xref:System.Windows.Forms.Control.Margin%2A> と <xref:System.Windows.Forms.Control.Padding%2A> 値が考慮されます。

4. <xref:System.Windows.Forms.AnchorStyles.Top> と <xref:System.Windows.Forms.AnchorStyles.Bottom> のスタイルを使用して、手順 2. と 3. を繰り返します。

## <a name="setting-row-and-column-properties"></a>行と列のプロパティの設定

<xref:System.Windows.Forms.TableLayoutPanel.RowStyles%2A> コレクションと <xref:System.Windows.Forms.TableLayoutPanel.ColumnStyles%2A> コレクションを使用すると、行と列の個々のプロパティを設定できます。

#### <a name="to-set-row-and-column-properties"></a>行と列のプロパティを設定するには

1. **Windows フォームデザイナー**で <xref:System.Windows.Forms.TableLayoutPanel> コントロールを選択します。

2. **[プロパティ]** ウィンドウで、 **[列]** エントリの横にある省略記号![(省略記号ボタン ([...]](./media/visual-studio-ellipsis-button.png)プロパティウィンドウ) をクリックして <xref:System.Windows.Forms.TableLayoutPanel.ColumnStyles%2A> コレクションを開きます。

3. 最初の列を選択し、<xref:System.Windows.Forms.TableLayoutStyle.SizeType%2A> プロパティの値を <xref:System.Windows.Forms.SizeType.AutoSize>に変更します。 **[OK]** をクリックして変更を確定します。 最初の列の幅が <xref:System.Windows.Forms.Button> コントロールに合うように縮小されることに注意してください。 列の幅のサイズは変更できないことにも注意してください。

4. **[プロパティ]** ウィンドウで <xref:System.Windows.Forms.TableLayoutPanel.ColumnStyles%2A> コレクションを開き、最初の列を選択します。 <xref:System.Windows.Forms.TableLayoutStyle.SizeType%2A> プロパティの値を <xref:System.Windows.Forms.SizeType.Percent>に変更します。 **[OK]** をクリックして変更を確定します。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールの幅を大きくして、最初の列の幅が拡大することを確認します。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールの幅を小さくして、セルに合わせて最初の列のボタンのサイズを変更します。 また、列の幅が変更可能であることにも注意してください。

5. **[プロパティ]** ウィンドウで <xref:System.Windows.Forms.TableLayoutPanel.ColumnStyles%2A> コレクションを開き、表示されているすべての列を選択します。 すべての <xref:System.Windows.Forms.TableLayoutStyle.SizeType%2A> プロパティの値を <xref:System.Windows.Forms.SizeType.Percent>に設定します。 **[OK]** をクリックして変更を確定します。 <xref:System.Windows.Forms.TableLayoutPanel.RowStyles%2A> コレクションを使用して繰り返します。

6. コーナーのサイズを変更するハンドルをつかんで、<xref:System.Windows.Forms.TableLayoutPanel> コントロールの幅と高さの両方を調整します。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールのサイズが変更されたときに、行と列のサイズが変更されることに注意してください。 また、行と列は、水平方向と垂直方向のサイズ変更ハンドルでサイズ変更できることにも注意してください。

## <a name="spanning-rows-and-columns-with-a-control"></a>コントロールを使用した行と列のスパニング

<xref:System.Windows.Forms.TableLayoutPanel> コントロールは、デザイン時にコントロールに新しいプロパティをいくつか追加します。 これらのプロパティのうち2つは `RowSpan` と `ColumnSpan`です。 これらのプロパティを使用して、1つのコントロールが複数の行または列にまたがって表示されるようにすることができます。

#### <a name="to-span-rows-and-columns-with-a-control"></a>コントロールを使用して行と列をスパンするには

1. 最初の行と最初の列の <xref:System.Windows.Forms.Button> コントロールを選択します。

2. **[プロパティ]** ウィンドウで、[`ColumnSpan`] プロパティの値を**2**に変更します。 <xref:System.Windows.Forms.Button> コントロールは、最初の列と2番目の列に入力することに注意してください。 また、この変更に対応するために余分な行が追加されていることにも注意してください。

3. `RowSpan` プロパティについて、手順 2. を繰り返します。

## <a name="inserting-controls-by-double-clicking-them-in-the-toolbox"></a>ツールボックスでのダブルクリックによるコントロールの挿入

<xref:System.Windows.Forms.TableLayoutPanel> ツールボックス **でコントロールをダブルクリックすると、** コントロールに内容を挿入できます。

#### <a name="to-insert-controls-by-double-clicking-in-the-toolbox"></a>ツールボックスでダブルクリックしてコントロールを挿入するには

1. <xref:System.Windows.Forms.TableLayoutPanel> ツールボックス **から** コントロールをフォームにドラッグします。

2. <xref:System.Windows.Forms.Button> ツールボックス **の**コントロール アイコンをダブルクリックします。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールの最初のセルに新しいボタンコントロールが表示されることに注意してください。

3. **ツールボックス**でさらにいくつかのコントロールをダブルクリックします。 新しいコントロールは、<xref:System.Windows.Forms.TableLayoutPanel> コントロールの未使用のセルに連続して表示されることに注意してください。 また、開いているセルがない場合は、新しいコントロールに合わせて <xref:System.Windows.Forms.TableLayoutPanel> コントロールが拡張されることにも注意してください。

## <a name="automatic-handling-of-overflows"></a>オーバーフローの自動処理

<xref:System.Windows.Forms.TableLayoutPanel> コントロールにコントロールを挿入するときに、新しいコントロールの空のセルが不足する場合があります。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールは、セルの数を増やすことによって、この状況を自動的に処理します。

#### <a name="to-observe-automatic-handling-of-overflows"></a>オーバーフローの自動処理を監視するには

1. <xref:System.Windows.Forms.TableLayoutPanel> コントロールに空のセルが残っている場合は、<xref:System.Windows.Forms.TableLayoutPanel> コントロールがいっぱいになるまで、新しい <xref:System.Windows.Forms.Button> コントロールを挿入し続けます。

2. <xref:System.Windows.Forms.TableLayoutPanel> コントロールがいっぱいになったら、 **[ツールボックス]** の <xref:System.Windows.Forms.Button> アイコンをダブルクリックして、別の <xref:System.Windows.Forms.Button> コントロールを挿入します。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールは、新しいコントロールを格納するために新しいセルを作成することに注意してください。 さらにいくつかのコントロールを挿入し、サイズ変更動作を観察します。

3. <xref:System.Windows.Forms.TableLayoutPanel> コントロールの <xref:System.Windows.Forms.TableLayoutPanel.GrowStyle%2A> プロパティの値を <xref:System.Windows.Forms.TableLayoutPanelGrowStyle.FixedSize>に変更します。 **[ツールボックス]** の <xref:System.Windows.Forms.Button> アイコンをダブルクリックして、<xref:System.Windows.Forms.TableLayoutPanel> コントロールがいっぱいになるまで <xref:System.Windows.Forms.Button> コントロールを挿入します。 **ツールボックス**の [<xref:System.Windows.Forms.Button>] アイコンをもう一度ダブルクリックします。 追加の行と列を作成できないことを通知する**Windows フォームデザイナー**からのエラーメッセージが表示されます。

## <a name="inserting-a-control-by-drawing-its-outline"></a>アウトラインの描画によるコントロールの挿入

セルにアウトラインを描画すると、コントロールを <xref:System.Windows.Forms.TableLayoutPanel> コントロールに挿入し、サイズを指定できます。

#### <a name="to-insert-a-control-by-drawing-its-outline"></a>アウトラインを描画してコントロールを挿入するには

1. <xref:System.Windows.Forms.TableLayoutPanel> ツールボックス **から** コントロールをフォームにドラッグします。

2. **[ツールボックス]** で <xref:System.Windows.Forms.Button> コントロール アイコンをクリックします。 フォームにドラッグしないでください。

3. <xref:System.Windows.Forms.TableLayoutPanel> コントロールにマウス ポインターを置きます。 ポインターが <xref:System.Windows.Forms.Button> コントロール アイコンが付いた十字カーソルに変わることにご注意ください。

4. マウス ボタンを押したままにします。

5. マウス ポインターをドラッグして、 <xref:System.Windows.Forms.Button> コントロールのアウトラインを描画します。 適切なサイズのアウトラインを描画したら、マウス ボタンを離します。 コントロールのアウトラインを描画したセルには、<xref:System.Windows.Forms.Button> コントロールが作成されることに注意してください。

## <a name="multiple-controls-within-cells-are-not-permitted"></a>セル内の複数のコントロールが許可されていません

<xref:System.Windows.Forms.TableLayoutPanel> コントロールには、セルごとに1つの子コントロールのみを含めることができます。

#### <a name="to-demonstrate-that-multiple-controls-within-cells-are-not-permitted"></a>セル内の複数のコントロールが許可されていないことを示すには

- <xref:System.Windows.Forms.Button> コントロールを **[ツールボックス]** から [<xref:System.Windows.Forms.TableLayoutPanel>] コントロールにドラッグして、いずれかの占有セルにドロップします。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールでは、<xref:System.Windows.Forms.Button> コントロールを占有セルにドロップすることはできないことに注意してください。

## <a name="swapping-controls"></a>コントロールの交換

<xref:System.Windows.Forms.TableLayoutPanel> コントロールを使用すると、2つの異なるセルを占有するコントロールを入れ替えることができます。

#### <a name="to-swap-controls"></a>コントロールをスワップするには

- 占有されているセルから <xref:System.Windows.Forms.Button> コントロールの1つをドラッグし、別の占有セルにドロップします。 2つのコントロールが1つのセルから別のセルに移動されることに注意してください。

## <a name="next-steps"></a>次の手順

レイアウト パネルとコントロールを組み合わせて使用すると、複雑なレイアウトを作成できます。 さらに理解を深めるには、次の操作を行うことをお勧めします。

- <xref:System.Windows.Forms.Button> コントロールのいずれかを大きなサイズに変更してみて、レイアウトへの影響を確認してください。

- <xref:System.Windows.Forms.TableLayoutPanel> コントロールに複数のコントロールを選択して貼り付け、コントロールを挿入する方法を確認します。

- レイアウト パネルには、別のレイアウト パネルを含めることができます。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールを既存のコントロールにドロップしてみます。

- <xref:System.Windows.Forms.TableLayoutPanel> コントロールを親フォームにドッキングします。 フォームのサイズを変更し、レイアウトの変化を確認します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.FlowLayoutPanel>
- <xref:System.Windows.Forms.TableLayoutPanel>
- [チュートリアル: FlowLayoutPanel を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-flowlayoutpanel.md)
- [チュートリアル: スナップ線を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-snaplines.md)
- [チュートリアル: データ入力用のサイズ変更可能な Windows フォームの作成](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/991eahec(v=vs.100))
- [チュートリアル: ローカライズ可能な Windows フォームの作成](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/7k9fa71y(v=vs.100))
- [TableLayoutPanel コントロールの推奨される手順](best-practices-for-the-tablelayoutpanel-control.md)
- [AutoSize プロパティの概要](autosize-property-overview.md)
- [方法: Windows フォーム上のコントロールをドッキングする](how-to-dock-controls-on-windows-forms.md)
- [方法: Windows フォームにコントロールを固定する](how-to-anchor-controls-on-windows-forms.md)
- [チュートリアル: Padding、Margin、および AutoSize プロパティを使用した Windows フォーム コントロールのレイアウト](windows-forms-controls-padding-autosize.md)
