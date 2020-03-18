---
title: 'チュートリアル : XAML を使用したボタンの作成'
ms.date: 03/30/2017
helpviewer_keywords:
- buttons [WPF]
ms.assetid: 138c41c4-1759-4bbf-8d77-77031a06a8a0
ms.openlocfilehash: 6738b9e66c1223ea4ec50c070a421d119fd30bc4
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458697"
---
# <a name="walkthrough-create-a-button-by-using-xaml"></a>チュートリアル : XAML を使用したボタンの作成

このチュートリアルの目的は、Windows Presentation Foundation (WPF) アプリケーションで使用するためのアニメーションボタンを作成する方法を学習することです。 このチュートリアルでは、スタイルとテンプレートを使用して、コードの再利用とボタンのロジックの分離を可能にするカスタマイズされたボタンリソースを作成します。 このチュートリアルは完全に [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]に記述されています。

> [!IMPORTANT]
> このチュートリアルでは、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] を入力またはコピーして Visual Studio に貼り付けることによってアプリケーションを作成する手順について説明します。 デザイナーを使用して同じアプリケーションを作成する方法については、「 [Microsoft Expression Blend を使用してボタンを作成](walkthrough-create-a-button-by-using-microsoft-expression-blend.md)する」を参照してください。

完成したボタンを次の図に示します。

![XAML を使用して作成されたカスタムボタン](./media/custom-button-animatedbutton-5.gif "custom_button_AnimatedButton_5")

## <a name="create-basic-buttons"></a>基本ボタンを作成する

まず、新しいプロジェクトを作成し、いくつかのボタンをウィンドウに追加します。

### <a name="to-create-a-new-wpf-project-and-add-buttons-to-the-window"></a>新しい WPF プロジェクトを作成し、ウィンドウにボタンを追加するには

1. Visual Studio を起動します。

2. **新しい WPF プロジェクトを作成します。** **[ファイル]** メニューの **[新規作成]** をポイントし、 **[プロジェクト]** をクリックします。 **Windows アプリケーション (WPF)** テンプレートを探し、プロジェクトに "AnimatedButton" という名前を指定します。 これにより、アプリケーションのスケルトンが作成されます。

3. **基本的な既定のボタンを追加します。** このチュートリアルで必要なすべてのファイルは、テンプレートによって提供されます。 ソリューションエクスプローラーでダブルクリックして、Window1.xaml ファイルを開きます。 既定では、Window1.xaml には <xref:System.Windows.Controls.Grid> 要素があります。 <xref:System.Windows.Controls.Grid> 要素を削除し、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] ページにいくつかのボタンを追加します。そのためには、次の強調表示されているコードを Window1.xaml に入力するか、コピーして貼り付けます。

    ```xaml
    <Window x:Class="AnimatedButton.Window1"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      Title="AnimatedButton" Height="300" Width="300"
      Background="Black">
      <!-- Buttons arranged vertically inside a StackPanel. -->
      <StackPanel HorizontalAlignment="Left">
          <Button>Button 1</Button>
          <Button>Button 2</Button>
          <Button>Button 3</Button>
      </StackPanel>
    </Window>
    ```

     F5 キーを押してアプリケーションを実行します。次の図のような一連のボタンが表示されます。

     ![3つの基本的なボタン](./media/custom-button-animatedbutton-1.gif "custom_button_AnimatedButton_1")

     これで基本的なボタンが作成されたので、Window1.xaml ファイルの作業は終了です。 このチュートリアルの残りの部分では、app.xaml ファイルに焦点を当てて、ボタンのスタイルとテンプレートを定義します。

## <a name="set-basic-properties"></a>基本プロパティの設定

次に、ボタンの外観とレイアウトを制御するために、これらのボタンのいくつかのプロパティを設定してみましょう。 ボタンのプロパティを個別に設定するのではなく、リソースを使用して、アプリケーション全体のボタンプロパティを定義します。 アプリケーションリソースは、概念的には Web ページの外部カスケードスタイルシート (CSS) に似ています。ただし、このチュートリアルの最後にわかるように、リソースはカスケードスタイルシート (CSS) よりはるかに強力です。 リソースの詳細については、「 [XAML resources](../advanced/xaml-resources.md)」を参照してください。

### <a name="to-use-styles-to-set-basic-properties-on-the-buttons"></a>スタイルを使用してボタンの基本プロパティを設定するには

1. **Application .resources ブロックを定義します**。App.xaml を開き、次の強調表示されているマークアップを追加します (まだ存在していない場合)。

    ```xaml
    <Application x:Class="AnimatedButton.App"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      StartupUri="Window1.xaml"
      >
      <Application.Resources>
        <!-- Resources for the entire application can be defined here. -->
      </Application.Resources>
    </Application>
    ```

     リソースのスコープは、リソースを定義する場所によって決まります。 App.xaml ファイルの `Application.Resources` にリソースを定義すると、アプリケーション内の任意の場所からリソースを使用できるようになります。 リソースのスコープを定義する方法の詳細については、「 [XAML resources](../advanced/xaml-resources.md)」を参照してください。

2. **スタイルを作成し、それを使用して基本的なプロパティ値を定義します。** 次のマークアップを `Application.Resources` ブロックに追加します。 このマークアップは、アプリケーションのすべてのボタンに適用される <xref:System.Windows.Style> を作成し、ボタンの <xref:System.Windows.FrameworkElement.Width%2A> を90に、<xref:System.Windows.FrameworkElement.Margin%2A> を10に設定します。

    ```xaml
    <Application.Resources>
      <Style TargetType="Button">
        <Setter Property="Width" Value="90" />
        <Setter Property="Margin" Value="10" />
      </Style>
    </Application.Resources>
    ```

     <xref:System.Windows.Style.TargetType%2A> プロパティは、スタイルが <xref:System.Windows.Controls.Button>型のすべてのオブジェクトに適用されることを指定します。 各 <xref:System.Windows.Setter> は、<xref:System.Windows.Style>に対して異なるプロパティ値を設定します。 そのため、この時点では、アプリケーションの各ボタンの幅は90、余白は10です。  F5 キーを押してアプリケーションを実行すると、次のウィンドウが表示されます。

     ![幅が90、余白が10のボタン](./media/custom-button-animatedbutton-2.gif "custom_button_AnimatedButton_2")

     スタイルを使用すると、さまざまな方法で、対象となるオブジェクトを細かく調整したり、複雑なプロパティ値を指定したり、他のスタイルの入力としてスタイルを使用したりできます。 詳しくは、「 [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)」をご覧ください。

3. **リソースにスタイルプロパティ値を設定します。** リソースを使用すると、一般的に定義されているオブジェクトと値を簡単に再利用できます。 特に、コードのモジュール性を高めるために、リソースを使用して複雑な値を定義すると便利です。 次の強調表示されたマークアップを app.xaml に追加します。

    ```xaml
    <Application.Resources>
      <LinearGradientBrush x:Key="GrayBlueGradientBrush" StartPoint="0,0" EndPoint="1,1">
        <GradientStop Color="DarkGray" Offset="0" />
        <GradientStop Color="#CCCCFF" Offset="0.5" />
        <GradientStop Color="DarkGray" Offset="1" />
      </LinearGradientBrush>
      <Style TargetType="{x:Type Button}">
        <Setter Property="Background" Value="{StaticResource GrayBlueGradientBrush}" />
        <Setter Property="Width" Value="80" />
        <Setter Property="Margin" Value="10" />
      </Style>
    </Application.Resources>
    ```

     `Application.Resources` ブロックのすぐ下には、"GrayBlueGradientBrush" という名前のリソースが作成されています。 このリソースは、水平方向のグラデーションを定義します。 このリソースは、アプリケーション内の任意の場所からのプロパティ値として使用できます。これには、<xref:System.Windows.Controls.Control.Background%2A> プロパティのボタンスタイルセッターの内部を含みます。 これで、すべてのボタンにこのグラデーションの <xref:System.Windows.Controls.Control.Background%2A> プロパティ値が設定されました。

     F5 キーを押してアプリケーションを実行します。 これは次のようになります。

     ![グラデーションの背景を持つボタン](./media/custom-button-animatedbutton-3.gif "custom_button_AnimatedButton_3")

## <a name="create-a-template-that-defines-the-look-of-the-button"></a>ボタンの外観を定義するテンプレートを作成する

このセクションでは、ボタンの外観 (プレゼンテーション) をカスタマイズするテンプレートを作成します。 ボタンの表示は、四角形やその他のコンポーネントを含む複数のオブジェクトで構成されています。これにより、ボタンに固有の外観が提供されます。

これまでのところ、アプリケーションでのボタンの外観の制御は、ボタンのプロパティの変更に限定されています。 ボタンの外観により大きな変更を加える場合はどうすればよいでしょうか。 テンプレートを使用すると、オブジェクトのプレゼンテーションを強力に制御できます。 テンプレートはスタイル内で使用できるため、スタイルが適用されるすべてのオブジェクト (このチュートリアルではボタン) にテンプレートを適用できます。

### <a name="to-use-the-template-to-define-the-look-of-the-button"></a>テンプレートを使用してボタンの外観を定義するには

1. **テンプレートを設定します。** <xref:System.Windows.Controls.Button> のようなコントロールには <xref:System.Windows.Controls.Control.Template%2A> のプロパティがあるため、<xref:System.Windows.Setter>を使用して <xref:System.Windows.Style> に設定した他のプロパティ値と同じように、テンプレートプロパティの値を定義できます。 次の強調表示されたマークアップをボタンのスタイルに追加します。

    ```xaml
    <Application.Resources>
      <LinearGradientBrush x:Key="GrayBlueGradientBrush"
        StartPoint="0,0" EndPoint="1,1">
        <GradientStop Color="DarkGray" Offset="0" />
        <GradientStop Color="#CCCCFF" Offset="0.5" />
        <GradientStop Color="DarkGray" Offset="1" />
      </LinearGradientBrush>
      <Style TargetType="{x:Type Button}">
        <Setter Property="Background" Value="{StaticResource GrayBlueGradientBrush}" />
        <Setter Property="Width" Value="80" />
        <Setter Property="Margin" Value="10" />
        <Setter Property="Template">
          <Setter.Value>
            <!-- The button template is defined here. -->
          </Setter.Value>
        </Setter>
      </Style>
    </Application.Resources>
    ```

2. **Alter button プレゼンテーション:** この時点で、テンプレートを定義する必要があります。 次の強調表示されたマークアップを追加します。 このマークアップは、角が丸い2つの <xref:System.Windows.Shapes.Rectangle> 要素と、その後に <xref:System.Windows.Controls.DockPanel>を指定します。 <xref:System.Windows.Controls.DockPanel> は、ボタンの <xref:System.Windows.Controls.ContentPresenter> をホストするために使用されます。 <xref:System.Windows.Controls.ContentPresenter> には、ボタンの内容が表示されます。 このチュートリアルでは、コンテンツはテキスト ("Button 1"、"Button 2"、"Button 3") です。 すべてのテンプレートコンポーネント (四角形と <xref:System.Windows.Controls.DockPanel>) は、<xref:System.Windows.Controls.Grid>内にレイアウトされます。

    ```xaml
    <Setter.Value>
      <ControlTemplate TargetType="Button">
        <Grid Width="{TemplateBinding Width}" Height="{TemplateBinding Height}" ClipToBounds="True">
          <!-- Outer Rectangle with rounded corners. -->
          <Rectangle x:Name="outerRectangle" HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Stroke="{TemplateBinding Background}" RadiusX="20" RadiusY="20" StrokeThickness="5" Fill="Transparent" />
          <!-- Inner Rectangle with rounded corners. -->
          <Rectangle x:Name="innerRectangle" HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Stroke="Transparent" StrokeThickness="20" Fill="{TemplateBinding Background}" RadiusX="20" RadiusY="20" />
          <!-- Present Content (text) of the button. -->
          <DockPanel Name="myContentPresenterDockPanel">
            <ContentPresenter x:Name="myContentPresenter" Margin="20" Content="{TemplateBinding  Content}" TextBlock.Foreground="Black" />
          </DockPanel>
        </Grid>
      </ControlTemplate>
    </Setter.Value>
    ```

     F5 キーを押してアプリケーションを実行します。 これは次のようになります。

     ![3個のボタンがあるウィンドウ](./media/custom-button-animatedbutton-4.gif)

3. **テンプレートに glasseffect を追加します。** 次に、グラスを追加します。 まず、ガラスのグラデーション効果を作成するいくつかのリソースを作成します。 次のグラデーションリソースを `Application.Resources` ブロック内の任意の場所に追加します。

    ```xaml
    <Application.Resources>
      <GradientStopCollection x:Key="MyGlassGradientStopsResource">
        <GradientStop Color="WhiteSmoke" Offset="0.2" />
        <GradientStop Color="Transparent" Offset="0.4" />
        <GradientStop Color="WhiteSmoke" Offset="0.5" />
        <GradientStop Color="Transparent" Offset="0.75" />
        <GradientStop Color="WhiteSmoke" Offset="0.9" />
        <GradientStop Color="Transparent" Offset="1" />
      </GradientStopCollection>
      <LinearGradientBrush x:Key="MyGlassBrushResource"
        StartPoint="0,0" EndPoint="1,1" Opacity="0.75"
        GradientStops="{StaticResource MyGlassGradientStopsResource}" />
    <!-- Styles and other resources below here. -->
    ```

     これらのリソースは、ボタンテンプレートの <xref:System.Windows.Controls.Grid> に挿入する四角形の <xref:System.Windows.Shapes.Shape.Fill%2A> として使用されます。 次の強調表示されたマークアップをテンプレートに追加します。

    ```xaml
    <Setter.Value>
      <ControlTemplate TargetType="{x:Type Button}">
        <Grid Width="{TemplateBinding Width}" Height="{TemplateBinding Height}"
          ClipToBounds="True">

        <!-- Outer Rectangle with rounded corners. -->
        <Rectangle x:Name="outerRectangle" HorizontalAlignment="Stretch"
          VerticalAlignment="Stretch" Stroke="{TemplateBinding Background}"
          RadiusX="20" RadiusY="20" StrokeThickness="5" Fill="Transparent" />

        <!-- Inner Rectangle with rounded corners. -->
        <Rectangle x:Name="innerRectangle" HorizontalAlignment="Stretch"
          VerticalAlignment="Stretch" Stroke="Transparent" StrokeThickness="20"
          Fill="{TemplateBinding Background}" RadiusX="20" RadiusY="20" />

        <!-- Glass Rectangle -->
        <Rectangle x:Name="glassCube" HorizontalAlignment="Stretch"
          VerticalAlignment="Stretch"
          StrokeThickness="2" RadiusX="10" RadiusY="10" Opacity="0"
          Fill="{StaticResource MyGlassBrushResource}"
          RenderTransformOrigin="0.5,0.5">
          <Rectangle.Stroke>
            <LinearGradientBrush StartPoint="0.5,0" EndPoint="0.5,1">
              <LinearGradientBrush.GradientStops>
                <GradientStop Offset="0.0" Color="LightBlue" />
                <GradientStop Offset="1.0" Color="Gray" />
              </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
          </Rectangle.Stroke>
          <!-- These transforms have no effect as they are declared here.
          The reason the transforms are included is to be targets
          for animation (see later). -->
          <Rectangle.RenderTransform>
            <TransformGroup>
              <ScaleTransform />
              <RotateTransform />
            </TransformGroup>
          </Rectangle.RenderTransform>
          <!-- A BevelBitmapEffect is applied to give the button a "Beveled" look. -->
          <Rectangle.BitmapEffect>
            <BevelBitmapEffect />
          </Rectangle.BitmapEffect>
        </Rectangle>

        <!-- Present Text of the button. -->
        <DockPanel Name="myContentPresenterDockPanel">
          <ContentPresenter x:Name="myContentPresenter" Margin="20"
            Content="{TemplateBinding  Content}" TextBlock.Foreground="Black" />
        </DockPanel>
      </Grid>
    </ControlTemplate>
    </Setter.Value>
    ```

     `x:Name` プロパティが "glassCube" の四角形の <xref:System.Windows.UIElement.Opacity%2A> が0であることに注意してください。そのため、サンプルを実行すると、グラス四角形が上に重なって表示されません。 これは、ユーザーがボタンを操作するときに、後でテンプレートにトリガーを追加するためです。 ただし、<xref:System.Windows.UIElement.Opacity%2A> の値を1に変更してアプリケーションを実行すると、ボタンがどのように表示されるかを確認できます。 次の図を参照してください。 次の手順に進む前に、<xref:System.Windows.UIElement.Opacity%2A> を0に戻します。

     ![XAML を使用して作成されたカスタムボタン](./media/custom-button-animatedbutton-5.gif "custom_button_AnimatedButton_5")

## <a name="create-button-interactivity"></a>ボタンの対話機能の作成

このセクションでは、プロパティトリガーとイベントトリガーを作成して、プロパティ値を変更し、マウスポインターをボタンの上に移動したり、をクリックしたりするなど、ユーザーの操作に応じてアニメーションを実行します。

対話機能を追加する簡単な方法としては、テンプレートまたはスタイル内でトリガーを定義する方法があります。 <xref:System.Windows.Trigger>を作成するには、プロパティ "condition" を定義します。たとえば、ボタン <xref:System.Windows.UIElement.IsMouseOver%2A> プロパティ値は `true`に等しくなります。 次に、トリガー条件が true のときに実行される setter (アクション) を定義します。

### <a name="to-create-button-interactivity"></a>ボタンのインタラクティビティを作成するには

1. **テンプレートトリガーの追加:** 強調表示されたマークアップをテンプレートに追加します。

    ```xaml
    <Setter.Value>
      <ControlTemplate TargetType="{x:Type Button}">
        <Grid Width="{TemplateBinding Width}"
          Height="{TemplateBinding Height}" ClipToBounds="True">

          <!-- Outer Rectangle with rounded corners. -->
          <Rectangle x:Name="outerRectangle" HorizontalAlignment="Stretch"
          VerticalAlignment="Stretch" Stroke="{TemplateBinding Background}"
          RadiusX="20" RadiusY="20" StrokeThickness="5" Fill="Transparent" />

          <!-- Inner Rectangle with rounded corners. -->
          <Rectangle x:Name="innerRectangle" HorizontalAlignment="Stretch"
            VerticalAlignment="Stretch" Stroke="Transparent"
            StrokeThickness="20"
            Fill="{TemplateBinding Background}" RadiusX="20" RadiusY="20"
          />

          <!-- Glass Rectangle -->
          <Rectangle x:Name="glassCube" HorizontalAlignment="Stretch"
            VerticalAlignment="Stretch"
            StrokeThickness="2" RadiusX="10" RadiusY="10" Opacity="0"
            Fill="{StaticResource MyGlassBrushResource}"
            RenderTransformOrigin="0.5,0.5">
            <Rectangle.Stroke>
              <LinearGradientBrush StartPoint="0.5,0" EndPoint="0.5,1">
                <LinearGradientBrush.GradientStops>
                  <GradientStop Offset="0.0" Color="LightBlue" />
                  <GradientStop Offset="1.0" Color="Gray" />
                </LinearGradientBrush.GradientStops>
              </LinearGradientBrush>
            </Rectangle.Stroke>

            <!-- These transforms have no effect as they
                 are declared here.
                 The reason the transforms are included is to be targets
                 for animation (see later). -->
            <Rectangle.RenderTransform>
              <TransformGroup>
                <ScaleTransform />
                <RotateTransform />
              </TransformGroup>
            </Rectangle.RenderTransform>

              <!-- A BevelBitmapEffect is applied to give the button a
                   "Beveled" look. -->
            <Rectangle.BitmapEffect>
              <BevelBitmapEffect />
            </Rectangle.BitmapEffect>
          </Rectangle>

          <!-- Present Text of the button. -->
          <DockPanel Name="myContentPresenterDockPanel">
            <ContentPresenter x:Name="myContentPresenter" Margin="20"
              Content="{TemplateBinding  Content}" TextBlock.Foreground="Black" />
          </DockPanel>
        </Grid>

        <ControlTemplate.Triggers>       <!-- Set action triggers for the buttons and define            what the button does in response to those triggers. -->     </ControlTemplate.Triggers>
      </ControlTemplate>
    </Setter.Value>
    ```

2. **プロパティトリガーの追加:** 強調表示されたマークアップを `ControlTemplate.Triggers` ブロックに追加します。

    ```xaml
    <ControlTemplate.Triggers>

      <!-- Set properties when mouse pointer is over the button. -->   <Trigger Property="IsMouseOver" Value="True">     <!-- Below are three property settings that occur when the           condition is met (user mouses over button).  -->     <!-- Change the color of the outer rectangle when user           mouses over it. -->     <Setter Property ="Rectangle.Stroke" TargetName="outerRectangle"       Value="{DynamicResource {x:Static SystemColors.HighlightBrushKey}}" />     <!-- Sets the glass opacity to 1, therefore, the           glass "appears" when user mouses over it. -->     <Setter Property="Rectangle.Opacity" Value="1" TargetName="glassCube" />     <!-- Makes the text slightly blurry as though you           were looking at it through blurry glass. -->     <Setter Property="ContentPresenter.BitmapEffect"        TargetName="myContentPresenter">       <Setter.Value>         <BlurBitmapEffect Radius="1" />       </Setter.Value>     </Setter>   </Trigger>

    <ControlTemplate.Triggers/>
    ```

     F5 キーを押してアプリケーションを実行し、ボタンの上にマウスポインターを置いたときの効果を確認します。

3. **フォーカストリガーを追加します。** 次に、同様の setter を追加して、ボタンにフォーカスがある場合 (ユーザーがクリックした後など) に対処します。

    ```xaml
    <ControlTemplate.Triggers>

      <!-- Set properties when mouse pointer is over the button. -->
      <Trigger Property="IsMouseOver" Value="True">

        <!-- Below are three property settings that occur when the
             condition is met (user mouses over button).  -->
        <!-- Change the color of the outer rectangle when user          mouses over it. -->
        <Setter Property ="Rectangle.Stroke" TargetName="outerRectangle"
          Value="{DynamicResource {x:Static SystemColors.HighlightBrushKey}}" />

        <!-- Sets the glass opacity to 1, therefore, the          glass "appears" when user mouses over it. -->
        <Setter Property="Rectangle.Opacity" Value="1"       TargetName="glassCube" />

        <!-- Makes the text slightly blurry as though you were          looking at it through blurry glass. -->
        <Setter Property="ContentPresenter.BitmapEffect"       TargetName="myContentPresenter">
          <Setter.Value>
            <BlurBitmapEffect Radius="1" />
          </Setter.Value>
        </Setter>
      </Trigger>
      <!-- Set properties when button has focus. -->   <Trigger Property="IsFocused" Value="true">     <Setter Property="Rectangle.Opacity" Value="1"       TargetName="glassCube" />     <Setter Property="Rectangle.Stroke" TargetName="outerRectangle"       Value="{DynamicResource {x:Static SystemColors.HighlightBrushKey}}" />     <Setter Property="Rectangle.Opacity" Value="1" TargetName="glassCube" />   </Trigger>

    </ControlTemplate.Triggers>
    ```

     F5 キーを押してアプリケーションを実行し、ボタンのいずれかをクリックします。 ボタンはフォーカスがあるため、クリックすると強調表示されたままになります。 別のボタンをクリックすると、最後のボタンではフォーカスが失われますが、新しいボタンはフォーカスを取得します。

4. <xref:System.Windows.UIElement.MouseEnter> と <xref:System.Windows.UIElement.MouseLeave>**のアニメーションを追加** **し** **ます。** 次に、いくつかのアニメーションをトリガーに追加します。 `ControlTemplate.Triggers` ブロック内の任意の場所に、次のマークアップを追加します。

    ```xaml
    <!-- Animations that start when mouse enters and leaves button. -->
    <EventTrigger RoutedEvent="Mouse.MouseEnter">
      <EventTrigger.Actions>
        <BeginStoryboard Name="mouseEnterBeginStoryboard">
          <Storyboard>
          <!-- This animation makes the glass rectangle shrink in the X direction. -->
            <DoubleAnimation Storyboard.TargetName="glassCube"
              Storyboard.TargetProperty=
              "(Rectangle.RenderTransform).(TransformGroup.Children)[0].(ScaleTransform.ScaleX)"
              By="-0.1" Duration="0:0:0.5" />
            <!-- This animation makes the glass rectangle shrink in the Y direction. -->
            <DoubleAnimation
            Storyboard.TargetName="glassCube"
              Storyboard.TargetProperty=
              "(Rectangle.RenderTransform).(TransformGroup.Children)[0].(ScaleTransform.ScaleY)"
              By="-0.1" Duration="0:0:0.5" />
          </Storyboard>
        </BeginStoryboard>
      </EventTrigger.Actions>
    </EventTrigger>
    <EventTrigger RoutedEvent="Mouse.MouseLeave">
      <EventTrigger.Actions>
        <!-- Stopping the storyboard sets all animated properties back to default. -->
        <StopStoryboard BeginStoryboardName="mouseEnterBeginStoryboard" />
      </EventTrigger.Actions>
    </EventTrigger>
    ```

     マウスポインターをボタンの上に移動するとグラス四角形が縮小され、ポインターが離れると通常のサイズに戻ります。

     ポインターがボタンの上に移動したときにトリガーされるアニメーションが2つあります (<xref:System.Windows.UIElement.MouseEnter> イベントが発生します)。 これらのアニメーションは、X 軸と Y 軸に沿ってグラス四角形を縮小します。 <xref:System.Windows.Media.Animation.DoubleAnimation> 要素のプロパティ (<xref:System.Windows.Media.Animation.Timeline.Duration%2A> と <xref:System.Windows.Media.Animation.DoubleAnimation.By%2A>に注意してください。 <xref:System.Windows.Media.Animation.Timeline.Duration%2A> は、アニメーションが0.5 秒を超えて実行されることを指定し、<xref:System.Windows.Media.Animation.DoubleAnimation.By%2A> は、ガラスが10% 削減されることを指定します。

     2つ目のイベントトリガー (<xref:System.Windows.UIElement.MouseLeave>) は、最初のトリガーを停止するだけです。 <xref:System.Windows.Media.Animation.Storyboard>を停止すると、アニメーション化されたすべてのプロパティが既定値に戻ります。 このため、ユーザーがポインターをボタンの外に移動すると、ボタンは、マウスポインターがボタンの上に移動する前の状態に戻ります。 アニメーションの詳細については、「[アニメーションの概要](../graphics-multimedia/animation-overview.md)」を参照してください。

5. **ボタンがクリックされたときのアニメーションを追加します。** 最後の手順では、ユーザーがボタンをクリックしたときのトリガーを追加します。 `ControlTemplate.Triggers` ブロック内の任意の場所に、次のマークアップを追加します。

    ```xaml
    <!-- Animation fires when button is clicked, causing glass to spin.  -->
    <EventTrigger RoutedEvent="Button.Click">
      <EventTrigger.Actions>
        <BeginStoryboard>
          <Storyboard>
            <DoubleAnimation Storyboard.TargetName="glassCube"
              Storyboard.TargetProperty=
              "(Rectangle.RenderTransform).(TransformGroup.Children)[1].(RotateTransform.Angle)"
              By="360" Duration="0:0:0.5" />
          </Storyboard>
        </BeginStoryboard>
      </EventTrigger.Actions>
    </EventTrigger>
    ```

     F5 キーを押してアプリケーションを実行し、ボタンのいずれかをクリックします。 ボタンをクリックすると、グラス四角形が回転します。

## <a name="summary"></a>まとめ
 このチュートリアルでは、次の演習を実行しました。

- オブジェクトの種類 (<xref:System.Windows.Controls.Button>) への <xref:System.Windows.Style> を対象としています。

- <xref:System.Windows.Style>を使用して、アプリケーション全体のボタンの基本プロパティを制御します。

- <xref:System.Windows.Style> setter のプロパティ値に使用する、グラデーションなどのリソースを作成しました。

- ボタンにテンプレートを適用することによって、アプリケーション全体のボタンの外観をカスタマイズします。

- アニメーション効果を含むユーザーの操作 (<xref:System.Windows.UIElement.MouseEnter>、<xref:System.Windows.UIElement.MouseLeave>、<xref:System.Windows.Controls.Primitives.ButtonBase.Click>など) に応じたボタンのカスタマイズされた動作。

## <a name="see-also"></a>関連項目

- [Microsoft Expression Blend を使用してボタンを作成する](walkthrough-create-a-button-by-using-microsoft-expression-blend.md)
- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [アニメーションの概要](../graphics-multimedia/animation-overview.md)
- [純色およびグラデーションによる塗りつぶしの概要](../graphics-multimedia/painting-with-solid-colors-and-gradients-overview.md)
- [ビットマップ効果の概要](../graphics-multimedia/bitmap-effects-overview.md)
