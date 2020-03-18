---
title: WPF の概要
titleSuffix: ''
ms.date: 11/04/2016
ms.topic: conceptual
ms.assetid: b8d7cf43-d1f2-4f3d-adb0-4f3a6428edc0
dev_langs:
- csharp
- vb
ms.openlocfilehash: ecdd3b3c24b71917efb0d982d1f23737673622f9
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744708"
---
# <a name="wpf-overview"></a>WPF の概要

Windows Presentation Foundation (WPF) を使用して、視覚的に美しいユーザー エクスペリエンスを持つ Windows 用のデスクトップ クライアント アプリケーションを作成できます。

![Contoso Healthcare UI のサンプル](media/introduction-to-wpf/wpfintrofigure24.png)

WPF の中核を成すのは、解像度に依存しない、ベクター ベースのレンダリング エンジンであり、これは最新のグラフィックス ハードウェアを活用できるように構築されています。 この中核を拡張するため、WPF では、Extensible Application Markup Language (XAML)、コントロール、データ バインディング、レイアウト、2D および 3D グラフィックス、アニメーション、スタイル、テンプレート、ドキュメント、メディア、テキスト、タイポグラフィなどの、アプリケーション開発機能の包括的なセットを使用します。 WPF は .NET の一部です。そのため、.NET API の他の要素を組み込むアプリケーションを構築できます。

この概要は初めての方を対象としており、WPF の主要な機能と概念を説明します。

## <a name="program-with-wpf"></a>WPF でのプログラミング

WPF は、ほとんどの部分が <xref:System.Windows> 名前空間に格納されている .NET 型のサブセットとして存在します。 ASP.NET や Windows フォームのようなマネージド テクノロジを使用して .NET で以前にアプリケーションを構築したことがあるユーザーは、基本的な WPF のプログラミングの経験には慣れているはずです。クラスのインスタンス化、プロパティの設定、メソッドの呼び出し、イベントの処理は C# または Visual Basic などの使い慣れた .NET プログラミング言語を使用して行うことができます。

WPF には、プロパティとイベントを拡張する追加のプログラミング構成要素である、 [依存関係プロパティ](advanced/dependency-properties-overview.md) と [ルーティング イベント](advanced/routed-events-overview.md)が含まれています。

## <a name="markup-and-code-behind"></a>マークアップおよび分離コード

WPF では "*マークアップ*" と "*分離コード*" の両方を使用したアプリケーションを開発できます。これは ASP.NET 開発者にとってなじみ深いエクスペリエンスに違いありません。 一般に、アプリケーションの外観を実装するには XAML マークアップを使用し、一方、その動作を実装するには、マネージド プログラミング言語 (分離コード) を使用します。 外観と動作の実装を別々に行うことには、次の利点があります。

- 外観固有のマークアップが動作固有のコードと密接に結び付いていないので、開発と保守のコストが削減されます。

- デザイナーがアプリケーションの外観を実装しているとき同時に、開発者はアプリケーションの動作を実装できるため、開発がより効率的に進みます。

- WPF アプリケーションの[グローバリゼーションとローカリゼーション](advanced/wpf-globalization-and-localization-overview.md) が簡略化します。

### <a name="markup"></a>マークアップ

XAML は、アプリケーションの外観を宣言的に実装する XML ベースのマークアップ言語です。 一般的に、ウィンドウ、ダイアログ ボックス、ページ、ユーザー コントロールの作成と、これらにコントロール、図形、グラフィックスを入れるために使用されます。

次の例では、XAML を使用して、1つのボタンを含むウィンドウの外観を実装しています。

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    Title="Window with Button"
    Width="250" Height="100">

  <!-- Add button to window -->
  <Button Name="button">Click Me!</Button>

</Window>
```

具体的には、この XAML は `Window` エレメントと `Button` エレメントを使用して、ウィンドウとボタンをそれぞれ定義しています。 各エレメントは属性で構成されます。たとえば `Window` エレメントの `Title` 属性はウィンドウのタイトルバーのテキストを指定します。 マークアップで定義されている要素と属性は、実行時に WPF により、WPF クラスのインスタンスに変換されます。 たとえば、 `Window` エレメントは、 <xref:System.Windows.Window> プロパティが <xref:System.Windows.Window.Title%2A> 属性の値である `Title` クラスのインスタンスに変換されます。

次の図は、前の例の XAML で定義されているユーザーインターフェイス (UI) を示しています。

![ボタンを含むウィンドウ](media/introduction-to-wpf/wpfintrofigure10.png)

XAML は XML ベースなので、XAML を使用して作成する UI は [要素ツリー](advanced/trees-in-wpf.md)と呼ばれるネストされた要素の階層で組み立てられます。 要素ツリーは UI を作成し、管理するための論理的かつ直感的な方法を提供します。

### <a name="code-behind"></a>コードビハインド

アプリケーションの主な動作は、ユーザー インタラクションに対して応答する機能を実装することです。これにはイベントの処理 (メニュー、ツールバー、またはボタンをクリックする、など) および応答のビジネス ロジックやデータ アクセス ロジックの呼び出しなどが含まれます。 WPF では、この動作が、マークアップと関連付けられたコードで実装されます。 このタイプのコードは分離コードと呼ばれています。 次の例は、前の例の更新されたマークアップと分離コードを示しています。

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    x:Class="SDKSample.AWindow"
    Title="Window with Button"
    Width="250" Height="100">

  <!-- Add button to window -->
  <Button Name="button" Click="button_Click">Click Me!</Button>

</Window>
```

```csharp
using System.Windows; // Window, RoutedEventArgs, MessageBox 

namespace SDKSample
{
    public partial class AWindow : Window
    {
        public AWindow()
        {
            // InitializeComponent call is required to merge the UI 
            // that is defined in markup with this class, including  
            // setting properties and registering event handlers
            InitializeComponent();
        }

        void button_Click(object sender, RoutedEventArgs e)
        {
            // Show message box when button is clicked.
            MessageBox.Show("Hello, Windows Presentation Foundation!");
        }
    }
}
```

```vb
Namespace SDKSample

    Partial Public Class AWindow
        Inherits System.Windows.Window

        Public Sub New()

            ' InitializeComponent call is required to merge the UI 
            ' that is defined in markup with this class, including  
            ' setting properties and registering event handlers
            InitializeComponent()

        End Sub 

        Private Sub button_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)

            ' Show message box when button is clicked.
            MessageBox.Show("Hello, Windows Presentation Foundation!")

        End Sub 

    End Class 

End Namespace
```

この例では、 <xref:System.Windows.Window> クラスから派生したクラスを分離コードが実装しています。 マークアップを分離コード クラスと関連付けるために `x:Class` 属性が使用されます。 分離コード クラスのコンストラクターから`InitializeComponent` が呼び出されて、マークアップで定義された UI を分離コード クラスとマージします。 (`InitializeComponent` は、アプリケーションがビルドされるときに生成されるため、手動で実装する必要はありません)。`x:Class` と `InitializeComponent` を組み合わせることにより、実装が作成されるたびに正しく初期化されるようになります。 分離コード クラスはボタンの <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントのイベント ハンドラーも実装します。 ボタンがクリックされると、イベント ハンドラーは <xref:System.Windows.MessageBox.Show%2A?displayProperty=fullName> メソッドを呼び出して、メッセージ ボックスを表示します。

次の図は、ボタンがクリックされたときの結果を示しています。

![MessageBox](media/introduction-to-wpf/wpfintrofigure25.png)

## <a name="controls"></a>コントロール

アプリケーション モデルにより提供されるユーザー エクスペリエンスは、構築済みのコントロールです。 WPF において、"*コントロール*" とはウィンドウまたはページによりホストされ、ユーザー インターフェイスを持ち、何らかの動作を実装する WPF クラスのカテゴリに適用される総称です。

詳しくは、「 [コントロール](controls/index.md)」をご覧ください。

### <a name="wpf-controls-by-function"></a>WPF コントロールの機能別一覧

組み込みの WPF コントロールを次に示します。

- **ボタン類**: <xref:System.Windows.Controls.Button> および <xref:System.Windows.Controls.Primitives.RepeatButton>。

- **データ表示**: <xref:System.Windows.Controls.DataGrid>、<xref:System.Windows.Controls.ListView>、および <xref:System.Windows.Controls.TreeView>。

- **日付表示と選択**: <xref:System.Windows.Controls.Calendar> および <xref:System.Windows.Controls.DatePicker>。

- **ダイアログ ボックス**: <xref:Microsoft.Win32.OpenFileDialog>、 <xref:System.Windows.Controls.PrintDialog>、 <xref:Microsoft.Win32.SaveFileDialog>。

- **デジタル インク**: <xref:System.Windows.Controls.InkCanvas> および <xref:System.Windows.Controls.InkPresenter>。

- **ドキュメント**: <xref:System.Windows.Controls.DocumentViewer>、 <xref:System.Windows.Controls.FlowDocumentPageViewer>、 <xref:System.Windows.Controls.FlowDocumentReader>、 <xref:System.Windows.Controls.FlowDocumentScrollViewer>、 <xref:System.Windows.Controls.StickyNoteControl>。

- **入力**: <xref:System.Windows.Controls.TextBox>、 <xref:System.Windows.Controls.RichTextBox>、 <xref:System.Windows.Controls.PasswordBox>。

- **レイアウト**: <xref:System.Windows.Controls.Border>、 <xref:System.Windows.Controls.Primitives.BulletDecorator>、 <xref:System.Windows.Controls.Canvas>、 <xref:System.Windows.Controls.DockPanel>、 <xref:System.Windows.Controls.Expander>、 <xref:System.Windows.Controls.Grid>、 <xref:System.Windows.Controls.GridView>、 <xref:System.Windows.Controls.GridSplitter>、 <xref:System.Windows.Controls.GroupBox>、 <xref:System.Windows.Controls.Panel>、 <xref:System.Windows.Controls.Primitives.ResizeGrip>、 <xref:System.Windows.Controls.Separator>、 <xref:System.Windows.Controls.Primitives.ScrollBar>、 <xref:System.Windows.Controls.ScrollViewer>、 <xref:System.Windows.Controls.StackPanel>、 <xref:System.Windows.Controls.Primitives.Thumb>、 <xref:System.Windows.Controls.Viewbox>、 <xref:System.Windows.Controls.VirtualizingStackPanel>、 <xref:System.Windows.Window>、 <xref:System.Windows.Controls.WrapPanel>。

- **メディア**: <xref:System.Windows.Controls.Image>、 <xref:System.Windows.Controls.MediaElement>、 <xref:System.Windows.Controls.SoundPlayerAction>。

- **メニュー類**: <xref:System.Windows.Controls.ContextMenu>、 <xref:System.Windows.Controls.Menu>、 <xref:System.Windows.Controls.ToolBar>。

- **ナビゲーション**: <xref:System.Windows.Controls.Frame>、 <xref:System.Windows.Documents.Hyperlink>、 <xref:System.Windows.Controls.Page>、 <xref:System.Windows.Navigation.NavigationWindow>、 <xref:System.Windows.Controls.TabControl>。

- **選択**: <xref:System.Windows.Controls.CheckBox>、 <xref:System.Windows.Controls.ComboBox>、 <xref:System.Windows.Controls.ListBox>、 <xref:System.Windows.Controls.RadioButton>、 <xref:System.Windows.Controls.Slider>。

- **ユーザー情報**: <xref:System.Windows.Controls.AccessText>、 <xref:System.Windows.Controls.Label>、 <xref:System.Windows.Controls.Primitives.Popup>、 <xref:System.Windows.Controls.ProgressBar>、 <xref:System.Windows.Controls.Primitives.StatusBar>、 <xref:System.Windows.Controls.TextBlock>、 <xref:System.Windows.Controls.ToolTip>。

## <a name="input-and-commands"></a>入力とコマンド

コントロールはほとんどの場合、ユーザー入力の検出と応答に使用されます。 [WPF 入力システム](advanced/input-overview.md) では直接イベントとルーティング イベントの両方を使用して、テキスト入力、フォーカス管理、マウス位置指定をサポートしています。

アプリケーションにはたいてい、複雑な入力要件があります。 WPF には、ユーザー入力動作と、それらの動作に応答するコードを分離する [コマンド システム](advanced/commanding-overview.md) があります。

## <a name="layout"></a>[レイアウト]

ユーザー インターフェイスを作成すると、場所とサイズによりコントロール類を配置して、レイアウトを決めます。 すべてのレイアウトの重要な要件は、ウィンドウ サイズと表示設定の変更に適応させることです。 こうした状況でレイアウトを適応させるために開発者にコードを作成させるのではなく、WPF では最上級の拡張可能なレイアウト システムを提供します。

レイアウト システムの要となるのは、相対的な位置指定です。これにより、ウィンドウや表示条件の変化への適応性が高まります。 さらに、レイアウト システムは各コントロール間のネゴシエーションを管理して、レイアウトを決定します。 このネゴシエーションは 2 段階のプロセスで行われます。まず、コントロールがその親に対して、必要な場所とサイズを伝えます。次に、親コントロールはどれぐらいの空間を持てるかを子コントロールに伝えます。

レイアウト システムは基本の WPF クラスを介して子コントロールに公開されます。 グリッド、スタック、ドックなどの一般的なレイアウトについて、WPF には複数のレイアウト コントロールが組み込まれています。

- <xref:System.Windows.Controls.Canvas>: 子コントロールは独自のレイアウトを提供します。

- <xref:System.Windows.Controls.DockPanel>: 子コントロールはパネルの縁に並べられます。

- <xref:System.Windows.Controls.Grid>: 子コントロールは行ごと、列ごとに位置指定されます。

- <xref:System.Windows.Controls.StackPanel>: 子コントロールは垂直方向または水平方向に積み上げられます。

- <xref:System.Windows.Controls.VirtualizingStackPanel>: 子コントロールは仮想化され、水平方向または垂直方向の 1 本の線上に配置されます。

- <xref:System.Windows.Controls.WrapPanel>: 子コントロールは左から右への順序に配置され、現在の行に納まらない場合は、次の行に折り返されます。

次の例では、<xref:System.Windows.Controls.DockPanel> を使用して、複数の <xref:System.Windows.Controls.TextBox> コントロールをレイアウトします。

[!code-xaml[IntroToWPFSnippets#LayoutMARKUP](~/samples/snippets/xaml/wpf/introduction-to-wpf/introduction-to-wpf_1.xaml)]

<xref:System.Windows.Controls.DockPanel> により、子 <xref:System.Windows.Controls.TextBox> コントロールはどのように配置するかを伝えることができます。 このために、<xref:System.Windows.Controls.DockPanel> は `Dock` 添付プロパティを実装しています。このプロパティが子コントロールに公開されて、それぞれのコントロールがドック スタイルを指定できるようになります。

> [!NOTE]
> 子コントロールで使用するために親コントロールにより実装されるプロパティは、[添付プロパティ](advanced/attached-properties-overview.md)と呼ばれる WPF 構成要素です。

次の図は、前の例の XAML マークアップの結果を示しています。

![DockPanel ページ](media/introduction-to-wpf/wpfintrofigure11.png)

## <a name="data-binding"></a>データ バインディング

ほとんどのアプリケーションは、データの表示と編集の手段をユーザーに提供するために作成されます。 WPF アプリケーションの場合、データ格納とアクセスの機能は、SQL Server や ADO .NET などのテクノロジにより、すでに提供されています。 データがアクセスされ、アプリケーションの管理対象オブジェクトに読み込まれると、WPF アプリケーションの処理が開始します。 基本的に、これには 2 つの処理が伴います。

1. 管理対象のオブジェクトからコントロールにデータをコピーします。これらのコントロールで、データを表示および編集できます。

2. コントロールを使用してデータに対して行う変更が、必ず管理対象オブジェクトにもう一度コピーされるようにしてください。

アプリケーションの開発を簡素化するために、WPF にはこれらの手順を自動的に実行する、データ バインディング エンジンが用意されています。 データ バインディング エンジンの中核となる単位は <xref:System.Windows.Data.Binding> クラスであり、その仕事はコントロール (バインディング ターゲット) をデータ オブジェクト (バインディング ソース) にバインドすることです。 この関係を次の図に示します。

![基本的なデータ バインディング ダイアグラム](media/introduction-to-wpf/databindingmostbasic.png)

次の例は、<xref:System.Windows.Controls.TextBox> をカスタム `Person` オブジェクトのインスタンスにバインドする方法を示しています。 `Person` の実装を次のコードに示します。

[!code-vb[SimpleDataBindingSnippets#PersonClassCODE](~/samples/snippets/visualbasic/wpf/introduction-to-wpf/introduction-to-wpf_2.vb)]
[!code-csharp[SimpleDataBindingSnippets#PersonClassCODE](~/samples/snippets/csharp/wpf/introduction-to-wpf/introduction-to-wpf_2.cs)]

次のマークアップは、<xref:System.Windows.Controls.TextBox> をカスタム `Person` オブジェクトのインスタンスにバインドします。

```xaml
 <Window
     xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
     xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
     x:Class="SDKSample.DataBindingWindow">

   <!-- Bind the TextBox to the data source (TextBox.Text to Person.Name) -->
   <TextBox Name="personNameTextBox" Text="{Binding Path=Name}" />

 </Window>
```

[!code-vb[SimpleDataBindingSnippets#DataBindingCODEBEHIND](~/samples/snippets/visualbasic/wpf/introduction-to-wpf/introduction-to-wpf_6.vb)]
[!code-csharp[SimpleDataBindingSnippets#DataBindingCODEBEHIND](~/samples/snippets/csharp/wpf/introduction-to-wpf/introduction-to-wpf_6.cs)]

この例で、 `Person` クラスは、分離コードでインスタンス化され、 `DataBindingWindow`のデータ コンテキストとして設定されます。 マークアップでは、 <xref:System.Windows.Controls.TextBox.Text%2A> の <xref:System.Windows.Controls.TextBox> プロパティが、 `Person.Name` プロパティにバインドされます ("`{Binding ... }`" XAML 構文を使用)。 この XAML は WPF に対して、ウィンドウの <xref:System.Windows.Controls.TextBox> プロパティに格納されている `Person` オブジェクトに、 <xref:System.Windows.FrameworkElement.DataContext%2A> コントロールをバインドするよう、指示します。

WPF データ バインディング エンジンは、検証、並べ替え、フィルター処理、グループ化などのその他のサポートを提供します。 さらにデータ バインディングでは、標準の WPF コントロールによって表示されたユーザー インターフェイスが適切ではない場合に、バインドされたデータを操作するためのカスタム ユーザー インターフェイスをデータ テンプレートで作成することをサポートしています。

詳しくは、「[データ バインディングの概要](../../desktop-wpf/data/data-binding-overview.md)」をご覧ください。

## <a name="graphics"></a>グラフィックス

WPF では次の利点を備えた、広範囲にわたるスケーラブルで柔軟なグラフィックス機能セットが導入されています。

- **解像度にもデバイスにも依存しないグラフィックス**。 WPF グラフィックス システムでの測定値の基本単位は、デバイスに依存しないピクセル、すなわち 1 インチの 1/96 であり、実際の画面解像度には関係ありません。このため、解像度にもデバイスにも依存しないレンダリングの基盤が提供されます。 デバイスに依存しない各ピクセルは、レンダリングを行うシステムのドット/インチ (dpi) 設定に合うように、自動的にスケーリングされます。

- **精度の向上**。 WPF の座標系は、単精度ではなく、倍精度浮動小数点数で測定されます。 変換および不透明度の値も倍精度で表現されます。 また、WPF は、広色域 (scRGB) に対応しており、異なる色空間からの入力を管理する統合サポートを提供します。

- **高度なグラフィックスおよびアニメーションのサポート**。 WPF はアニメーションのシーンを管理してグラフィックスのプログラミングを簡略化します。シーンの処理、レンダリング ループ、バイリニア補間について心配する必要はありません。 WPF はさらに、ヒット テストのサポートとアルファ合成の完全なサポートを提供しています。

- **ハードウェアの高速化** WPF グラフィックス システムでは、CPU 使用率を最小限に抑えるためにグラフィックス ハードウェアを利用します。

### <a name="2d-shapes"></a>2D 図形

WPF には、次の図に示す四角形や楕円のような、一般的なベクター描画による 2D 図形のライブラリが用意されています。

![楕円と四角形](media/introduction-to-wpf/wpfintrofigure4.PNG)

図形の興味深い機能は、単に表示するだけのものではないところです。図形はコントロールに期待される多くの機能 (キーボード入力とマウス入力を含む) を実装します。 次の例は、処理される <xref:System.Windows.Shapes.Ellipse> の <xref:System.Windows.UIElement.MouseUp> イベントを示しています。

[!code-xaml[IntroToWPFSnippets#HandleEllipseMouseUpEventMARKUP](~/samples/snippets/xaml/wpf/introduction-to-wpf/introduction-to-wpf_7.xaml)]

[!code-vb[IntroToWPFSnippets#HandleEllipseMouseUpEventCODEBEHIND](~/samples/snippets/visualbasic/wpf/introduction-to-wpf/introduction-to-wpf_8.vb)]
[!code-csharp[IntroToWPFSnippets#HandleEllipseMouseUpEventCODEBEHIND](~/samples/snippets/csharp/wpf/introduction-to-wpf/introduction-to-wpf_8.cs)]

次の図は、上記のコードによって生成される内容を示しています。

!["you clicked the ellipse&#33;" というテキストを含むウィンドウ](media/introduction-to-wpf/wpfintrofigure12.png)

詳しくは、「[WPF での図形と基本描画の概要](../../desktop-wpf/data/data-binding-overview.md)」をご覧ください。

### <a name="2d-geometries"></a>2D ジオメトリ

WPF で提供される 2D 図形では、基本的な図形の標準セットが網羅されています。 ただし、カスタマイズされたユーザー インターフェイスを容易に設計するには、カスタム図形を作成しなければならない場合があります。 このため、WPF ではジオメトリが用意されています。 次の図では、直接描画、ブラシとして使用、または他の図形やコントロールをクリップするために使用できるカスタム図形を作成するジオメトリの使用法を示します。

<xref:System.Windows.Shapes.Path> オブジェクトは、閉じているまたは開いている図形、複数の図形、さらに曲線図形の描画に使用できます。

<xref:System.Windows.Media.Geometry> オブジェクトは、クリップ、ヒット テスト、2D グラフィック データのレンダリングに使用できます。

![パスのさまざまな使用](media/introduction-to-wpf/wpfintrofigure5.png)

詳しくは、「[ジオメトリの概要](graphics-multimedia/geometry-overview.md)」をご覧ください。

### <a name="2d-effects"></a>2D 効果

WPF の 2D 機能のサブセットには、グラデーション、ビットマップ、描画、ビデオによる塗りつぶし、回転、スケーリング、傾斜などの視覚効果が含まれています。 これらはすべて、ブラシを使用して実現されます。次の図は、いくつかの例を示しています。

![さまざまなブラシの図](media/introduction-to-wpf/wpfintrofigure6.png)

詳しくは、「[WPF のブラシの概要](graphics-multimedia/wpf-brushes-overview.md)」をご覧ください。

### <a name="3d-rendering"></a>3D レンダリング

WPF には 2D グラフィックスと統合し、より魅力的で興味深いユーザー インターフェイスを作成できる 3D レンダリング機能も含まれています。 たとえば、次の図は、3D 図形にレンダリングされる2D イメージを示しています。

![Visual3D サンプルのスクリーンショット](media/introduction-to-wpf/wpfintrofigure13.png)

詳細については、「[3D グラフィックスの概要](graphics-multimedia/3-d-graphics-overview.md)」を参照してください。

## <a name="animation"></a>アニメーション

WPF のアニメーション サポートを使用すると、コントロールを拡大、振動、スピン、フェードさせることができ、魅力的なページ遷移などを作成できです。 カスタム クラスも含めて、ほとんどの WPF クラスをアニメーション表示できます。 次の図は、動作中の単純なアニメーションを示しています。

![アニメーション キューブのイメージ](media/introduction-to-wpf/wpfintrofigure7.png)

詳しくは、「[アニメーションの概要](graphics-multimedia/animation-overview.md)」をご覧ください。

## <a name="media"></a>メディア

リッチ コンテンツを伝達するための 1 つの方法は、オーディオビジュアル メディアを使用することです。 WPF では、イメージ、ビデオ、オーディオに対して特別なサポートを提供しています。

### <a name="images"></a>イメージ

イメージは、ほとんどのアプリケーションに共通していますが、WPF にはイメージを使用するための、いくつかの方法が用意されています。 次の図に、サムネイル イメージが含まれているリスト ボックスがある、ユーザー インターフェイスを示します。 サムネイルを選ぶと、そのイメージがフル サイズで表示されます。

![サムネイル イメージとフルサイズのイメージ](media/introduction-to-wpf/wpfintrofigure8.png)

詳しくは、「[イメージングの概要](graphics-multimedia/imaging-overview.md)」をご覧ください。

### <a name="video-and-audio"></a>ビデオとオーディオ

<xref:System.Windows.Controls.MediaElement> コントロールは、オーディオとビデオの両方を再生でき、カスタム メディア プレーヤーの土台となれる柔軟性を備えています。 次の XAML マークアップは、メディアプレーヤーを実装します。

[!code-xaml[IntroToWPFSnippets#MediaElementMARKUP](~/samples/snippets/xaml/wpf/introduction-to-wpf/introduction-to-wpf_9.xaml)]

次の図のウィンドウは、動作中の <xref:System.Windows.Controls.MediaElement> コントロールを示しています。

![オーディオおよびビデオを含む MediaElement コントロール](media/introduction-to-wpf/wpfintrofigure1.png)

詳細については、「[グラフィックスとマルチメディア](graphics-multimedia/index.md)」を参照してください。

## <a name="text-and-typography"></a>テキストとタイポグラフィ

高品質のテキスト レンダリングを容易に行うために、WPF では次の機能が提供されています。

- OpenType フォントのサポート。

- ClearType 機能拡張。

- ハードウェアの高速化を利用する高パフォーマンス。

- テキストと、メディア、グラフィックス、アニメーションとの統合。

- 国際対応フォントのサポートとフォールバック メカニズム。

グラフィックスとのテキストの統合のデモとして、次の図は、文字装飾の適用を示しています。

![さまざまなテキスト装飾を含むテキスト](media/introduction-to-wpf/wpfintrofigure23.png)

詳しくは、「 [Windows Presentation Foundation のタイポグラフィ](advanced/typography-in-wpf.md)」をご覧ください。

## <a name="customize-wpf-apps"></a>WPF アプリをカスタマイズする

ここまで、アプリケーションを開発するための中核となる WPF 構成要素を説明してきました。 主にコントロールから成るアプリケーション コンテンツをホストして提供するには、アプリケーション モデルを使用します。 ユーザー インターフェイスでのコントロールの配置を簡素化して、ウィンドウ サイズや表示設定に変更が発生した場合にも配置を維持するには、WPF レイアウト システムを使用します。 ほとんどのアプリケーションでは、ユーザーがデータと対話できるようになっているため、データ バインディングを使用すればユーザー インターフェイスとデータの統合作業を削減できます。 アプリケーションの外観を向上させるには、WPF が提供する幅広いグラフィックス、アニメーション、メディアのサポートを使用します。

ただし多くの場合、基本要素だけでは、真に個性的で視覚的に美しいユーザー エクスペリエンスを作成し、管理するには不十分です。 標準の WPF コントロールでは、アプリケーションの目的の外観にはそぐわない場合があります。 最も効果的な方法でデータを表示できない可能性があります。 アプリケーションの全体的なユーザー エクスペリエンスが、Windows のテーマの既定のルック アンド フィールに適合しない場合があります。 多くの点で、プレゼンテーション技術には他の種類の機能拡張と同様、視覚的な機能拡張性が必要です。

このため、WPF には独自のユーザー エクスペリエンスを作成するためのさまざまなメカニズムが用意されています。コントロール、トリガー、コントロールとデータのテンプレート、スタイル、ユーザー インターフェイスのリソース、テーマとスキン用の豊富なコンテンツ モデルがあります。

### <a name="content-model"></a>コンテンツ モデル

大半の WPF コントロールの主な目的はコンテンツを表示することです。 WPF では、コントロールのコンテンツを構成するアイテムの種類と数を、コントロールの *コンテンツ モデル*と呼びます。 一部のコントロールには、1 つのアイテムと種類のコンテンツを含めることができます。たとえば、 <xref:System.Windows.Controls.TextBox> のコンテンツは <xref:System.Windows.Controls.TextBox.Text%2A> プロパティに割り当てられている文字列値です。 次の例では、<xref:System.Windows.Controls.TextBox>の内容を設定します。

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    x:Class="SDKSample.TextBoxContentWindow"
    Title="TextBox Content">

    <TextBox Text="This is the content of a TextBox." />
</Window>
```

結果を次の図に示します。

![テキストを含む TextBox コントロール](media/introduction-to-wpf/wpfintrofigure21.png)

しかし、さまざまな種類の複数のアイテムのコンテンツを含めることができるコントロールもあります。 <xref:System.Windows.Controls.Button>プロパティで指定された <xref:System.Windows.Controls.ContentControl.Content%2A> のコンテンツには、レイアウト コントロール、テキスト、画像、図形などのさまざまなアイテムを含めることができます。 次の例は、<xref:System.Windows.Controls.DockPanel>、<xref:System.Windows.Controls.Label>、<xref:System.Windows.Controls.Border>、および <xref:System.Windows.Controls.MediaElement>を含むコンテンツを含む <xref:System.Windows.Controls.Button> を示しています。

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    x:Class="SDKSample.ButtonContentWindow"
    Title="Button Content">

  <Button Margin="20">
    <!-- Button Content -->
    <DockPanel Width="200" Height="180">
      <Label DockPanel.Dock="Top" HorizontalAlignment="Center">Click Me!</Label>
      <Border Background="Black" BorderBrush="Yellow" BorderThickness="2"
        CornerRadius="2" Margin="5">
        <MediaElement Source="media/wpf.wmv" Stretch="Fill" />
      </Border>
    </DockPanel>
  </Button>
</Window>
```

次の図は、このボタンの内容を示しています。

![複数の種類の内容を含むボタン](media/introduction-to-wpf/wpfintrofigure22.png)

さまざまなコントロールでサポートされているコンテンツの種類について詳しくは、「[WPF コンテンツ モデル](controls/wpf-content-model.md)」をご覧ください。

### <a name="triggers"></a>トリガー

XAML マークアップの主な目的はアプリケーションの外観を実装することですが、XAML を使用してアプリケーションの動作の一部の機能を実装することもできます。 その一例として、ユーザーの操作に基づいて、アプリケーションの外観を変更するトリガーの使用があります。 詳細については、[スタイルとテンプレート](../../desktop-wpf/fundamentals/styles-templates-overview.md)に関するページをご覧ください。

### <a name="control-templates"></a>コントロール テンプレート

WPF コントロールの既定のユーザー インターフェイスは、通常、他のコントロールと図形で構成されます。 たとえば、 <xref:System.Windows.Controls.Button> は <xref:Microsoft.Windows.Themes.ButtonChrome> コントロールと <xref:System.Windows.Controls.ContentPresenter> コントロールの両方で構成されます。 <xref:Microsoft.Windows.Themes.ButtonChrome> は標準的なボタンの外観を提供するのに対し、 <xref:System.Windows.Controls.ContentPresenter> は <xref:System.Windows.Controls.ContentControl.Content%2A> プロパティで指定したボタンのコンテンツを表示します。

コントロールの既定の外観が、アプリケーションの全体的な外観と調和しない場合もあります。 そのような場合は、 <xref:System.Windows.Controls.ControlTemplate> を使用すれば、コンテンツと動作を変更することなく、コントロールのユーザー インターフェイスの外観を変更できます。

次の例は、<xref:System.Windows.Controls.ControlTemplate>を使用して <xref:System.Windows.Controls.Button> の外観を変更する方法を示しています。

[!code-xaml[IntroToWPFSnippets#ButtonControlTemplateWindowMARKUP](~/samples/snippets/xaml/wpf/introduction-to-wpf/introduction-to-wpf_16.xaml)]

[!code-csharp[IntroToWPFSnippets#ButtonControlTemplateWindowCODEBEHIND](~/samples/snippets/csharp/wpf/introduction-to-wpf/introduction-to-wpf_17.cs)]
[!code-vb[IntroToWPFSnippets#ButtonControlTemplateWindowCODEBEHIND](~/samples/snippets/visualbasic/wpf/introduction-to-wpf/introduction-to-wpf_17.vb)]

この例では、既定のボタン ユーザー インターフェイスが、濃い青の枠線を持ち、 <xref:System.Windows.Shapes.Ellipse> で塗りつぶされた <xref:System.Windows.Media.RadialGradientBrush>に置き換えられています。 <xref:System.Windows.Controls.ContentPresenter> コントロールは <xref:System.Windows.Controls.Button>のコンテンツである "Click Me!" を表示します。 <xref:System.Windows.Controls.Button> がクリックされると、 <xref:System.Windows.Controls.Primitives.ButtonBase.Click> コントロールの既定の動作の一部として <xref:System.Windows.Controls.Button> イベントが発生します。 結果を次の図に示します。

![省略記号ボタンと 2 番目のウィンドウ](media/introduction-to-wpf/wpfintrofigure2.png)

### <a name="data-templates"></a>データ テンプレート

コントロール テンプレートを使用すると、コントロールの外観を指定できますが、データ テンプレートではコントロールのコンテンツの外観を指定できます。 データ テンプレートはたいてい、バインドされたデータの表示方法を多様化するために使用されます。 次の図は、`Task` オブジェクトのコレクションにバインドされている <xref:System.Windows.Controls.ListBox> の既定の外観を示しています。各タスクには名前、説明、および優先順位があります。

![既定の外観を使用したリスト ボックス](media/introduction-to-wpf/wpfintrofigure18.png)

既定の外観は <xref:System.Windows.Controls.ListBox>に期待されるものです。 ただし、各タスクの既定の外観にはタスク名しか含まれていません。 タスク名、説明、優先度の既定の外観を表示するには、 <xref:System.Windows.Controls.ListBox> コントロールのバインドされたリスト項目の既定の外観を、 <xref:System.Windows.DataTemplate>を使用して変更する必要があります。 次の XAML は、<xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> 属性を使用して各タスクに適用される、このような <xref:System.Windows.DataTemplate>を定義します。

```xaml
<Window
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
  x:Class="SDKSample.DataTemplateWindow"
  Title="With a Data Template">
  <Window.Resources>
    <!-- Data Template (applied to each bound task item in the task collection) -->
    <DataTemplate x:Key="myTaskTemplate">
      <Border Name="border" BorderBrush="DarkSlateBlue" BorderThickness="2"
        CornerRadius="2" Padding="5" Margin="5">
        <Grid>
          <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
          </Grid.RowDefinitions>
          <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition />
          </Grid.ColumnDefinitions>
          <TextBlock Grid.Row="0" Grid.Column="0" Padding="0,0,5,0" Text="Task Name:"/>
          <TextBlock Grid.Row="0" Grid.Column="1" Text="{Binding Path=TaskName}"/>
          <TextBlock Grid.Row="1" Grid.Column="0" Padding="0,0,5,0" Text="Description:"/>
          <TextBlock Grid.Row="1" Grid.Column="1" Text="{Binding Path=Description}"/>
          <TextBlock Grid.Row="2" Grid.Column="0" Padding="0,0,5,0" Text="Priority:"/>
          <TextBlock Grid.Row="2" Grid.Column="1" Text="{Binding Path=Priority}"/>
        </Grid>
      </Border>
    </DataTemplate>
  </Window.Resources>

  <!-- UI -->
  <DockPanel>
    <!-- Title -->
    <Label DockPanel.Dock="Top" FontSize="18" Margin="5" Content="My Task List:"/>

    <!-- Data template is specified by the ItemTemplate attribute -->
    <ListBox
      ItemsSource="{Binding}"
      ItemTemplate="{StaticResource myTaskTemplate}"
      HorizontalContentAlignment="Stretch"
      IsSynchronizedWithCurrentItem="True"
      Margin="5,0,5,5" />

 </DockPanel>
</Window>
```

次の図は、このコードの効果を示しています。

![データ テンプレートを使用するリスト ボックス](media/introduction-to-wpf/wpfintrofigure19.png)

<xref:System.Windows.Controls.ListBox> の動作と全体的な外観は保持されていることにご注意ください。リスト ボックスにより表示されるコンテンツの外観のみが変更されています。

詳しくは「[データ テンプレートの概要](data/data-templating-overview.md)」をご覧ください。

### <a name="styles"></a>スタイル

スタイルを使うと、開発者とデザイナーは製品の特定の外観を標準化できます。 WPF には強力なスタイル モデルが用意されており、この基盤となるのが <xref:System.Windows.Style> 要素です。 次の例では、ウィンドウ上のすべての <xref:System.Windows.Controls.Button> の背景色を `Orange`に設定するスタイルを作成します。

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    x:Class="SDKSample.StyleWindow"
    Title="Styles">
  <!-- Style that will be applied to all buttons -->
  <Style TargetType="{x:Type Button}">
    <Setter Property="Background" Value="Orange" />
    <Setter Property="BorderBrush" Value="Crimson" />
    <Setter Property="FontSize" Value="20" />
    <Setter Property="FontWeight" Value="Bold" />
    <Setter Property="Margin" Value="5" />
  </Style>
  <!-- This button will have the style applied to it -->
  <Button>Click Me!</Button>

  <!-- This label will not have the style applied to it -->
  <Label>Don't Click Me!</Label>

  <!-- This button will have the style applied to it -->
  <Button>Click Me!</Button>
</Window>
```

このスタイルでは、すべての <xref:System.Windows.Controls.Button> コントロールを対象としているため、次の図に示すように、スタイルがウィンドウのすべてのボタンに自動的に適用されます。

![2 つのオレンジ色のボタン](media/introduction-to-wpf/wpfintrofigure20.png)

詳細については、[スタイルとテンプレート](../../desktop-wpf/fundamentals/styles-templates-overview.md)に関するページをご覧ください。

### <a name="resources"></a>リソース

アプリケーションのコントロール類は、同じ外観を持つ必要があります。こうした外観はフォントや背景色からコントロール テンプレート、データ テンプレート、スタイルまで、多岐にわたります。 ユーザー インターフェイスのリソースに対する WPF のサポートを使用すれば、こうした各種リソースを 1 つの場所でカプセル化して、再利用することができます。

次の例では、<xref:System.Windows.Controls.Button> と <xref:System.Windows.Controls.Label>によって共有される共通の背景色を定義します。

```xaml
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    x:Class="SDKSample.ResourcesWindow"
    Title="Resources Window">

  <!-- Define window-scoped background color resource -->
  <Window.Resources>
    <SolidColorBrush x:Key="defaultBackground" Color="Red" />
  </Window.Resources>

  <!-- Button background is defined by window-scoped resource -->
  <Button Background="{StaticResource defaultBackground}">One Button</Button>

  <!-- Label background is defined by window-scoped resource -->
  <Label Background="{StaticResource defaultBackground}">One Label</Label>
</Window>
```

この例では `Window.Resources` プロパティ要素を使用して、背景色リソースを実装しています。 このリソースは <xref:System.Windows.Window>のすべての子に使用できます。 次のように、リソースのスコープは多様です。この一覧では、解決される順序で各スコープが並べられています。

1. 個々のコントロール (継承された <xref:System.Windows.FrameworkElement.Resources%2A?displayProperty=fullName> プロパティを使用)。

2. <xref:System.Windows.Window> または <xref:System.Windows.Controls.Page> (これも、継承された <xref:System.Windows.FrameworkElement.Resources%2A?displayProperty=fullName> プロパティを使用)。

3. <xref:System.Windows.Application> ( <xref:System.Windows.Application.Resources%2A?displayProperty=fullName> プロパティを使用)。

このようなスコープの多様さのおかげで、リソースを柔軟に定義して共有できるようになります。

リソースを特定のスコープに直接関連付ける代わりに、アプリケーションの他の部分で参照できる別個の <xref:System.Windows.ResourceDictionary> を使用して、1 つ以上のリソースをパッケージ化することができます。 たとえば、次の例では、リソースディクショナリに既定の背景色を定義しています。

```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

  <!-- Define background color resource -->
  <SolidColorBrush x:Key="defaultBackground" Color="Red" />

  <!-- Define other resources -->
</ResourceDictionary>
```

次の例では、アプリケーション全体で共有されるように、前の例で定義されているリソースディクショナリを参照します。

```xaml
<Application
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    x:Class="SDKSample.App">

  <Application.Resources>
    <ResourceDictionary>
      <ResourceDictionary.MergedDictionaries>
        <ResourceDictionary Source="BackgroundColorResources.xaml"/>
      </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
  </Application.Resources>
</Application>
```

リソースおよびリソース ディクショナリは、テーマとスキンに対する WPF サポートの基礎です。

詳しくは、[リソース](../../desktop-wpf/fundamentals/xaml-resources-define.md)に関する記事をご覧ください。

### <a name="custom-controls"></a>カスタム コントロール

WPF にはカスタマイズに対する多くのサポートが用意されていますが、状況によっては既存の WPF コントロールではアプリケーションやそのユーザーのニーズを満たせない場合があります。 次のような状況が考えられます。

- WPF の既存の実装のルックアンドフィールをカスタマイズしても、必要とするユーザー インターフェイスを作成できない。

- WPF の既存の実装が、必要とする動作をサポートしない (または簡単にはサポートできない)。

ただし現時点では、3 つの WPF モデルのいずれかを利用して、新しいコントロールを作成できます。 各モデルは固有のシナリオを対象としており、カスタム コントロールは特定の WPF 基底クラスから派生するものでなければなりません。 この 3 つのモデルは次のとおりです。

- **ユーザー コントロール モデル** カスタム コントロールは <xref:System.Windows.Controls.UserControl> から派生し、他の 1 つ以上のコントロールで構成されます。

- **コントロール モデル** カスタム コントロールは <xref:System.Windows.Controls.Control> から派生し、大半の WPF コントロール同様、テンプレートを使用して、動作を外観から分離する実装を構築するために使用されます。 <xref:System.Windows.Controls.Control> から派生することにより、ユーザー コントロールよりも自由度の高いカスタム ユーザー インターフェイスを作成できますが、これにはより多くの労力が必要です。

- **フレームワーク要素モデル** カスタム コントロールは、カスタム レンダリング ロジック (テンプレートではなく) によって外観が定義されるときに、 <xref:System.Windows.FrameworkElement> から派生します。

次の例は、<xref:System.Windows.Controls.UserControl>から派生するカスタムの数値のアップダウンコントロールを示しています。

[!code-xaml[IntroToWPFSnippets#UserControlMARKUP](~/samples/snippets/xaml/wpf/introduction-to-wpf/introduction-to-wpf_33.xaml)]

[!code-csharp[IntroToWPFSnippets#UserControlCODEBEHIND1](~/samples/snippets/csharp/wpf/introduction-to-wpf/introduction-to-wpf_34.cs)]
[!code-vb[IntroToWPFSnippets#UserControlCODEBEHIND1](~/samples/snippets/visualbasic/wpf/introduction-to-wpf/introduction-to-wpf_34.vb)]

次の例は、ユーザーコントロールを <xref:System.Windows.Window>に組み込むために必要な XAML を示しています。

[!code-xaml[IntroToWPFSnippets#UserControlWindowMARKUP1](~/samples/snippets/xaml/wpf/introduction-to-wpf/introduction-to-wpf_37.xaml)]

次の図は、<xref:System.Windows.Window>でホストされている `NumericUpDown` コントロールを示しています。

![カスタム UserControl](media/introduction-to-wpf/wpfintrofigure3.png)

カスタム コントロールについて詳しくは、「[コントロールの作成の概要](controls/control-authoring-overview.md)」をご覧ください。

## <a name="wpf-best-practices"></a>WPF のベスト プラクティス

WPF はすべての開発プラットフォームと同様、目的の結果を得るために、さまざまな方法で使用できます。 必要なユーザー エクスペリエンスを WPF アプリケーションが確実に提供し、オーディエンス一般の需要に応える 1 つの方法として、アクセシビリティ、グローバリゼーションとローカリゼーション、パフォーマンスに関するお勧めのベスト プラクティスがあります。 詳細については、次を参照してください。

- [ユーザー補助](../ui-automation/accessibility-best-practices.md)
- [WPF のグローバリゼーションとローカライズ](advanced/wpf-globalization-and-localization-overview.md)
- [WPF アプリのパフォーマンス](advanced/optimizing-wpf-application-performance.md)
- [WPF のセキュリティ](security-wpf.md)

## <a name="next-steps"></a>次のステップ

WPF の主な機能を確認しました。 次は、初めての WPF アプリをビルドします。

> [!div class="nextstepaction"]
> [チュートリアル: 初めての WPF デスクトップ アプリケーション](getting-started/walkthrough-my-first-wpf-desktop-application.md)

## <a name="see-also"></a>参照

- [WPF の使用を開始する](getting-started/index.md)
- [Windows Presentation Foundation](index.md)
- [WPF コミュニティ リソース](getting-started/community-feedback.md)
