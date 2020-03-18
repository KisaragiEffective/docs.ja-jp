---
title: 余白、余白、および AutoSize プロパティを使用してコントロールをレイアウトします
ms.date: 03/30/2017
f1_keywords:
- Margin.Bottom
- Margin.Left
- Margin.Top
- Margin.All
- Margin.Right
helpviewer_keywords:
- Margin property [Windows Forms], walkthroughs
- walkthroughs [Windows Forms], layout
- AutoSize property [Windows Forms], walkthroughs
- Padding property [Windows Forms], walkthroughs
- layout [Windows Forms], margins and padding
- Windows Forms, layout
ms.assetid: f8ae2a6b-db13-4630-8e25-d104091205c7
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: ca7942c04434592f2541252c47ac3dd17e03dbac
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742376"
---
# <a name="walkthrough-lay-out-controls-with-padding-margins-and-the-autosize-property"></a>チュートリアル: padding、margin、および AutoSize プロパティを使用してコントロールをレイアウトする

フォーム上のコントロールを正確に配置することは、多くのアプリケーションで優先度の高い作業です。 Visual Studio の**Windows フォームデザイナー**には、これを実現するためのさまざまなレイアウトツールが用意されています。 最も重要なのは、すべての Windows フォームコントロールに存在する <xref:System.Windows.Forms.Control.Margin%2A>、<xref:System.Windows.Forms.Control.Padding%2A>、および <xref:System.Windows.Forms.Control.AutoSize%2A> プロパティです。

<xref:System.Windows.Forms.Control.Margin%2A> プロパティは、その他のコントロールで、コントロールの枠線からの指定された距離を保持するコントロールの周囲のスペースを定義します。

<xref:System.Windows.Forms.Control.Padding%2A> プロパティは、コントロールの内容 (<xref:System.Windows.Forms.Control.Text%2A> プロパティの値など) で、コントロールの枠線からの指定した距離を保持するコントロールの内部のスペースを定義します。

次の図は、コントロールの <xref:System.Windows.Forms.Control.Padding%2A> プロパティと <xref:System.Windows.Forms.Control.Margin%2A> プロパティを示しています。

![Windows フォーム コントロールのパディングとマージン](./media/vs-winformpadmargin.gif)

<xref:System.Windows.Forms.Control.AutoSize%2A> プロパティは、コントロールの内容に合わせて自動的にサイズを変更するようにコントロールに指示します。 元の <xref:System.Windows.Forms.Control.Size%2A> プロパティの値よりも小さいサイズになることはなく、<xref:System.Windows.Forms.Control.Padding%2A> プロパティの値が考慮されます。

## <a name="prerequisites"></a>前提条件

このチュートリアルを完了するには、Visual Studio が必要です。

## <a name="create-the-project"></a>プロジェクトを作成する

1. Visual Studio で、`LayoutExample`という名前の**Windows アプリケーション**プロジェクトを作成します。

2. **Windows フォームデザイナー**でフォームを選択します。

## <a name="set-margins-for-controls"></a>コントロールの余白を設定する

<xref:System.Windows.Forms.Control.Margin%2A> プロパティを使用して、コントロール間の既定の距離を設定できます。 コントロールを別のコントロールに移動すると、2つのコントロールの余白を示すスナップ線が表示されます。 移動するコントロールは、余白で定義されている距離にもスナップされます。

### <a name="arrange-controls-on-your-form-using-the-margin-property"></a>Margin プロパティを使用してフォーム上のコントロールを配置する

1. **ツールボックス**から2つの <xref:System.Windows.Forms.Button> コントロールをフォームにドラッグします。

2. <xref:System.Windows.Forms.Button> コントロールのいずれかを選択し、ほぼタッチするまで、そのコントロールの近くに移動します。

   これらの間に表示されるスナップ線を観察します。 この距離は、2つのコントロールの <xref:System.Windows.Forms.Control.Margin%2A> 値の合計です。 移動しようとしているコントロールは、この距離にスナップされます。 詳細については、「[チュートリアル: スナップ線を使用した Windows フォームでのコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-snaplines.md)」を参照してください。

3. **[プロパティ]** ウィンドウで <xref:System.Windows.Forms.Control.Margin%2A> エントリを展開し、[<xref:System.Windows.Forms.Padding.All%2A>] プロパティを**20**に設定して、いずれかのコントロールの <xref:System.Windows.Forms.Control.Margin%2A> プロパティを変更します。

4. <xref:System.Windows.Forms.Button> コントロールのいずれかを選択し、そのコントロールの近くに移動します。

   余白の値の合計を定義するスナップ線が長くなり、コントロールが他のコントロールから離れた距離にスナップされます。

5. **[プロパティ]** ウィンドウで <xref:System.Windows.Forms.Control.Margin%2A> エントリを展開し、[<xref:System.Windows.Forms.Padding.Top%2A>] プロパティを**5**に設定して、選択したコントロールの <xref:System.Windows.Forms.Control.Margin%2A> プロパティを変更します。

6. 選択したコントロールを他のコントロールの下に移動し、スナップ線が短くなっていることを確認します。 選択したコントロールを他のコントロールの左側に移動し、スナップ線が手順 4. で観察した値を保持していることを確認します。

7. <xref:System.Windows.Forms.Control.Margin%2A> プロパティの各側面、<xref:System.Windows.Forms.Padding.Left%2A>、<xref:System.Windows.Forms.Padding.Top%2A>、<xref:System.Windows.Forms.Padding.Right%2A>、<xref:System.Windows.Forms.Padding.Bottom%2A>を異なる値に設定できます。また、<xref:System.Windows.Forms.Padding.All%2A> プロパティですべて同じ値に設定することもできます。

## <a name="set-padding-for-controls"></a>コントロールの埋め込みを設定する

アプリケーションに必要な正確なレイアウトを実現するために、コントロールには多くの場合、子コントロールが含まれます。 親コントロールの境界線に対する子コントロールの境界線の距離を指定する場合は、子コントロールの <xref:System.Windows.Forms.Control.Margin%2A> プロパティと共に、親コントロールの <xref:System.Windows.Forms.Control.Padding%2A> プロパティを使用します。 <xref:System.Windows.Forms.Control.Padding%2A> プロパティは、コントロールのコンテンツ (たとえば、<xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Text%2A> プロパティ) の境界線への近接を制御するためにも使用されます。

### <a name="arrange-controls-on-your-form-using-padding"></a>パディングを使用してフォームにコントロールを配置する

1. <xref:System.Windows.Forms.Button> ツールボックス **から** コントロールをフォームにドラッグします。

2. <xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.AutoSize%2A> プロパティの値を**true**に変更します。

3. **[プロパティ]** ウィンドウで <xref:System.Windows.Forms.Control.Padding%2A> エントリを展開し、[<xref:System.Windows.Forms.Padding.All%2A>] プロパティを**5**に設定して、<xref:System.Windows.Forms.Control.Padding%2A> プロパティを変更します。

   コントロールが拡張され、新しい埋め込み用の領域が提供されます。

4. <xref:System.Windows.Forms.GroupBox> ツールボックス **から** コントロールをフォームにドラッグします。 **[ツールボックス]** から [<xref:System.Windows.Forms.Button>] コントロールを <xref:System.Windows.Forms.GroupBox> コントロールにドラッグします。 <xref:System.Windows.Forms.Button> コントロールを <xref:System.Windows.Forms.GroupBox> コントロールの右下隅になるように配置します。

   <xref:System.Windows.Forms.Button> コントロールとして表示されるスナップ線が、<xref:System.Windows.Forms.GroupBox> コントロールの下と右の境界線に近づいていることを確認します。 これらのスナップ線は、<xref:System.Windows.Forms.Button>の <xref:System.Windows.Forms.Control.Margin%2A> プロパティに対応しています。

5. **[プロパティ]** ウィンドウの <xref:System.Windows.Forms.Control.Padding%2A> エントリを展開し、[<xref:System.Windows.Forms.Padding.All%2A>] プロパティを**20**に設定して、<xref:System.Windows.Forms.GroupBox> コントロールの <xref:System.Windows.Forms.Control.Padding%2A> プロパティを変更します。

6. <xref:System.Windows.Forms.GroupBox> コントロール内の <xref:System.Windows.Forms.Button> コントロールを選択し、<xref:System.Windows.Forms.GroupBox>の中央に移動します。

   スナップ線は、<xref:System.Windows.Forms.GroupBox> コントロールの境界から離れた場所に表示されます。 この距離は、<xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Margin%2A> プロパティと <xref:System.Windows.Forms.GroupBox> コントロールの <xref:System.Windows.Forms.Control.Padding%2A> プロパティの合計です。

## <a name="size-controls-automatically"></a>コントロールを自動的にサイズ変更する

アプリケーションによっては、デザイン時と同じように、実行時にコントロールのサイズが同じになることはありません。 たとえば、<xref:System.Windows.Forms.Button> コントロールのテキストはデータベースから取得できますが、その長さは事前にわかっていません。

<xref:System.Windows.Forms.Control.AutoSize%2A> プロパティが `true`に設定されている場合、コントロールはその内容に合わせてサイズを変更します。 詳細については、「 [AutoSize プロパティの概要](autosize-property-overview.md)」を参照してください。

### <a name="arrange-controls-on-your-form-using-the-autosize-property"></a>AutoSize プロパティを使用してフォーム上のコントロールを配置する

1. <xref:System.Windows.Forms.Button> ツールボックス **から** コントロールをフォームにドラッグします。

2. <xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.AutoSize%2A> プロパティの値を**true**に変更します。

3. <xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Text%2A> プロパティをこのボタンに変更すると、**その Text プロパティに長い文字列が**表示されます。

   変更をコミットすると、新しいテキストに合わせて <xref:System.Windows.Forms.Button> コントロールのサイズが変更されます。

4. 別の <xref:System.Windows.Forms.Button> コントロールを **[ツールボックス]** からフォームにドラッグします。

5. <xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Text%2A> プロパティを "**このボタンには、テキストプロパティの長い文字列が含まれ**ています" に変更します。

   変更をコミットすると、<xref:System.Windows.Forms.Button> コントロールのサイズは変更されず、テキストはコントロールの右端でクリップされます。

6. **[プロパティ]** ウィンドウで <xref:System.Windows.Forms.Control.Padding%2A> エントリを展開し、[<xref:System.Windows.Forms.Padding.All%2A>] プロパティを**5**に設定して、<xref:System.Windows.Forms.Control.Padding%2A> プロパティを変更します。

   コントロールの内部のテキストは、4辺すべてにクリップされます。

7. <xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.AutoSize%2A> プロパティを**true**に変更します。

   <xref:System.Windows.Forms.Button> コントロールは、文字列全体をカバーするようにサイズを変更します。 また、テキストの周囲に埋め込みが追加されているため、<xref:System.Windows.Forms.Button> コントロールが4つのすべての方向に展開されます。

8. <xref:System.Windows.Forms.Button> ツールボックス **から** コントロールをフォームにドラッグします。 フォームの右下隅近くに配置します。

9. <xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.AutoSize%2A> プロパティの値を**true**に変更します。

10. <xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Anchor%2A> プロパティを <xref:System.Windows.Forms.AnchorStyles.Right>、<xref:System.Windows.Forms.AnchorStyles.Bottom>に設定します。

11. <xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Text%2A> プロパティを "**このボタンには、テキストプロパティの長い文字列が含まれ**ています" に変更します。

   変更をコミットすると、<xref:System.Windows.Forms.Button> コントロールが左側に向かってサイズ変更されます。 一般に、自動サイズ変更では、<xref:System.Windows.Forms.Control.Anchor%2A> プロパティ設定とは逆の方向にコントロールのサイズが増加します。

## <a name="autosize-and-autosizemode-properties"></a>AutoSize および System.windows.forms.datagridviewcolumn.autosizemode プロパティ

 コントロールによっては、`AutoSizeMode` プロパティがサポートされています。これにより、コントロールの自動サイズ調整動作をよりきめ細かく制御できます。

### <a name="use-the-autosizemode-property"></a>System.windows.forms.datagridviewcolumn.autosizemode プロパティを使用する

1. <xref:System.Windows.Forms.Panel> ツールボックス **から** コントロールをフォームにドラッグします。

2. <xref:System.Windows.Forms.Panel> コントロールの <xref:System.Windows.Forms.Control.AutoSize%2A> プロパティの値を**true**に設定します。

3. **[ツールボックス]** から [<xref:System.Windows.Forms.Button>] コントロールを <xref:System.Windows.Forms.Panel> コントロールにドラッグします。

4. <xref:System.Windows.Forms.Panel> コントロールの右下隅近くに <xref:System.Windows.Forms.Button> コントロールを配置します。

5. <xref:System.Windows.Forms.Panel> コントロールを選択し、右下のサイズ変更ハンドルを取得します。 <xref:System.Windows.Forms.Panel> コントロールのサイズを大きくして小さくします。

   > [!NOTE]
   > <xref:System.Windows.Forms.Panel> コントロールのサイズは自由に変更できますが、<xref:System.Windows.Forms.Button> コントロールの右下隅の位置よりもサイズを小さくすることはできません。 この動作は、`AutoSizeMode` プロパティの既定値 (<xref:System.Windows.Forms.AutoSizeMode.GrowOnly>) によって指定されます。

6. <xref:System.Windows.Forms.Panel> コントロールの `AutoSizeMode` プロパティの値を <xref:System.Windows.Forms.AutoSizeMode.GrowAndShrink>に設定します。

   <xref:System.Windows.Forms.Panel> コントロールは、<xref:System.Windows.Forms.Button> コントロールを囲むようにサイズを調整します。 <xref:System.Windows.Forms.Panel> コントロールのサイズを変更することはできません。

7. <xref:System.Windows.Forms.Button> コントロールを <xref:System.Windows.Forms.Panel> コントロールの左上隅にドラッグします。

   <xref:System.Windows.Forms.Panel> コントロールは、<xref:System.Windows.Forms.Button> コントロールの新しい位置にサイズ変更されます。

## <a name="next-steps"></a>次のステップ

Windows フォームアプリケーションにコントロールを配置するためのレイアウト機能は他にも多数あります。 いくつかの組み合わせを次に示します。

- <xref:System.Windows.Forms.TableLayoutPanel> コントロールを使用してフォームを作成します。 詳細については、「[チュートリアル: TableLayoutPanel を使用した Windows フォームでのコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md)」を参照してください。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールの <xref:System.Windows.Forms.Control.Padding%2A> プロパティの値、および子コントロールの <xref:System.Windows.Forms.Control.Margin%2A> プロパティを変更してみてください。

- <xref:System.Windows.Forms.FlowLayoutPanel> コントロールを使用して同じ実験を試してみてください。 詳細については、「[チュートリアル: FlowLayoutPanel を使用した Windows フォームでのコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-flowlayoutpanel.md)」を参照してください。

- <xref:System.Windows.Forms.Panel> コントロールにドッキングされた子コントロールを試してみてください。 <xref:System.Windows.Forms.Control.Padding%2A> プロパティは、<xref:System.Windows.Forms.ScrollableControl.DockPadding%2A> プロパティの一般的な実現方法であり、<xref:System.Windows.Forms.Panel> コントロールに子コントロールを配置し、子コントロールの <xref:System.Windows.Forms.Control.Dock%2A> プロパティを <xref:System.Windows.Forms.DockStyle.Fill>に設定することによって、このことを満たすことができます。 <xref:System.Windows.Forms.Panel> コントロールの <xref:System.Windows.Forms.Control.Padding%2A> プロパティをさまざまな値に設定し、効果を確認します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Control.AutoSize%2A>
- <xref:System.Windows.Forms.ScrollableControl.DockPadding%2A>
- <xref:System.Windows.Forms.Control.Margin%2A>
- <xref:System.Windows.Forms.Control.Padding%2A>
- [AutoSize プロパティの概要](autosize-property-overview.md)
- [チュートリアル: TableLayoutPanel を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md)
- [チュートリアル: FlowLayoutPanel を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-flowlayoutpanel.md)
- [チュートリアル: スナップ線を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-snaplines.md)
