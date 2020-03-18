---
title: Visual Studio 2019 で初めての WPF アプリを作成する-.NET Framework
titleSuffix: ''
ms.date: 09/06/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- getting started [WPF], WPF
- WPF [WPF], getting started
ms.assetid: b96bed40-8946-4285-8fe4-88045ab854ed
ms.topic: tutorial
ms.custom: mvc,vs-dotnet
ms.openlocfilehash: bc47405636c4727f502caf1f6e27050367eda74a
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77124339"
---
# <a name="tutorial-create-your-first-wpf-application-in-visual-studio-2019"></a>チュートリアル: Visual Studio 2019 で初めての WPF アプリケーションを作成する

この記事では、ほとんどの WPF アプリケーションに共通する要素 (Extensible Application Markup Language (XAML) マークアップ、分離コード、アプリケーション定義) を含む Windows Presentation Foundation (WPF) デスクトップアプリケーションを開発する方法について説明します。コントロール、レイアウト、データバインディング、およびスタイル。 アプリケーションを開発するには、Visual Studio を使用します。 

このチュートリアルでは、以下の内容を学習します。
> [!div class="checklist"]
>
> - WPF プロジェクトを作成します。
> - XAML を使用して、アプリケーションのユーザーインターフェイス (UI) の外観をデザインします。
> - アプリケーションの動作を構築するコードを記述します。
> - アプリケーションを管理するためのアプリケーション定義を作成します。
> - コントロールを追加し、レイアウトを作成してアプリケーション UI を作成します。
> - アプリケーションの UI 全体で一貫した外観のスタイルを作成します。
> - Ui にデータを設定し、データと UI の同期を保つために、UI をデータにバインドします。

チュートリアルを終了すると、選択したユーザーの経費報告書をユーザーが表示できるスタンドアロン Windows アプリケーションが作成されます。 アプリケーションは、ブラウザースタイルのウィンドウでホストされるいくつかの WPF ページで構成されています。

> [!TIP]
> このチュートリアルで使用されているサンプルコードは、Visual Basic とC#チュートリアルの[WPF アプリのサンプルコード](https://github.com/Microsoft/WPF-Samples/tree/master/Getting%20Started/WalkthroughFirstWPFApp)の両方で使用できます。
>
> このページの上部にある言語セレクターを使用しC#て、と Visual Basic の間でサンプルコードのコード言語を切り替えることができます。

## <a name="prerequisites"></a>前提条件

- **.Net デスクトップ開発**ワークロードがインストールされた[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 。

   最新バージョンの Visual Studio をインストールする方法の詳細については、「 [Visual studio のインストール](/visualstudio/install/install-visual-studio)」を参照してください。

## <a name="create-the-application-project"></a>アプリケーションプロジェクトを作成する

最初の手順では、アプリケーションの定義、2つのページ、およびイメージを含むアプリケーションインフラストラクチャを作成します。

1. Visual Basic または Visual C# **`ExpenseIt`** という名前の新しい WPF アプリケーションプロジェクトを作成します。

   1. Visual Studio を開き、 **[作業の開始]** メニューの **[新しいプロジェクトの作成]** を選択します。

      **[新しいプロジェクトの作成]** ダイアログボックスが表示されます。

   2. **言語** ドロップダウンで、 **C#**  または  **Visual Basic** を選択します。
      
   3. **[WPF アプリ (.NET Framework)]** テンプレートを選択し、 **[次へ]** を選択します。 
     
      ![[新しいプロジェクトの作成] ダイアログ](./media/walkthrough-my-first-wpf-desktop-application/create-new-project-dialog.png)
    
      **[新しいプロジェクトの構成]** ダイアログボックスが開きます。

   4. **`ExpenseIt`** プロジェクト名を入力し、 **[作成]** を選択します。

      ![[新しいプロジェクトの構成] ダイアログ](./media/walkthrough-my-first-wpf-desktop-application/configure-new-project-dialog.png)

      Visual Studio によってプロジェクトが作成され、 **mainwindow.xaml**という名前の既定のアプリケーションウィンドウのデザイナーが開きます。

2. *アプリケーション .xaml* (Visual Basic) または*app.xaml* (C#) を開きます。

    この XAML ファイルは、WPF アプリケーションとすべてのアプリケーションリソースを定義します。 また、このファイルを使用して、アプリケーションの起動時に自動的に表示される UI (この場合は Mainwindow.xaml) を指定し*ます*。

    XAML は、Visual Basic では次のようになります。

    [!code-xaml[ExpenseIt#1_A](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ExpenseIt/VB/ExpenseIt1_A/Application.xaml#1_a)]

    およびでは、次C#のようになります。

    [!code-xaml[ExpenseIt#1](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt/App.xaml#1)]

3. *Mainwindow.xaml*を開きます。

    この XAML ファイルは、アプリケーションのメインウィンドウであり、ページで作成されたコンテンツを表示します。 <xref:System.Windows.Window> クラスは、ウィンドウのタイトル、サイズ、アイコンなどのプロパティを定義し、終了や非表示などのイベントを処理します。

4. 次の XAML に示すように、<xref:System.Windows.Window> 要素を <xref:System.Windows.Navigation.NavigationWindow>に変更します。

   ```xaml
   <NavigationWindow x:Class="ExpenseIt.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        ...
   </NavigationWindow>
   ```

   このアプリは、ユーザーの入力に応じてさまざまなコンテンツに移動します。 このため、メイン <xref:System.Windows.Window> を <xref:System.Windows.Navigation.NavigationWindow>に変更する必要があります。 <xref:System.Windows.Navigation.NavigationWindow> <xref:System.Windows.Window>のすべてのプロパティを継承します。 XAML ファイルの <xref:System.Windows.Navigation.NavigationWindow> 要素は、<xref:System.Windows.Navigation.NavigationWindow> クラスのインスタンスを作成します。 詳細については、「[ナビゲーションの概要](../app-development/navigation-overview.md)」を参照してください。

5. <xref:System.Windows.Navigation.NavigationWindow> タグの間から <xref:System.Windows.Controls.Grid> 要素を削除します。

6. <xref:System.Windows.Navigation.NavigationWindow> 要素の XAML コードで、次のプロパティを変更します。

    - <xref:System.Windows.Window.Title%2A> プロパティを "`ExpenseIt`" に設定します。

    - <xref:System.Windows.FrameworkElement.Height%2A> プロパティを350ピクセルに設定します。

    - <xref:System.Windows.FrameworkElement.Width%2A> プロパティを500ピクセルに設定します。

    XAML は、Visual Basic に対して次のようになります。

    [!code-xaml[ExpenseIt#2_A](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ExpenseIt/VB/ExpenseIt/MainWindow.xaml#2_a)]

    では、次のC#ようになります。

    [!code-xaml[ExpenseIt#2](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt/MainWindow.xaml#2)]

7. *Mainwindow.xaml*または*MainWindow.xaml.cs*を開きます。

    このファイルは、 *mainwindow.xaml*で宣言されたイベントを処理するコードを含む分離コードファイルです。 このファイルには、XAML で定義されたウィンドウの部分クラスが含まれています。

8. を使用してC#いる場合は、<xref:System.Windows.Navigation.NavigationWindow>から派生するように `MainWindow` クラスを変更します。 (Visual Basic では、XAML でウィンドウを変更すると、自動的にこの処理が行われます)。コードC#は次のようになります。

   [!code-csharp[ExpenseIt#3](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt9/MainWindow.xaml.cs?highlight=21)]

## <a name="add-files-to-the-application"></a>アプリケーションへのファイルの追加

このセクションでは、アプリケーションに 2 つのページと 1 つのイメージを追加します。

1. 新しいページをプロジェクトに追加し、 *`ExpenseItHome.xaml`* という名前を指定します。

   1. **ソリューションエクスプローラー**で、[ **`ExpenseIt`** プロジェクト] ノードを右クリックし、[ > **ページ**の**追加**] を選択します。

   1. **[新しい項目の追加]** ダイアログでは、 **[ページ (WPF)]** テンプレートは既に選択されています。 **`ExpenseItHome`** 名を入力し、 **[追加]** を選択します。

    このページは、アプリケーションの起動時に表示される最初のページです。 このレポートには、の経費明細書を表示するために選択する相手の一覧が表示されます。

1. *`ExpenseItHome.xaml`* を開きます。

1. <xref:System.Windows.Controls.Page.Title%2A> を "`ExpenseIt - Home`" に設定します。

1. `DesignHeight` を350ピクセルに設定し、`DesignWidth` を500ピクセルに設定します。

    XAML は、Visual Basic に対して次のように表示されるようになりました。

    [!code-xaml[ExpenseIt#6_A](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ExpenseIt/VB/ExpenseIt1_A/ExpenseItHome.xaml#6_a)]

    では、次のC#ようになります。

    [!code-xaml[ExpenseIt#6](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt2/ExpenseItHome.xaml#6)]

1. *Mainwindow.xaml*を開きます。

1. <xref:System.Windows.Navigation.NavigationWindow> 要素に <xref:System.Windows.Navigation.NavigationWindow.Source%2A> プロパティを追加し、それを "`ExpenseItHome.xaml`" に設定します。

    これにより、アプリケーションの起動時に最初に開かれるページに *`ExpenseItHome.xaml`* が設定されます。 

    Visual Basic の XAML の例:

    [!code-xaml[ExpenseIt#7_A](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ExpenseIt/VB/ExpenseIt1_A/MainWindow.xaml#7_a)]

    および C# の場合。

    [!code-xaml[ExpenseIt#7](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt2/MainWindow.xaml#7)]

   > [!TIP]
   > また、 **[プロパティ]** ウィンドウの **[その他]** カテゴリで、 **[ソース]** プロパティを設定することもできます。
   >
   > ![プロパティウィンドウのソースプロパティ](./media/properties-source.png)

1. 別の新しい WPF ページをプロジェクトに追加し、次*のように*名前を指定します。

   1. **ソリューションエクスプローラー**で、[ **`ExpenseIt`** プロジェクト] ノードを右クリックし、[ > **ページ**の**追加**] を選択します。

   1. **[新しい項目の追加]** ダイアログで、 **[ページ (WPF)]** テンプレートを選択します。 [名前の指定] をクリック**し、[** **追加**] を選択します。

    このページには、[ **`ExpenseItHome`** ] ページで選択されたユーザーの経費明細書が表示されます。

1. *ExpenseReportPage.xaml*を開きます。

1. <xref:System.Windows.Controls.Page.Title%2A> を "`ExpenseIt - View Expense`" に設定します。

1. `DesignHeight` を350ピクセルに設定し、`DesignWidth` を500ピクセルに設定します。 

    Visual Basic では、この*xaml*は次のようになります。

    [!code-xaml[ExpenseIt#4_A](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ExpenseIt/VB/ExpenseIt1_A/ExpenseReportPage.xaml#4_a)]

    およびでは、次C#のようになります。

    [!code-xaml[ExpenseIt#4](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt/ExpenseReportPage.xaml#4)]

1. *Expenseithome.xaml*を開き、 *ExpenseItHome.xaml.cs* 、または ExpenseReportPage.xaml.cs *、また*はを開きます。

    新しいページファイルを作成すると、Visual Studio によってその*分離コード*ファイルが自動的に作成されます。 これらの分離コード ファイルでは、ユーザー入力に対応するためのロジックを処理します。

    **`ExpenseItHome`** のコードは次のようになります。

    [!code-csharp[ExpenseIt#2_5](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt2/ExpenseItHome.xaml.cs#2_5)]

    [!code-vb[ExpenseIt#2_5](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ExpenseIt/VB/ExpenseIt1_A/ExpenseItHome.xaml.vb#2_5)]

    また、次のような**ページ**があります。

    [!code-csharp[ExpenseIt#5](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt/ExpenseReportPage.xaml.cs#5)]

    [!code-vb[ExpenseIt#5](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ExpenseIt/VB/ExpenseIt1_A/ExpenseReportPage.xaml.vb#5)]

1. *透かし*という名前のイメージをプロジェクトに追加します。 独自のイメージを作成したり、サンプルコードからファイルをコピーしたり、 [microsoft/WPF-Samples](https://raw.githubusercontent.com/microsoft/WPF-Samples/master/Getting%20Started/WalkthroughFirstWPFApp/csharp/watermark.png) GitHub リポジトリから取得したりすることができます。

    1. プロジェクトノードを右クリックし、[**既存の項目**の**追加** > ] を選択するか、 **Shift**キーを押し+**Alt**+**A**キーを押します。

    2. **[既存項目の追加]** ダイアログボックスで、すべての **[ファイル]** または **[イメージファイル]** のいずれかにファイルフィルターを設定し、使用するイメージファイルを参照して **[追加]** を選択します。

## <a name="build-and-run-the-application"></a>アプリケーションの構築と実行

1. アプリケーションをビルドして実行するには、 **F5**キーを押すか、 **[デバッグ]** メニューの **[デバッグの開始]** をクリックします。

    次の図は、<xref:System.Windows.Navigation.NavigationWindow> ボタンを持つアプリケーションを示しています。

    ![アプリケーションをビルドして実行します。](./media/walkthrough-my-first-wpf-desktop-application/build-run-application.png)

2. アプリケーションを閉じて、Visual Studio に戻ります。

## <a name="create-the-layout"></a>レイアウトを作成する

レイアウトは、UI 要素を配置するための順序付けされた方法を提供し、UI のサイズが変更されたときの要素のサイズと位置も管理します。 通常、レイアウトを作成するには、次のいずれかのレイアウト コントロールを使用します。

- <xref:System.Windows.Controls.Canvas>-キャンバス領域に対する相対座標を使用して子要素を明示的に配置できる領域を定義します。
- <xref:System.Windows.Controls.DockPanel>-子要素を水平方向または垂直方向に整列できる領域を定義します。
- <xref:System.Windows.Controls.Grid>-列と行で構成される柔軟性のあるグリッド領域を定義します。
- <xref:System.Windows.Controls.StackPanel>-子要素を水平方向または垂直方向の単一行に整列します。
- <xref:System.Windows.Controls.VirtualizingStackPanel>-水平方向または垂直方向の単一行でコンテンツを整列し、仮想化します。
- 子要素を左から右へ順番に配置し、内容を含んでいるボックスの端にある次の行に分割します。 <xref:System.Windows.Controls.WrapPanel> 後続の順序付けは、Orientation プロパティの値に応じて、上から下または右から左に順番に行われます。

これらの各レイアウトコントロールは、その子要素に対して特定の種類のレイアウトをサポートしています。 `ExpenseIt` のページのサイズを変更することができ、各ページには、水平方向および垂直方向に他の要素と共に配置された要素があります。 この例では、<xref:System.Windows.Controls.Grid> はアプリケーションのレイアウト要素として使用されます。

> [!TIP]
> <xref:System.Windows.Controls.Panel> 要素の詳細については、「[パネルの概要](../controls/panels-overview.md)」を参照してください。 レイアウトの詳細については、「 [layout](../advanced/layout.md)」を参照してください。

このセクションでは、 *`ExpenseItHome.xaml`* の <xref:System.Windows.Controls.Grid> に列と行の定義を追加することによって、3つの行と10ピクセルの余白を含む単一列テーブルを作成します。

1. *`ExpenseItHome.xaml`* で、<xref:System.Windows.Controls.Grid> 要素の <xref:System.Windows.FrameworkElement.Margin%2A> プロパティを "10, 0, 10, 10" に設定します。これは、左、上、右、下の余白に対応します。

   ```xaml
   <Grid Margin="10,0,10,10">
   ```

   > [!TIP]
   > また、 **[プロパティ]** ウィンドウの **[レイアウト]** カテゴリで、**余白**の値を設定することもできます。
   >
   > ![プロパティウィンドウの余白の値](./media/properties-margin.png)

2. 次の XAML を <xref:System.Windows.Controls.Grid> タグの間に追加して、行と列の定義を作成します。

    [!code-xaml[ExpenseIt#8](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt3/ExpenseItHome.xaml#8)]

    2つの行の <xref:System.Windows.Controls.RowDefinition.Height%2A> は <xref:System.Windows.GridLength.Auto%2A>に設定されます。つまり、行のサイズは、行の内容に基づいて決定されます。 既定の <xref:System.Windows.Controls.RowDefinition.Height%2A> は <xref:System.Windows.GridUnitType.Star> サイズ変更です。これは、行の高さが使用可能な領域の加重比率であることを意味します。 たとえば、2つの行のそれぞれに "*" の <xref:System.Windows.Controls.RowDefinition.Height%2A> がある場合、それぞれの行の高さは使用可能な領域の半分になります。

    <xref:System.Windows.Controls.Grid> に次の XAML が含まれるようになります。

    [!code-xaml[ExpenseIt#9](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt3/ExpenseItHome.xaml#9)]

## <a name="add-controls"></a>コントロールの追加

このセクションでは、ホームページの UI を更新して、人の一覧を表示します。ここでは、1人のユーザーを選択して経費報告書を表示します。 コントロールとは、ユーザーがアプリケーションと対話できるようにする UI オブジェクトのことです。 詳しくは、「 [コントロール](../controls/index.md)」をご覧ください。

この UI を作成するには、 *`ExpenseItHome.xaml`* に次の要素を追加します。

- <xref:System.Windows.Controls.ListBox> (people の一覧)。
- (リストヘッダーの) <xref:System.Windows.Controls.Label>。
- <xref:System.Windows.Controls.Button> (クリックすると、一覧で選択されているユーザーの経費明細書が表示されます)。

各コントロールは、<xref:System.Windows.Controls.Grid.Row%2A?displayProperty=nameWithType> 添付プロパティを設定することによって、<xref:System.Windows.Controls.Grid> の行に配置されます。 添付プロパティの詳細については、「[添付プロパティの概要](../advanced/attached-properties-overview.md)」を参照してください。

1. *`ExpenseItHome.xaml`* で、<xref:System.Windows.Controls.Grid> タグの間に次の XAML を追加します。

   [!code-xaml[ExpenseIt#10](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt4/ExpenseItHome.xaml#10)]

   > [!TIP]
   > コントロールは、 **[ツールボックス]** ウィンドウからデザインウィンドウにドラッグして、 **[プロパティ]** ウィンドウでプロパティを設定することによって作成することもできます。

2. アプリケーションをビルドして実行します。

    次の図は、作成したコントロールを示しています。

![名前の一覧を表示している、このサンプルスクリーンショット](./media/walkthrough-my-first-wpf-desktop-application/add-application-controls.png)

## <a name="add-an-image-and-a-title"></a>イメージとタイトルを追加する

このセクションでは、ホームページの UI をイメージとページタイトルで更新します。

1. *`ExpenseItHome.xaml`* で <xref:System.Windows.Controls.Grid.ColumnDefinitions%2A> に別の列を追加し、230ピクセルの固定 <xref:System.Windows.Controls.ColumnDefinition.Width%2A> を使用します。

    [!code-xaml[ExpenseIt#11](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt9/ExpenseItHome.xaml?highlight=52-55)]

2. <xref:System.Windows.Controls.Grid.RowDefinitions%2A>に、合計4行の行を追加します。

    [!code-xaml[ExpenseIt#11b](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt9/ExpenseItHome.xaml?highlight=57-62)]

3. コントロールを2番目の列に移動するには、3つのコントロール (Border、ListBox、および Button) のそれぞれで [<xref:System.Windows.Controls.Grid.Column%2A?displayProperty=nameWithType>] プロパティを1に設定します。

4. 各コントロールを1行下に移動します。そのためには、3つのコントロール (Border、ListBox、および Button) と Border 要素のそれぞれに対して、<xref:System.Windows.Controls.Grid.Row%2A?displayProperty=nameWithType> 値を1ずつインクリメントします。

   3つのコントロールの XAML は、次のようになります。

    [!code-xaml[ExpenseIt#12](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt5/ExpenseItHome.xaml#12)]

5. `<Grid>` タグと `</Grid>` タグの間に次の XAML を追加して、<xref:System.Windows.Controls.Panel.Background%2A?displayProperty=nameWithType> プロパティを*ウォーターマークの .png*画像ファイルに設定します。

    [!code-xaml[ExpenseIt#14](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt5/ExpenseItHome.xaml#14)]

6. <xref:System.Windows.Controls.Border> 要素の前に、「経費報告書の表示」という内容の <xref:System.Windows.Controls.Label> を追加します。 このラベルはページのタイトルです。

    [!code-xaml[ExpenseIt#13](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt5/ExpenseItHome.xaml#13)]

7. アプリケーションをビルドして実行します。

次の図は、先ほど追加したものの結果を示しています。

![新しいイメージの背景とページのタイトルを示す、ページのサンプルスクリーンショット](./media/walkthrough-my-first-wpf-desktop-application/add-application-image-title.png)

## <a name="add-code-to-handle-events"></a>イベントを処理するコードを追加する

1. *`ExpenseItHome.xaml`* で、<xref:System.Windows.Controls.Button> 要素に <xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントハンドラーを追加します。 詳細については、「[方法: 単純なイベントハンドラーを作成](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/bb675300(v=vs.100))する」を参照してください。

    [!code-xaml[ExpenseIt#15](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt6/ExpenseItHome.xaml#15)]

2. *`ExpenseItHome.xaml.vb`* または *`ExpenseItHome.xaml.cs`* を開きます。

3. 次のコードを `ExpenseItHome` クラスに追加して、ボタンクリックイベントハンドラーを追加します。 イベントハンドラーによって、[処理された**Sereportpage** ] ページが開きます。

    [!code-csharp[ExpenseIt#16](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt6/ExpenseItHome.xaml.cs#16)]
    [!code-vb[ExpenseIt#16](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ExpenseIt/VB/ExpenseIt6/ExpenseItHome.xaml.vb#16)]

## <a name="create-the-ui-for-expensereportpage"></a>[印刷] ページの UI を作成する

[ **`ExpenseItHome`** ] ページで選択されたユーザーの経費明細書*が表示されます。* このセクションでは、[の使用]**ページ**の UI を作成します。 また、さまざまな UI 要素に背景色と塗りつぶしの色を追加します。

1. *ExpenseReportPage.xaml*を開きます。

2. <xref:System.Windows.Controls.Grid> タグの間に次の XAML を追加します。

    [!code-xaml[ExpenseIt#17](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt6/ExpenseReportPage.xaml#17)]

    この UI は *`ExpenseItHome.xaml`* に似ていますが、レポートデータが <xref:System.Windows.Controls.DataGrid>に表示される点が異なります。

3. アプリケーションをビルドして実行します。

4. **[表示]** ボタンを選択します。

    経費明細書ページが表示されます。 また、[戻る] ナビゲーションボタンが有効になっていることにも注意してください。

次の図は、*ページ*に追加された UI 要素を示しています。

![このページでは、[ページの作成] で作成した UI を示しています。](./media/walkthrough-my-first-wpf-desktop-application/create-application-ui.png)

## <a name="style-controls"></a>スタイルコントロール

多くの場合、UI で同じ種類のすべての要素に対して、さまざまな要素の外観が同じになります。 UI では、[スタイル](../controls/styling-and-templating.md)を使用して、複数の要素間で外観を再利用できるようにします。 スタイルの再利用性により、XAML の作成と管理が簡単になります。 このセクションでは、これまでの手順で定義した要素ごとの属性を、スタイルに置き換えます。

1. *アプリケーション .xaml* *または app.xaml を*開きます。

2. <xref:System.Windows.Application.Resources%2A?displayProperty=nameWithType> タグの間に次の XAML を追加します。

    [!code-xaml[ExpenseIt#18](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt7/App.xaml#18)]

    この XAML は、次のスタイルを追加します。

    - `headerTextStyle`: ページ タイトル <xref:System.Windows.Controls.Label>の書式を設定します。

    - `labelStyle`: <xref:System.Windows.Controls.Label> コントロールの書式を設定します。

    - `columnHeaderStyle`: <xref:System.Windows.Controls.Primitives.DataGridColumnHeader>の書式を設定します。

    - `listHeaderStyle`: リスト ヘッダーの <xref:System.Windows.Controls.Border> コントロールの書式を設定します。

    - `listHeaderTextStyle`: リストヘッダー <xref:System.Windows.Controls.Label>の書式を設定します。

    - `buttonStyle`: `ExpenseItHome.xaml`で <xref:System.Windows.Controls.Button> を書式設定します。

    スタイルは、<xref:System.Windows.Application.Resources%2A?displayProperty=nameWithType> property 要素のリソースと子であることに注意してください。 ここでは、スタイルはアプリケーション内のすべての要素に適用されます。 .NET アプリでのリソースの使用例については、「[アプリケーションリソースの使用](../advanced/how-to-use-application-resources.md)」を参照してください。

3. *`ExpenseItHome.xaml`* で、<xref:System.Windows.Controls.Grid> の要素間のすべてを次の XAML に置き換えます。

    [!code-xaml[ExpenseIt#19](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt7/ExpenseItHome.xaml#19)]

    各コントロールの外観を定義する <xref:System.Windows.VerticalAlignment> や <xref:System.Windows.Media.FontFamily> などのプロパティは、これらのスタイルを適用することで、削除されて置き換えられます。 たとえば、`headerTextStyle` は "経費報告書の表示" <xref:System.Windows.Controls.Label>に適用されます。

4. *ExpenseReportPage.xaml*を開きます。

5. <xref:System.Windows.Controls.Grid> の要素間のすべてを次の XAML に置き換えます。

    [!code-xaml[ExpenseIt#20](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt7/ExpenseReportPage.xaml#20)]

    この XAML は、<xref:System.Windows.Controls.Label> 要素と <xref:System.Windows.Controls.Border> 要素にスタイルを追加します。

6. アプリケーションをビルドして実行します。 ウィンドウの外観は、以前と同じです。

    ![前回のセクションと同じ外観の、このサンプルスクリーンショット。](./media/walkthrough-my-first-wpf-desktop-application/create-application-ui.png)

7. アプリケーションを閉じて、Visual Studio に戻ります。

## <a name="bind-data-to-a-control"></a>コントロールへのデータのバインド

このセクションでは、さまざまなコントロールにバインドされている XML データを作成します。

1. *`ExpenseItHome.xaml`* で、開いている <xref:System.Windows.Controls.Grid> 要素の後に、次の XAML を追加して、各ユーザーのデータを含む <xref:System.Windows.Data.XmlDataProvider> を作成します。

    [!code-xaml[ExpenseIt#23](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt8/ExpenseItHome.xaml?range=13,16-40,49)]

    データは <xref:System.Windows.Controls.Grid> リソースとして作成されます。 通常、このデータはファイルとして読み込まれますが、わかりやすくするために、データはインラインで追加されます。

2. `<Grid.Resources>` 要素内に、次の `<xref:System.Windows.DataTemplate>` 要素を追加します。この要素は、<xref:System.Windows.Controls.ListBox>内のデータを `<XmlDataProvider>` 要素の後に表示する方法を定義します。

    [!code-xaml[ExpenseIt#24](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt8/ExpenseItHome.xaml?range=13,43-46,49)]

    データテンプレートの詳細については、「データテンプレートの[概要](../data/data-templating-overview.md)」を参照してください。

3. 既存の <xref:System.Windows.Controls.ListBox> を次の XAML に置き換えます。

    [!code-xaml[ExpenseIt#25](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt8/ExpenseItHome.xaml#25)]

    この XAML は、<xref:System.Windows.Controls.ListBox> の <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> プロパティをデータソースにバインドし、データテンプレートを <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A>として適用します。

## <a name="connect-data-to-controls"></a>コントロールへのデータの接続

次に、[ **`ExpenseItHome`** ] ページで選択されている名前を取得して、 **[ページの追加] のコンストラクター**に渡すコードを追加します。 [調整された**Sereportpage** ] は、渡された項目を使用してそのデータコンテキストを設定します。これは、"*ページ*のバインド先" に定義されているコントロールです。

1. *ExpenseReportPage.xaml.vb* または *ExpenseReportPage.xaml.cs*を開きます。

2. オブジェクトを取得するコンストラクターを追加して、選択した個人の経費報告書データを渡せるようにします。

    [!code-csharp[ExpenseIt#26](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt8/ExpenseReportPage.xaml.cs#26)]
    [!code-vb[ExpenseIt#26](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ExpenseIt/VB/ExpenseIt8/ExpenseReportPage.xaml.vb#26)]

3. *`ExpenseItHome.xaml.vb`* または *`ExpenseItHome.xaml.cs`* を開きます。

4. 選択したユーザーの経費報告書データを渡す新しいコンストラクターを呼び出すように、<xref:System.Windows.Controls.Primitives.ButtonBase.Click> イベントハンドラーを変更します。

    [!code-csharp[ExpenseIt#27](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt8/ExpenseItHome.xaml.cs#27)]
    [!code-vb[ExpenseIt#27](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ExpenseIt/VB/ExpenseIt8/ExpenseItHome.xaml.vb#27)]

## <a name="style-data-with-data-templates"></a>データテンプレートを使用したデータのスタイル

このセクションでは、データテンプレートを使用して、データバインドリストの各項目の UI を更新します。

1. *ExpenseReportPage.xaml*を開きます。

2. "Name" および "Department" <xref:System.Windows.Controls.Label> 要素の内容を適切なデータソースプロパティにバインドします。 データバインディングの詳細については、「[データバインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)」を参照してください。

    [!code-xaml[ExpenseIt#31](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt9/ExpenseReportPage.xaml#31)]

3. 開いている <xref:System.Windows.Controls.Grid> 要素の後に、経費報告書データの表示方法を定義する次のデータテンプレートを追加します。

    [!code-xaml[ExpenseIt#30](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt9/ExpenseReportPage.xaml#30)]

4. <xref:System.Windows.Controls.DataGridTextColumn> 要素を <xref:System.Windows.Controls.DataGrid> 要素の下の <xref:System.Windows.Controls.DataGridTemplateColumn> に置き換え、テンプレートを適用します。

    [!code-xaml[ExpenseIt#32](~/samples/snippets/csharp/VS_Snippets_Wpf/ExpenseIt/CSharp/ExpenseIt9/ExpenseReportPage.xaml#32)]

5. アプリケーションをビルドして実行します。

6. ユーザーを選択し、 **[表示]** ボタンを選択します。

次の図は、コントロール、レイアウト、スタイル、データバインディング、適用されたデータテンプレートを適用した `ExpenseIt` アプリケーションの両方のページを示しています。

![名前の一覧と経費報告書を表示するアプリの両方のページ。](./media/walkthrough-my-first-wpf-desktop-application/application-data-templates.png)

> [!NOTE]
> このサンプルでは、WPF の特定の機能について説明します。セキュリティ、ローカライズ、アクセシビリティなどのベストプラクティスについては説明しません。 WPF と .NET アプリ開発のベストプラクティスの包括的な説明については、次のトピックを参照してください。
>
> - [ユーザー補助](../../ui-automation/accessibility-best-practices.md)
> - [セキュリティ](../security-wpf.md)
> - [WPF のグローバリゼーションとローカライズ](../advanced/wpf-globalization-and-localization-overview.md)
> - [WPF のパフォーマンス](../advanced/optimizing-wpf-application-performance.md)

## <a name="next-steps"></a>次のステップ

このチュートリアルでは、Windows Presentation Foundation (WPF) を使用して UI を作成するためのいくつかの手法について学習しました。 これで、データバインドされた .NET アプリの構成要素についての基本的な理解ができました。 WPF のアーキテクチャおよびプログラミング モデルの詳細については、次のトピックを参照してください。

- [WPF のアーキテクチャ](../advanced/wpf-architecture.md)
- [XAML の概要 (WPF)](../advanced/xaml-overview-wpf.md)
- [依存関係プロパティの概要](../advanced/dependency-properties-overview.md)
- [レイアウト](../advanced/layout.md)

アプリケーションの作成の詳細については、次のトピックを参照してください。

- [アプリケーション開発](../app-development/index.md)
- [コントロール](../controls/index.md)
- [データバインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [グラフィックスとマルチメディア](../graphics-multimedia/index.md)
- [WPF のドキュメント](../advanced/documents-in-wpf.md)

## <a name="see-also"></a>参照

- [パネルの概要](../controls/panels-overview.md)
- [データテンプレートの概要](../data/data-templating-overview.md)
- [WPF アプリケーションのビルド](../app-development/building-a-wpf-application-wpf.md)
- [スタイルとテンプレート](../controls/styles-and-templates.md)
