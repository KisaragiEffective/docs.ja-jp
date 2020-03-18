---
title: スナップ線を使用したコントロールの配置
ms.date: 03/30/2017
helpviewer_keywords:
- controls [Windows Forms], arranging with snaplines
- snaplines [Windows Forms], arranging Windows Forms controls
- SnapLine class [Windows Forms], walkthroughs
- Windows Forms controls, arranging
ms.assetid: d5c9edc7-cf30-4a97-8ebe-201d569340f8
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 0b68a70b55cbf03d480fd388a637a4caf78b6eaa
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628801"
---
# <a name="walkthrough-arrange-controls-on-windows-forms-using-snaplines"></a>チュートリアル: スナップ線を使用した Windows フォームでのコントロールの配置

フォーム上のコントロールを正確に配置することは、多くのアプリケーションで優先度の高い作業です。 Windows フォームデザイナーには、これを実現するための多数のレイアウトツールが用意されています。 最も重要なものの1つは、<xref:System.Windows.Forms.Design.Behavior.SnapLine> 機能です。

スナップ線を使用すると、他のコントロールとコントロールを整列する場所を正確に確認できます。 また、 [Windows ユーザーインターフェイスのガイドライン](/windows/win32/uxguide/guidelines)に従って、コントロール間の余白の推奨される距離も示しています。

スナップ線を使用すると、コントロールの配置が簡単になり、プロフェッショナルな外観と動作 (ルックアンドフィール) がわかりやすくなります。

## <a name="create-the-project"></a>プロジェクトを作成する

1. Visual Studio で、"SnaplineExample" という名前の Windows ベースのアプリケーションプロジェクトを作成します。

2. フォーム デザイナーでフォームを選びます。

## <a name="space-and-align-controls"></a>[スペースとコントロールの整列]

スナップ線を使用すると、フォーム上にコントロールを配置するための正確で直感的な方法が提供されます。 これは、選択したコントロールを、別のコントロールまたはコントロールのセットと一致する位置の近くに移動するときに表示されます。 他のコントロールを移動すると、選択した位置が提案された位置に "スナップ" されます。

### <a name="to-arrange-controls-using-snaplines"></a>スナップ線を使用してコントロールを配置するには

1. <xref:System.Windows.Forms.Button> ツールボックス **から** コントロールをフォームにドラッグします。

2. <xref:System.Windows.Forms.Button> コントロールをフォームの右下隅に移動します。 <xref:System.Windows.Forms.Button> コントロールとして表示されるスナップ線は、フォームの下と右の境界線に近づいています。 これらのスナップ線には、コントロールの境界線とフォームとの間の推奨される距離が表示されます。

3. フォームの境界線を中心に <xref:System.Windows.Forms.Button> コントロールを移動し、スナップ線が表示される場所をメモします。 操作が完了したら、フォームの中央付近にある <xref:System.Windows.Forms.Button> コントロールを移動します。

4. 別の <xref:System.Windows.Forms.Button> コントロールを **[ツールボックス]** からフォームにドラッグします。

5. 2番目の <xref:System.Windows.Forms.Button> コントロールを、最初のレベルに近い方に移動します。 両方のボタンのテキストベースラインに表示されるスナップ線に注目してください。移動しようとしているコントロールは、他のコントロールと正確に同じ位置にスナップします。

6. 2番目の <xref:System.Windows.Forms.Button> コントロールを、最初ののすぐ上に配置するまで移動します。 両方のボタンの左端と右端に表示されるスナップ線に注目してください。移動するコントロールは、他のコントロールと正確に一致する位置にスナップされることに注意してください。

7. <xref:System.Windows.Forms.Button> コントロールのいずれかを選択し、ほぼタッチするまで、そのコントロールの近くに移動します。 これらの間に表示されるスナップ線に注意してください。 この距離は、コントロールの境界線の間の推奨される距離です。 また、移動しようとしているコントロールがこの位置にスナップされることにも注意してください。

8. **ツールボックス**から2つの <xref:System.Windows.Forms.Panel> コントロールをフォームにドラッグします。

9. <xref:System.Windows.Forms.Panel> コントロールのいずれかを1つ目のコントロールの最上位レベルまで移動します。 両方のコントロールの上端と下端に表示されるスナップ線に注目します。また、移動しようとしているコントロールは、他のコントロールと正確に同じ位置にスナップします。

## <a name="align-to-form-and-container-margins"></a>フォームとコンテナーの余白に合わせる

スナップ線を使用すると、一貫した方法でコントロールを配置し、コンテナーの余白を整えることができます。

1. <xref:System.Windows.Forms.Button> コントロールの1つを選択し、スナップ線が表示されるまでフォームの右の境界線の近くに移動します。 右の境界線からのスナップ線の距離は、コントロールの <xref:System.Windows.Forms.Control.Margin%2A> プロパティとフォームの <xref:System.Windows.Forms.Control.Padding%2A> プロパティ値の合計です。

   > [!NOTE]
   > フォームの <xref:System.Windows.Forms.Control.Padding%2A> プロパティが0、0、0、0に設定されている場合、Windows フォームデザイナーによって、影付きの <xref:System.Windows.Forms.Control.Padding%2A> 値 (9、9、9、9) が示されます。 この動作をオーバーライドするには、0、0、0、0以外の値を割り当てます。

2. **[プロパティ]** ウィンドウで <xref:System.Windows.Forms.Control.Margin%2A> エントリを展開し、[<xref:System.Windows.Forms.Padding.All%2A>] プロパティを0に設定して、<xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Margin%2A> プロパティの値を変更します。 詳細については、「[チュートリアル: 埋め込み、余白、AutoSize プロパティを使用した Windows フォームコントロールのレイアウト](windows-forms-controls-padding-autosize.md)」を参照してください。

3. スナップ線が表示されるまで、<xref:System.Windows.Forms.Button> コントロールをフォームの右の境界線の近くに移動します。 この距離は、フォームの <xref:System.Windows.Forms.Control.Padding%2A> プロパティの値によって指定されるようになりました。

4. <xref:System.Windows.Forms.GroupBox> ツールボックス **から** コントロールをフォームにドラッグします。

5. **[プロパティ]** ウィンドウの <xref:System.Windows.Forms.Control.Padding%2A> エントリを展開し、[<xref:System.Windows.Forms.Padding.All%2A>] プロパティを10に設定して、<xref:System.Windows.Forms.GroupBox> コントロールの <xref:System.Windows.Forms.Control.Padding%2A> プロパティの値を変更します。

6. **[ツールボックス]** から [<xref:System.Windows.Forms.Button>] コントロールを <xref:System.Windows.Forms.GroupBox> コントロールにドラッグします。

7. スナップ線が表示されるまで、<xref:System.Windows.Forms.GroupBox> コントロールの右の境界線の近くに <xref:System.Windows.Forms.Button> コントロールを移動します。 <xref:System.Windows.Forms.GroupBox> コントロール内で <xref:System.Windows.Forms.Button> コントロールを移動し、スナップ線が表示される場所をメモします。

## <a name="align-to-grouped-controls"></a>グループ化されたコントロールに揃える

スナップ線を使用すると、グループ化されたコントロールだけでなく、<xref:System.Windows.Forms.GroupBox> コントロール内のコントロールも配置できます。

1. フォーム上の2つのコントロールを選択します。 選択範囲を移動し、選択範囲と他のコントロールの間に表示されるスナップ線をメモします。

2. <xref:System.Windows.Forms.GroupBox> ツールボックス **から** コントロールをフォームにドラッグします。

3. **[ツールボックス]** から [<xref:System.Windows.Forms.Button>] コントロールを <xref:System.Windows.Forms.GroupBox> コントロールにドラッグします。

4. <xref:System.Windows.Forms.Button> コントロールのいずれかを選択し、<xref:System.Windows.Forms.GroupBox> コントロールの周囲に移動します。 <xref:System.Windows.Forms.GroupBox> コントロールの端に表示されるスナップ線に注意してください。 また、<xref:System.Windows.Forms.GroupBox> コントロールに含まれる <xref:System.Windows.Forms.Button> コントロールの端に表示されるスナップ線もメモします。 コンテナーコントロールの子であるコントロールは、スナップ線もサポートします。

## <a name="use-snaplines-to-place-a-control-by-outlining-its-size"></a>スナップ線を使用して、サイズをアウトラインし、コントロールを配置する

1. **[ツールボックス]** で <xref:System.Windows.Forms.Button> コントロール アイコンをクリックします。 フォームにドラッグしないでください。

2. フォームのデザイン画面上にマウスポインターを移動します。 ポインターが <xref:System.Windows.Forms.Button> コントロール アイコンが付いた十字カーソルに変わることにご注意ください。 また、<xref:System.Windows.Forms.Button> コントロールに対して配置された位置を示すために表示されるスナップ線もメモします。

3. マウス ボタンを押したままにします。

4. フォームの周りにマウスポインターをドラッグします。 コントロールの位置とサイズを示すアウトラインが描画されることに注意してください。

5. フォーム上の別のコントロールに合わせて、ポインターをドラッグします。 スナップ線が配置を示すように見えることに注意してください。

6. マウスのボタンを離します。 コントロールは、アウトラインによって示される位置とサイズで作成されます。

## <a name="use-snaplines-when-dragging-a-control-from-the-toolbox"></a>ツールボックスからコントロールをドラッグするときにスナップ線を使用する

1. <xref:System.Windows.Forms.Button> コントロールを**ツールボックス**からフォームにドラッグしますが、マウスボタンを離しません。

2. フォームのデザイン画面上にマウスポインターを移動します。 ポインターは、新しい <xref:System.Windows.Forms.Button> コントロールが作成される位置を示すように変更されることに注意してください。

3. フォームの周りにマウスポインターをドラッグします。 <xref:System.Windows.Forms.Button> コントロールに対して配置された位置を提案するために表示されるスナップ線に注意してください。 他のコントロールに合わせて配置された位置を検索します。

4. マウスのボタンを離します。 コントロールは、スナップ線によって示される位置に作成されます。

## <a name="resize-a-control-using-snaplines"></a>スナップ線を使用したコントロールのサイズ変更

1. <xref:System.Windows.Forms.Button> ツールボックス **から** コントロールをフォームにドラッグします。

2. 角のサイズ変更ハンドルとドラッグを取得して、<xref:System.Windows.Forms.Button> コントロールのサイズを変更します。 詳細については、「[方法: Windows フォームのコントロールのサイズを変更する](how-to-resize-controls-on-windows-forms.md)」を参照してください。

3. サイズ変更ハンドルをドラッグして、<xref:System.Windows.Forms.Button> コントロールの境界線の1つが別のコントロールに揃うようにします。 スナップ線が表示されることに注意してください。 また、サイズ変更ハンドルはスナップ線によって示される位置にスナップされることにも注意してください。

4. さまざまな方向に <xref:System.Windows.Forms.Button> コントロールのサイズを変更し、サイズ変更ハンドルを別のコントロールに揃えます。 整列を示すために、さまざまな向きでスナップ線がどのように表示されるかに注目してください。

## <a name="align-a-label-to-a-controls-text"></a>コントロールのテキストにラベルを揃える

1. <xref:System.Windows.Forms.TextBox> ツールボックス **から** コントロールをフォームにドラッグします。 フォームに <xref:System.Windows.Forms.TextBox> コントロールをドロップする場合は、スマートタググリフをクリックし、 **[テキストを textBox1 に設定]** オプションを選択します。 詳細については、「[チュートリアル: デザイナーアクションを使用した一般的なタスクの実行](perform-common-tasks-design-actions.md)」を参照してください。

2. <xref:System.Windows.Forms.Label> ツールボックス **から** コントロールをフォームにドラッグします。

3. <xref:System.Windows.Forms.Label> コントロールの <xref:System.Windows.Forms.Control.AutoSize%2A> プロパティの値を `true`に変更します。 コントロールの境界線は、表示テキストに合わせて調整されることに注意してください。

4. <xref:System.Windows.Forms.Label> コントロールを <xref:System.Windows.Forms.TextBox> コントロールの左側に移動すると、<xref:System.Windows.Forms.TextBox> コントロールの下端に揃えられます。 2つのコントロールの下端に沿って表示されるスナップ線に注意してください。

5. <xref:System.Windows.Forms.Label> テキストと <xref:System.Windows.Forms.TextBox> テキストが揃うまで、<xref:System.Windows.Forms.Label> コントロールを少し上に移動します。 表示される別のスタイルのスナップ線を確認すると、両方のコントロールのテキストフィールドがアラインされたことを示します。

## <a name="use-snaplines-with-keyboard-navigation"></a>キーボードナビゲーションでスナップ線を使用する

1. <xref:System.Windows.Forms.Button> ツールボックス **から** コントロールをフォームにドラッグします。 フォームの左上隅に配置します。

2. **Ctrl**+**↓**キーを押します。 コントロールが、最初に使用可能な水平方向の配置位置にフォームを下方向に移動することに注意してください。

3. **Ctrl**+↓キーを押して、コントロールがフォームの下部に到達するまで**移動**します。 フォームの下に移動すると、その位置に注意してください。

4. **Ctrl**+**→**キーを押します。 コントロールは、フォーム上で、最初に使用可能な垂直方向の配置位置に移動することに注意してください。

5. コントロールがフォームの側に到達するまで、Ctrl キーを押し**ながら** **右方向**キーを+ます。 フォーム間を移動するときに占有される位置に注意してください。

6. 方向キーの組み合わせを使用して、コントロールをフォームの周囲に移動します。 コントロールが占有する位置と、コントロールに付随するスナップ線をメモします。

7. **Shift**キーを押し+**方向キー**を押して、1ピクセルずつインクリメントして <xref:System.Windows.Forms.Button> コントロールのサイズを変更します。

8. **Ctrl**++**Shift**キーを押しながら**方向キー**を押して、スナップ線のインクリメントで <xref:System.Windows.Forms.Button> コントロールのサイズを変更します。

## <a name="selectively-disable-snaplines"></a>スナップガイドラインを選択的に無効にする

1. <xref:System.Windows.Forms.TableLayoutPanel> ツールボックス **から** コントロールをフォームにドラッグします。

2. <xref:System.Windows.Forms.Button> ツールボックス **の**コントロール アイコンをダブルクリックします。 <xref:System.Windows.Forms.TableLayoutPanel> コントロールの最初のセルに新しいボタンコントロールが表示されることに注意してください。

3. **ツールボックス**の [<xref:System.Windows.Forms.Button> コントロール] アイコンを2回ダブルクリックします。 これにより、<xref:System.Windows.Forms.TableLayoutPanel> コントロールに1つの空のセルが残されます。

4. **[ツールボックス]** から <xref:System.Windows.Forms.Button> コントロールを <xref:System.Windows.Forms.TableLayoutPanel> コントロールの空のセルにドラッグします。 スナップ線は表示されないことに注意してください。

5. <xref:System.Windows.Forms.Button> コントロールを <xref:System.Windows.Forms.TableLayoutPanel> コントロールの外にドラッグし、<xref:System.Windows.Forms.TableLayoutPanel> コントロールの周囲に移動します。 スナップ線が再び表示されることに注意してください。

## <a name="disable-snaplines"></a>スナップガイドラインの無効化

**Alt**キーを押しながら、コントロールをフォームの上に移動します。

スナップ線は表示されず、コントロールは潜在的な配置位置にスナップされません。

### <a name="to-disable-snaplines-in-the-design-environment"></a>デザイン環境でスナップガイドラインを無効にするには

1. **[ツール]** メニューの **[オプション]** ダイアログボックスを開きます。 **[Windows フォームデザイナー]** を選択します。

2. **[全般]** ノードを選択します。 **[レイアウトモード]** セクションで、選択範囲を**スナップガイドライン**から**SnapToGrid**に変更します。

3. **[OK]** を選択して設定を適用します。

4. フォーム上のコントロールを選択し、他のコントロールの周囲に移動します。 スナップ線が表示されないことに注意してください。

## <a name="next-steps"></a>次のステップ:

スナップ線は、フォーム上にコントロールを配置するための直感的な手段を提供します。 さらに理解を深めるには、次の操作を行うことをお勧めします。

- <xref:System.Windows.Forms.GroupBox> コントロールを別の <xref:System.Windows.Forms.GroupBox> コントロール内に入れ子にしてみてください。 子 <xref:System.Windows.Forms.GroupBox> コントロールに <xref:System.Windows.Forms.Button> コントロールを配置し、親 <xref:System.Windows.Forms.GroupBox> コントロール内で別のコントロールを配置します。 <xref:System.Windows.Forms.Button> コントロールを移動して、スナップ線がコンテナーの境界を越えていることを確認します。

- <xref:System.Windows.Forms.TextBox> コントロールの列と、<xref:System.Windows.Forms.Label> コントロールの対応する列を作成します。 <xref:System.Windows.Forms.Label> コントロールの <xref:System.Windows.Forms.Control.AutoSize%2A> プロパティの値を `true`に設定します。 スナップ線を使用して <xref:System.Windows.Forms.Label> コントロールを移動し、表示されているテキストが <xref:System.Windows.Forms.TextBox> コントロール内のテキストに合わせて調整されるようにします。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Design.Behavior.SnapLine>
- [チュートリアル: FlowLayoutPanel を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-flowlayoutpanel.md)
- [チュートリアル: TableLayoutPanel を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md)
- [チュートリアル: Padding、Margin、および AutoSize プロパティを使用した Windows フォーム コントロールのレイアウト](windows-forms-controls-padding-autosize.md)
