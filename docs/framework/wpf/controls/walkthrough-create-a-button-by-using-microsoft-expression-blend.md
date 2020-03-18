---
title: 'チュートリアル : Microsoft Expression Blend を使用してボタンを作成する'
ms.date: 03/30/2017
helpviewer_keywords:
- buttons [WPF]
- converting [WPF], shape to button
- Expression Blend [WPF Designer]
ms.assetid: ff5037c2-bba7-4cae-8abb-6475b686c48e
ms.openlocfilehash: 10d049288cf560dadedf7bc5e624deb7c42aae81
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636173"
---
# <a name="walkthrough-create-a-button-by-using-microsoft-expression-blend"></a>チュートリアル : Microsoft Expression Blend を使用してボタンを作成する

このチュートリアルでは、Microsoft Expression Blend を使用して、WPF のカスタマイズされたボタンを作成する手順について説明します。

> [!IMPORTANT]
> Microsoft Expression Blend は、実行可能プログラムを作成するためにコンパイルされた [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] を生成することによって機能します。 XAML を直接操作する場合は、Blend ではなく Visual Studio で XAML を使用して、同じアプリケーションを作成するもう1つのチュートリアルがあります。 詳細について[は、「XAML を使用したボタンの作成](walkthrough-create-a-button-by-using-xaml.md)」を参照してください。

次の図は、作成するカスタマイズされたボタンを示しています。

![ユーザーが作成するカスタマイズされたボタン](./media/custom-button-blend-intro.jpg)

## <a name="convert-a-shape-to-a-button"></a>図形をボタンに変換する

このチュートリアルの最初の部分では、カスタムボタンの外観を独自に作成します。 これを行うには、最初に四角形をボタンに変換します。 次に、ボタンのテンプレートに図形を追加して、より複雑な外観のボタンを作成します。 通常のボタンで開始してカスタマイズするのはなぜですか。 ボタンには必要のない組み込みの機能があるため、カスタムボタンの場合は、四角形から始める方が簡単です。

### <a name="to-create-a-new-project-in-expression-blend"></a>Expression Blend で新しいプロジェクトを作成するには

1. Expression Blend を開始します。 ( **[スタート]** をクリックし、 **[すべてのプログラム]** 、 **[microsoft expression]** の順にポイントし、 **[microsoft expression Blend]** をクリックします)。

2. 必要に応じて、アプリケーションを最大化します。

3. **[ファイル]** メニューの **[新しいプロジェクト]** をクリックします。

4. **[標準アプリケーション (.exe)]** を選択します。

5. プロジェクトに `CustomButton` という名前を指定し、 **[OK]** をクリックします。

この時点で、空の WPF プロジェクトがあります。 F5 キーを押してアプリケーションを実行できます。 ご想像のとおり、アプリケーションは空のウィンドウだけで構成されています。 次に、角丸四角形を作成し、ボタンに変換します。

### <a name="to-convert-a-rectangle-to-a-button"></a>四角形をボタンに変換するには

1. **Window Background プロパティを black に設定します。** ウィンドウを選択し、**プロパティ タブ**をクリックして、<xref:System.Windows.Controls.Control.Background%2A> プロパティを `Black`に設定します。

    ![ボタンの背景を黒に設定する方法](./media/custom-button-blend-changebackground.png)

2. **ウィンドウのボタンのサイズとほぼ同じ四角形を描画します。** 左側のツールパネルで [四角形] ツールを選択し、四角形をウィンドウにドラッグします。

    ![四角形を描画する方法](./media/custom-button-blend-drawrect.png)

3. **四角形の角を丸めます。** 四角形のコントロールポイントをドラッグするか、<xref:System.Windows.Shapes.Rectangle.RadiusX%2A> と <xref:System.Windows.Shapes.Rectangle.RadiusY%2A> のプロパティを直接設定します。 <xref:System.Windows.Shapes.Rectangle.RadiusX%2A> と <xref:System.Windows.Shapes.Rectangle.RadiusY%2A> の値を20に設定します。

    ![四角形の角を丸くする方法](./media/custom-button-blend-roundcorners.png)

4. **四角形をボタンに変更します。** 四角形を選択します。 **[ツール]** メニューの [**作成] ボタン**をクリックします。

    ![図形をボタンにする方法](./media/custom-button-blend-makebutton.png)

5. **スタイル/テンプレートのスコープを指定します。** 次のようなダイアログボックスが表示されます。

    ![[Style リソースの作成] ダイアログ ボックス](./media/custom-button-blend-makebutton2.gif)

    **[リソース名 (キー)]** で、 **[すべてに適用]** を選択します。  これにより、結果のスタイルとボタンテンプレートが、ボタンであるすべてのオブジェクトに適用されます。 [**定義] で**、 **[アプリケーション]** を選択します。 これにより、結果のスタイルとボタンテンプレートにアプリケーション全体のスコープが設定されます。 これらの2つのボックスの値を設定すると、ボタンのスタイルとテンプレートはアプリケーション全体のすべてのボタンに適用され、アプリケーションで作成したボタンは既定でこのテンプレートを使用します。

## <a name="edit-the-button-template"></a>ボタンテンプレートを編集する

これで、四角形がボタンに変更されました。 このセクションでは、ボタンのテンプレートを変更し、その外観をカスタマイズします。

### <a name="to-edit-the-button-template-to-change-the-button-appearance"></a>ボタンテンプレートを編集してボタンの外観を変更するには

1. **テンプレートビューの編集に進む:** ボタンの外観をさらにカスタマイズするには、ボタンテンプレートを編集する必要があります。 このテンプレートは、四角形をボタンに変換したときに作成されたものです。 ボタンテンプレートを編集するには、ボタンを右クリックし、[**コントロールパーツの編集] (テンプレート)** 、 **[テンプレートの編集]** の順に選択します。

    ![テンプレートを編集する方法](./media/custom-button-blend-edittemplate.jpg)

    テンプレートエディターで、ボタンが <xref:System.Windows.Shapes.Rectangle> と <xref:System.Windows.Controls.ContentPresenter>に分割されていることを確認します。 <xref:System.Windows.Controls.ContentPresenter> は、ボタン内にコンテンツを表示するために使用されます (たとえば、文字列 "Button")。 四角形と <xref:System.Windows.Controls.ContentPresenter> の両方が <xref:System.Windows.Controls.Grid>内にレイアウトされます。

    ![四角形で表したコンポーネント](./media/custom-button-blend-templatepanel.png)

2. **テンプレートコンポーネントの名前を変更します。** テンプレートインベントリ内の四角形を右クリックし、<xref:System.Windows.Shapes.Rectangle> 名を "[四角形]" から "outerRectangle" に変更し、"[ContentPresenter]" を "myContentPresenter" に変更します。

    ![テンプレートのコンポーネント名を変更する方法](./media/custom-button-blend-renamecomponents.png)

3. **(ドーナツのように) 内側に空になるように四角形を変更します。** **OuterRectangle**を選択し、<xref:System.Windows.Shapes.Shape.Fill%2A> を "Transparent" に、<xref:System.Windows.Shapes.Shape.StrokeThickness%2A> を5に設定します。

    ![四角形を空にする方法](./media/custom-button-blend-changerectproperties.png)

    次に、<xref:System.Windows.Shapes.Shape.Stroke%2A> を、テンプレートのすべての色に設定します。 これを行うには、 **[ストローク]** の横にある小さな白いボックスをクリックし、 **[customexpression]** を選択して、ダイアログボックスに「{TemplateBinding Background}」と入力します。

    ![テンプレートの色の使用を設定する方法](./media/custom-button-blend-templatestroke.png)

4. **内部の四角形を作成します。** 次に、別の四角形を作成し ("innerRectangle" という名前を指定)、 **outerRectangle**の内部に対称的に配置します。 このような作業では、ズームして、編集領域のボタンのサイズを大きくすることをお勧めします。

    > [!NOTE]
    > 四角形は、図の場合とは異なる場合があります (たとえば、角が丸くなる場合があります)。

    ![別の四角形の中に四角形を作成する方法](./media/custom-button-blend-innerrectangleproperties.png)

5. **ContentPresenter を一番上に移動します。** この時点で、テキスト "Button" が表示されなくなる可能性があります。 これが原因である場合、これは、 **Innerrectangle**が**myContentPresenter**の上にあるためです。 これを修正するには、 **myContentPresenter**を**innerrectangle**の下にドラッグします。 四角形と**myContentPresenter**の位置を変更して、次のようにします。

    > [!NOTE]
    > または、 **myContentPresenter**を右クリックして **[Send Forward]** をクリックして、上に配置することもできます。

    ![別のボタンの上にボタンを移動する方法](./media/custom-button-blend-innerrectangle2.png)

6. **InnerRectangle の外観を変更します。** <xref:System.Windows.Shapes.Rectangle.RadiusX%2A>、<xref:System.Windows.Shapes.Rectangle.RadiusY%2A>、および <xref:System.Windows.Shapes.Shape.StrokeThickness%2A> の値を20に設定します。 さらに、カスタム式 "{TemplateBinding Background}" を使用して、テンプレートの背景に <xref:System.Windows.Shapes.Shape.Fill%2A> を設定し、<xref:System.Windows.Shapes.Shape.Stroke%2A> を "transparent" に設定します。 **Innerrectangle**の <xref:System.Windows.Shapes.Shape.Fill%2A> と <xref:System.Windows.Shapes.Shape.Stroke%2A> の設定は、 **outerRectangle**の設定とは逆になっていることに注意してください。

    ![四角形の外観を変更する方法](./media/custom-button-blend-glassrectangleproperties1.png)

7. **グラスレイヤーを上に追加します。** ボタンの外観をカスタマイズする最後の部分は、ガラスレイヤーを上部に追加することです。 このグラスレイヤーは、3番目の四角形で構成されています。 グラスではボタン全体が表示されるので、ガラスの四角形は**outerRectangle**のような大きさに似ています。 そのため、 **outerRectangle**のコピーを作成するだけで、四角形を作成できます。 **OuterRectangle**を強調表示し、Ctrl + C キーと Ctrl + V キーを使用してコピーを作成します。 この新しい四角形に "glassCube" という名前を指定します。

8. **必要に応じて glassCube の位置を変更します。** **GlassCube**がボタン全体をカバーするように配置されていない場合は、その位置にドラッグします。

9. **GlassCube は、outerRectangle とは少し異なる形で指定します。** **GlassCube**のプロパティを変更します。 まず、<xref:System.Windows.Shapes.Rectangle.RadiusX%2A> と <xref:System.Windows.Shapes.Rectangle.RadiusY%2A> のプロパティを10に変更し、<xref:System.Windows.Shapes.Shape.StrokeThickness%2A> を2に変更します。

    ![glassCube の外観設定](./media/custom-button-blend-glasscubeappearance.gif)

10. **GlassCube をガラスのように表示します。** 透明度が75% である線状グラデーションを使用して、<xref:System.Windows.Shapes.Shape.Fill%2A> を glassy に設定します。線形グラデーションの色は白で、透明度は約均等に6分間隔で透明です。 次に、グラデーションの分岐点を設定する方法を示します。

    - グラデーションの分岐点 1: アルファ値が75% の白

    - グラデーションの分岐点 2: 透明

    - グラデーションの分岐点 3: アルファ値が75% の白

    - グラデーションの分岐点 4: 透明

    - グラデーションの分岐点 5: アルファ値が75% の白

    - グラデーションの分岐点 6: 透明

    これにより、"波" グラスが作成されます。

    ![グラスのような四角形](./media/custom-button-blend-glassrectangleproperties2.png)

11. **グラスレイヤーを非表示にします。** Glassy レイヤーがどのように見えるかを確認したので、[**プロパティ] パネル**の [**外観] ウィンドウ**に戻り、不透明度を0% に設定して非表示にします。 前のセクションでは、プロパティトリガーとイベントを使用して、グラスレイヤーを表示および操作します。

    ![グラス四角形を非表示にする方法](./media/custom-button-glassrectangleproperties3.gif)

## <a name="customize-the-button-behavior"></a>ボタンの動作をカスタマイズする

この時点では、テンプレートを編集してボタンの表示をカスタマイズしていますが、ボタンは通常のボタンとは異なります (たとえば、マウスオーバーでの外観の変更、フォーカスの受け取り、クリックなど)。次の2つの手順では、これらのような動作をカスタムボタンにビルドする方法を示します。 単純なプロパティトリガーから始めて、イベントトリガーとアニメーションを追加します。

### <a name="to-set-property-triggers"></a>プロパティトリガーを設定するには

1. **新しいプロパティトリガーを作成します。** **GlassCube**を選択した状態で、 **[トリガー]** パネルの **[+ プロパティ]** をクリックします (次の手順に続く図を参照してください)。 これにより、既定のプロパティトリガーを持つプロパティトリガーが作成されます。

2. **トリガーによって使用されるプロパティを次のように設定します。** プロパティを <xref:System.Windows.UIElement.IsMouseOver%2A>に変更します。 これにより、<xref:System.Windows.UIElement.IsMouseOver%2A> プロパティが `true` 場合 (ユーザーがマウスでボタンをポイントしたとき) に、プロパティトリガーがアクティブになります。

    ![プロパティでトリガーを設定する方法](./media/custom-button-blend-ismousedoverpropertytrigger.png)

3. **GlassCube の ismouseover トリガーの100% の不透明度。** **トリガーの記録がオンになっ**ていることに注意してください (前の図を参照してください)。 つまり、記録中に**glassCube**のプロパティ値に加えた変更は、<xref:System.Windows.UIElement.IsMouseOver%2A> が `true`たときに実行されるアクションになります。 記録中に、 **glassCube**の <xref:System.Windows.UIElement.Opacity%2A> を100% に変更します。

    ![ボタンの不透明度を設定する方法](./media/custom-button-blend-ismousedoverpropertytrigger2.gif)

    これで、最初のプロパティトリガーが作成されました。 エディターの [**トリガー] パネル**に、100% に変更された <xref:System.Windows.UIElement.Opacity%2A> が記録されていることを確認します。

    ![[トリガー] パネル](./media/custom-button-blend-propertytriggerinfo.png)

    F5 キーを押してアプリケーションを実行し、マウスポインターをボタンの上または下に移動します。 ボタンをマウスでポイントするとグラスレイヤーが表示され、ポインターが離れると消えます。

4. **Ismouseover トリガーのストローク値の変更:** その他のアクションを <xref:System.Windows.UIElement.IsMouseOver%2A> トリガーに関連付けてみましょう。 記録が続行されたら、選択内容を**glassCube**から**outerRectangle**に切り替えます。 次に、 **outerRectangle**の <xref:System.Windows.Shapes.Shape.Stroke%2A> を "{dynamicresource {HighlightBrushKey}}" のカスタム式に設定します。 これにより、<xref:System.Windows.Shapes.Shape.Stroke%2A> がボタンによって使用される一般的な強調表示色に設定されます。 F5 キーを押して、ボタンの上にマウスを置いたときの効果を確認します。

    ![ストロークを強調表示色に設定する方法](./media/custom-button-blend-ismousedoverpropertytrigger3.png)

5. **Ismouseover トリガーがぼやけたテキスト:** 1つ以上のアクションを <xref:System.Windows.UIElement.IsMouseOver%2A> プロパティトリガーに関連付けてみましょう。 ボタンの上にグラスが表示されているときに、ボタンの内容が少しぼやけて表示されるようにします。 これを行うには、<xref:System.Windows.Controls.ContentPresenter> (**myContentPresenter**) にぼかし <xref:System.Windows.Media.Effects.BitmapEffect> を適用します。

    ![ボタンの内容をぼかす方法](./media/custom-button-blend-propertytriggerwithbitmapeffect.png)

    > [!NOTE]
    > [**プロパティ] パネル**を <xref:System.Windows.Media.Effects.BitmapEffect>検索を実行する前の値に戻すには、**検索ボックス**のテキストをクリアします。

    この時点で、いくつかの関連するアクションを含むプロパティトリガーを使用して、マウスポインターがボタン領域に出入りするときの強調表示動作を作成しました。 ボタンのもう1つの一般的な動作は、フォーカスがあるときに強調表示されます (クリックされた後)。 このような動作を追加するには、<xref:System.Windows.UIElement.IsFocused%2A> プロパティに別のプロパティトリガーを追加します。

6. **IsFocused のプロパティトリガーを作成します。** <xref:System.Windows.UIElement.IsMouseOver%2A> の場合と同じ手順 (このセクションの最初の手順を参照) を使用して、<xref:System.Windows.UIElement.IsFocused%2A> プロパティのプロパティトリガーをもう1つ作成します。 **トリガーの記録がオンになって**いる間に、次のアクションをトリガーに追加します。

    - **glassCube**は100% の <xref:System.Windows.UIElement.Opacity%2A> を取得します。

    - **outerRectangle**は、"{dynamicresource {HighlightBrushKey}}" という <xref:System.Windows.Shapes.Shape.Stroke%2A> カスタム式の値を取得します。

このチュートリアルの最後の手順として、アニメーションをボタンに追加します。 これらのアニメーションは、イベント (具体的には、<xref:System.Windows.UIElement.MouseEnter> イベントと <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントによってトリガーされます。

### <a name="to-use-event-triggers-and-animations-to-add-interactivity"></a>イベントトリガーとアニメーションを使用して対話機能を追加するには

1. **MouseEnter イベントトリガーを作成します。** 新しいイベントトリガーを追加し、トリガーで使用するイベントとして [<xref:System.Windows.UIElement.MouseEnter>] を選択します。

     ![MouseEnter イベント トリガーを作成する方法](./media/custom-button-blend-mouseovereventtrigger.png)

2. **アニメーションタイムラインを作成します。** 次に、アニメーションタイムラインを <xref:System.Windows.UIElement.MouseEnter> イベントに関連付けます。

    ![アニメーション タイムラインをイベントに追加する方法](./media/custom-button-blend-mouseovereventtrigger2.png)

    **[OK]** をクリックして新しいタイムラインを作成すると、[タイムライン **] パネル**が表示され、[タイムラインの記録がオンになっています] がデザインパネルに表示されます。 これは、タイムラインでプロパティの変更の記録を開始できることを意味します (プロパティの変更をアニメーション化します)。

    > [!NOTE]
    > 画面を表示するには、ウィンドウやパネルのサイズを変更する必要がある場合があります。

    ![タイムライン パネル](./media/custom-button-blend-mouseovereventtrigger3.png)

3. **キーフレームを作成します。** アニメーションを作成するには、アニメーション化するオブジェクトを選択し、タイムライン上に複数のキーフレームを作成します。これらのキーフレームについては、アニメーションで補間するプロパティ値を設定します。 次の図は、キーフレームを作成する手順を示しています。

    ![キーフレームを作成する方法](./media/custom-button-blend-mouseovereventtrigger4.png)

4. **このキーフレームで glassCube を圧縮します。** 2番目のキーフレームを選択した状態で、**サイズ変換**を使用して、 **glassCube**のサイズを完全なサイズの90% に縮小します。

    ![ボタンのサイズを縮小する方法](./media/custom-button-blend-sizetransform.png)

    F5 キーを押してアプリケーションを実行します。 マウスポインターをボタンの上に移動します。 グラスレイヤーがボタンの上に縮小されることに注意してください。

5. **別のイベントトリガーを作成し、別のアニメーションを関連付けます。** もう1つアニメーションを追加してみましょう。 前のイベントトリガーアニメーションの作成に使用したものと同様の手順を使用します。

    1. <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントを使用して、新しいイベントトリガーを作成します。

    2. 新しいタイムラインを <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントに関連付けます。

        ![新しいタイムラインを作成する方法](./media/custom-button-blend-clickeventtrigger1.png)

    3. このタイムラインでは、2つのキーフレームを作成します。1つは0.0 秒、2番目は0.3 秒です。

    4. キーフレームが0.3 秒で強調表示されている状態で、**回転変換の角度**を360度に設定します。

        ![回転変換を作成する方法](./media/custom-button-blend-rotatetransform.gif)

    5. F5 キーを押してアプリケーションを実行します。 ボタンをクリックします。 グラスレイヤーが回転していることに注意してください。

## <a name="conclusion"></a>結論

カスタマイズしたボタンを完了しました。 これを行うには、アプリケーションのすべてのボタンに適用されたボタンテンプレートを使用します。 テンプレート編集モードをそのままにした場合 (次の図を参照してください)、さらにボタンを作成すると、既定のボタンのようにではなく、カスタムボタンのような外観と動作が表示されます。

![カスタムボタンテンプレート](./media/custom-button-blend-scopeup.gif)

![同じテンプレートを使用する複数のボタン](./media/custom-button-blend-createmultiplebuttons.png)

F5 キーを押してアプリケーションを実行します。 ボタンをクリックして、すべての動作が同じであることを確認します。

テンプレートをカスタマイズしている間は、 **Innerrectangle**の <xref:System.Windows.Shapes.Shape.Fill%2A> プロパティと <xref:System.Windows.Shapes.Shape.Stroke%2A> プロパティ**outerRectangle**をテンプレートの背景 ({TemplateBinding background}) に設定します。 このため、個々のボタンの背景色を設定すると、設定した背景がそれぞれのプロパティに使用されます。 今すぐ背景を変更してみてください。 次の図では、さまざまなグラデーションが使用されています。 したがって、テンプレートはボタンのようなコントロールを全体的にカスタマイズするのに役立ちますが、テンプレートを持つコントロールは、互いに異なる外観に変更される可能性があります。

![外観のような同じテンプレートを持つボタン](./media/custom-button-blend-blendconclusion.jpg "custom_button_blend_BlendConclusion")

結論として、ボタンテンプレートをカスタマイズするプロセスでは、Microsoft Expression Blend で次の操作を行う方法を学習しました。

- コントロールの外観をカスタマイズします。

- プロパティトリガーを設定します。 プロパティトリガーは、コントロールだけでなく、ほとんどのオブジェクトで使用できるため、非常に便利です。

- イベントトリガーを設定します。 イベントトリガーは、コントロールだけでなく、ほとんどのオブジェクトで使用できるため、非常に便利です。

- アニメーションを作成します。

- その他: グラデーションを作成し、BitmapEffects を追加し、変換を使用して、オブジェクトの基本プロパティを設定します。

## <a name="see-also"></a>関連項目

- [XAML を使用したボタンの作成](walkthrough-create-a-button-by-using-xaml.md)
- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [アニメーションの概要](../graphics-multimedia/animation-overview.md)
- [純色およびグラデーションによる塗りつぶしの概要](../graphics-multimedia/painting-with-solid-colors-and-gradients-overview.md)
- [ビットマップ効果の概要](../graphics-multimedia/bitmap-effects-overview.md)
