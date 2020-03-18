---
title: 'チュートリアル : メニューのマージと ToolStrip コントロールのある MDI フォームを作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- toolbars [Windows Forms]
- ToolStripPanel control [Windows Forms]
- MDI [Windows Forms], creating forms
- multiple document interface forms
- MDI forms
- ToolStrip control [Windows Forms]
- MDI forms [Windows Forms], creating
- MDI forms [Windows Forms], walkthroughs
ms.assetid: fbab4221-74af-42d0-bbf4-3c97f7b2e544
ms.openlocfilehash: e0343b98cb71521b35418e70550a93e0bfe20fa8
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628788"
---
# <a name="walkthrough-creating-an-mdi-form-with-menu-merging-and-toolstrip-controls"></a>チュートリアル : メニューのマージと ToolStrip コントロールのある MDI フォームを作成する

<xref:System.Windows.Forms?displayProperty=nameWithType> 名前空間は、マルチ ドキュメント インターフェイス (MDI) アプリケーションをサポートし、<xref:System.Windows.Forms.MenuStrip> コントロールはメニューの結合をサポートします。 MDI フォームは、<xref:System.Windows.Forms.ToolStrip> コントロールもサポートします。

このチュートリアルでは、<xref:System.Windows.Forms.ToolStripPanel> コントロールを MDI フォームで使用する方法について説明します。 フォームは、子メニューをマージするメニューもサポートしています。 このチュートリアルでは、次のタスクについて説明します。

- Windows フォームプロジェクトを作成しています。

- フォームのメインメニューを作成します。 メニューの実際の名前は異なります。

- <xref:System.Windows.Forms.ToolStripPanel> コントロールを**ツールボックス**に追加します。

- 子フォームを作成する。

- <xref:System.Windows.Forms.ToolStripPanel> コントロールを z オーダーによって配置する。

操作が完了すると、メニューの結合および移動可能な <xref:System.Windows.Forms.ToolStrip> コントロールをサポートする MDI フォームが作成されます。

このトピックのコードを単一のリストとしてコピーする方法については、「[方法: メニューのマージと ToolStrip コントロールを使用して MDI フォームを作成](how-to-create-an-mdi-form-with-menu-merging-and-toolstrip-controls.md)する」を参照してください。

## <a name="prerequisites"></a>前提条件

このチュートリアルを完了するには、Visual Studio が必要です。

## <a name="create-the-project"></a>プロジェクトを作成する

1. Visual Studio で、 **system.windows.forms.toolstrip.mdiform**という名前の Windows アプリケーションプロジェクトを作成します。このプロジェクトには、  **C# visual**または**Visual Basic** > **クラシックデスクトップ** > Windows フォーム**アプリケーション**) と**いう > の** **新しい** > **プロジェクト** > ます。

2. Windows フォームデザイナーで、フォームを選択します。

3. プロパティウィンドウで、<xref:System.Windows.Forms.Form.IsMdiContainer%2A> の値を `true`に設定します。

## <a name="create-the-main-menu"></a>メインメニューを作成する

親 MDI フォームには、メインメニューが含まれています。 メインメニューには、 **Window**という名前のメニュー項目が1つあります。 **[ウィンドウ]** メニュー項目を使用すると、子フォームを作成できます。 子フォームのメニュー項目は、メインメニューにマージされます。

1. **[ツールボックス]** から <xref:System.Windows.Forms.MenuStrip> コントロールをフォームにドラッグします。

2. <xref:System.Windows.Forms.MenuStrip> コントロールに <xref:System.Windows.Forms.ToolStripMenuItem> を追加し、**ウィンドウ**に名前を指定します。

3. <xref:System.Windows.Forms.MenuStrip> コントロールを選択します。

4. プロパティウィンドウで、<xref:System.Windows.Forms.MenuStrip.MdiWindowListItem%2A> プロパティの値を `ToolStripMenuItem1`に設定します。

5. [ウィンドウ] メニュー項目にサブ項目を追加し、サブ項目に「 **New** **」** という名前を指定します。

6. プロパティウィンドウで、 **[イベント]** をクリックします。

7. <xref:System.Windows.Forms.ToolStripItem.Click> イベントをダブルクリックします。

     Windows フォームデザイナーは、<xref:System.Windows.Forms.ToolStripItem.Click> イベントのイベントハンドラーを生成します。

8. イベントハンドラーに次のコードを挿入します。

     [!code-csharp[System.Windows.Forms.ToolStrip.MdiForm#2](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.ToolStrip.MdiForm/CS/Form1.cs#2)]
     [!code-vb[System.Windows.Forms.ToolStrip.MdiForm#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.ToolStrip.MdiForm/VB/Form1.vb#2)]

## <a name="add-the-toolstrippanel-control-to-the-toolbox"></a>ToolStripPanel コントロールをツールボックスに追加する

<xref:System.Windows.Forms.MenuStrip> コントロールを MDI フォームで使用する場合は、<xref:System.Windows.Forms.ToolStripPanel> コントロールが必要です。 Windows フォームデザイナーで MDI フォームをビルドするには、<xref:System.Windows.Forms.ToolStripPanel> コントロールを**ツールボックス**に追加する必要があります。

1. **[ツールボックス]** を開き、 **[すべての Windows フォーム]** タブをクリックして、使用可能な Windows フォームコントロールを表示します。

2. 右クリックしてショートカットメニューを開き、 **[項目の選択]** を選択します。

3. **[ツールボックスアイテムの選択]** ダイアログボックスで、 **ToolStripPanel**が表示されるまで **[名前]** 列を下にスクロールします。

4. **ToolStripPanel**のチェックボックスをオンにして、[ **OK]** をクリックします。

     <xref:System.Windows.Forms.ToolStripPanel> コントロールが **[ツールボックス]** に表示されます。

## <a name="create-a-child-form"></a>子フォームを作成する

この手順では、独自の <xref:System.Windows.Forms.MenuStrip> コントロールを持つ別の子フォームクラスを定義します。 このフォームのメニュー項目は、親フォームのメニュー項目とマージされます。

1. `ChildForm` という名前の新しいフォームをプロジェクトに追加します。

     詳細については、「[方法: プロジェクトに Windows フォームを追加](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/y2xxdce3(v=vs.100))する」を参照してください。

2. **[ツールボックス]** から <xref:System.Windows.Forms.MenuStrip> コントロールを子フォームにドラッグします。

3. <xref:System.Windows.Forms.MenuStrip> コントロールのデザイナーアクショングリフ (![小さい黒い矢印](./media/designer-actions-glyph.gif)) をクリックし、 **[アイテムの編集]** を選択します。

4. **[項目コレクションエディター]** ダイアログボックスで、 **childmenuitem**という名前の新しい <xref:System.Windows.Forms.ToolStripMenuItem> を子メニューに追加します。

     詳細については、「 [ToolStrip Items コレクションエディター](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/ms233643(v=vs.100))」を参照してください。

## <a name="test-the-form"></a>フォームをテストする

1. **F5**キーを押して、フォームをコンパイルして実行します。

2. **[ウィンドウ]** メニュー項目をクリックしてメニューを開き、 **[新規]** をクリックします。

     フォームの MDI クライアント領域に新しい子フォームが作成されます。 子フォームのメニューがメインメニューにマージされます。

3. 子フォームを閉じます。

     子フォームのメニューがメインメニューから削除されます。

4. **[新規]** を数回クリックします。

     <xref:System.Windows.Forms.MenuStrip> コントロールの <xref:System.Windows.Forms.MenuStrip.MdiWindowListItem%2A> プロパティが割り当てられているため、子フォームは **[ウィンドウ]** メニュー項目の下に自動的に表示されます。

## <a name="add-toolstrip-support"></a>ToolStrip サポートの追加

この手順では、4つの <xref:System.Windows.Forms.ToolStrip> コントロールを MDI 親フォームに追加します。 各 <xref:System.Windows.Forms.ToolStrip> コントロールは、フォームの端にドッキングされる <xref:System.Windows.Forms.ToolStripPanel> コントロール内に追加されます。

1. **[ツールボックス]** から <xref:System.Windows.Forms.ToolStripPanel> コントロールをフォームにドラッグします。

2. <xref:System.Windows.Forms.ToolStripPanel> コントロールを選択した状態で、**ツールボックス**の <xref:System.Windows.Forms.ToolStrip> コントロールをダブルクリックします。

     <xref:System.Windows.Forms.ToolStripPanel> コントロールに <xref:System.Windows.Forms.ToolStrip> コントロールが作成されます。

3. <xref:System.Windows.Forms.ToolStripPanel> コントロールを選択します。

4. プロパティウィンドウで、コントロールの <xref:System.Windows.Forms.Control.Dock%2A> プロパティの値を <xref:System.Windows.Forms.DockStyle.Left>に変更します。

     メインメニューの下にあるフォームの左側に、<xref:System.Windows.Forms.ToolStripPanel> コントロールがドッキングされます。 MDI クライアント領域は、<xref:System.Windows.Forms.ToolStripPanel> コントロールに合わせてサイズが変更されます。

5. 手順 1. ~ 4. を繰り返します。

     新しい <xref:System.Windows.Forms.ToolStripPanel> コントロールをフォームの上部にドッキングします。

     <xref:System.Windows.Forms.ToolStripPanel> コントロールはメインメニューの下にドッキングされますが、最初の <xref:System.Windows.Forms.ToolStripPanel> コントロールの右側にドッキングされます。 この手順では、<xref:System.Windows.Forms.ToolStripPanel> コントロールを正しく配置するための z オーダーの重要性について説明します。

6. 2つの <xref:System.Windows.Forms.ToolStripPanel> コントロールについて、手順 1. ~ 4. を繰り返します。

     新しい <xref:System.Windows.Forms.ToolStripPanel> コントロールをフォームの右端と下部にドッキングします。

## <a name="arrange-toolstrippanel-controls-by-z-order"></a>ToolStripPanel コントロールを Z オーダーで整列する

MDI フォーム上のドッキングされた <xref:System.Windows.Forms.ToolStripPanel> コントロールの位置は、コントロールの z オーダーでの位置によって決まります。 [ドキュメントアウトライン] ウィンドウでは、コントロールの z オーダーを簡単に配置できます。

1. **[表示]** メニューの **[その他のウィンドウ]** をクリックし、 **[ドキュメントアウトライン]** をクリックします。

     前の手順の <xref:System.Windows.Forms.ToolStripPanel> コントロールの配置は非標準です。 これは、z オーダーが正しくないためです。 [ドキュメントアウトライン] ウィンドウを使用すると、コントロールの z オーダーを変更できます。

2. ドキュメントアウトライン ウィンドウで、 **ToolStripPanel4** を選択します。

3. **ToolStripPanel4**が一覧の一番下に表示されるまで、下向きの矢印ボタンを繰り返しクリックします。

     **ToolStripPanel4**コントロールは、他のコントロールの下にあるフォームの下部にドッキングされます。

4. **[ToolStripPanel2]** を選択します。

5. 下向き矢印ボタンを1回クリックして、コントロールを一覧の3番目に配置します。

     **ToolStripPanel2**コントロールは、メインメニューの下、他のコントロールの上にあるフォームの上部にドッキングされます。

6. **[ドキュメントアウトライン]** ウィンドウでさまざまなコントロールを選択し、z オーダーの別の位置に移動します。 ドッキングされたコントロールの配置に対する z オーダーの効果に注意してください。 **[編集]** メニューの CTRL + Z または**元に戻す**を使用して、変更を元に戻します。

## <a name="checkpoint---test-your-form"></a>チェックポイント-フォームのテスト

1. **F5**キーを押して、フォームをコンパイルして実行します。

2. <xref:System.Windows.Forms.ToolStrip> コントロールのグリップをクリックし、フォーム上の別の位置にコントロールをドラッグします。

     <xref:System.Windows.Forms.ToolStrip> コントロールを1つの <xref:System.Windows.Forms.ToolStripPanel> コントロールから別のコントロールにドラッグすることができます。

## <a name="next-steps"></a>次のステップ:

このチュートリアルでは、<xref:System.Windows.Forms.ToolStrip> コントロールとメニューのマージを使用して、MDI 親フォームを作成しました。 <xref:System.Windows.Forms.ToolStrip> のコントロールファミリは、他のさまざまな用途に使用できます。

- <xref:System.Windows.Forms.ContextMenuStrip>を使用して、コントロールのショートカットメニューを作成します。 詳細については、「 [ContextMenu コンポーネントの概要](contextmenu-component-overview-windows-forms.md)」を参照してください。

- 自動的に設定された標準メニューを使用してフォームを作成しました。 詳細については、「[チュートリアル: フォームに標準メニュー項目を提供する](walkthrough-providing-standard-menu-items-to-a-form.md)」を参照してください。

- <xref:System.Windows.Forms.ToolStrip> コントロールにプロフェッショナルな外観を与えます。 詳細については、「[方法: アプリケーションの ToolStrip レンダラーを設定する](how-to-set-the-toolstrip-renderer-for-an-application.md)」を参照してください。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.MenuStrip>
- <xref:System.Windows.Forms.ToolStrip>
- <xref:System.Windows.Forms.StatusStrip>
- [方法: MDI 親フォームを作成する](../advanced/how-to-create-mdi-parent-forms.md)
- [方法: MDI 子フォームを作成する](../advanced/how-to-create-mdi-child-forms.md)
- [方法: MDI ドロップダウン メニューに MenuStrip を挿入する](how-to-insert-a-menustrip-into-an-mdi-drop-down-menu-windows-forms.md)
- [ToolStrip コントロール](toolstrip-control-windows-forms.md)
