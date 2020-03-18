---
title: 'チュートリアル : ハイブリッド アプリケーションでのデータへのバインディング'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- hybrid applications [WPF interoperability]
- data binding [WPF interoperability]
ms.assetid: 18997e71-745a-4425-9c69-2cbce1d8669e
ms.openlocfilehash: 0d1e66a1277e6a04d2f49ac91691160f70fb56e4
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095074"
---
# <a name="walkthrough-binding-to-data-in-hybrid-applications"></a>チュートリアル : ハイブリッド アプリケーションでのデータへのバインディング

データソースをコントロールにバインドすることは、Windows フォームと [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]のどちらを使用しているかにかかわらず、ユーザーが基になるデータにアクセスできるようにするために不可欠です。 このチュートリアルでは、Windows フォームと [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の両方のコントロールを含むハイブリッドアプリケーションでデータバインディングを使用する方法について説明します。

このチュートリアルでは、以下のタスクを行います。

- プロジェクトの作成。

- データテンプレートを定義しています。

- フォームレイアウトを指定します。

- データバインディングを指定します。

- 相互運用を使用してデータを表示する。

- プロジェクトにデータソースを追加しています。

- データソースにバインドしています。

このチュートリアルで示すタスクの完全なコード一覧については、「[ハイブリッドアプリケーションでのデータバインディングのサンプル](https://github.com/microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/WPFWithWFAndDatabinding)」を参照してください。

完了すると、ハイブリッドアプリケーションのデータバインディング機能について理解できるようになります。

## <a name="prerequisites"></a>前提条件

このチュートリアルを実行するには、次のコンポーネントが必要です。

- 見ることができます。

- Microsoft SQL Server で実行されている Northwind サンプルデータベースへのアクセス。

## <a name="creating-the-project"></a>プロジェクトの作成

### <a name="to-create-and-set-up-the-project"></a>プロジェクトを作成し、設定するには

1. `WPFWithWFAndDatabinding`という名前の WPF アプリケーションプロジェクトを作成します。

2. ソリューション エクスプローラーで、次のアセンブリへの参照を追加します。

    - WindowsFormsIntegration

    - System.Windows.Forms

3. WPF デザイナーで Mainwindow.xaml を開きます。

4. <xref:System.Windows.Window> 要素に、次の Windows フォーム名前空間マッピングを追加します。

    ```xaml
    xmlns:wf="clr-namespace:System.Windows.Forms;assembly=System.Windows.Forms"
    ```

5. <xref:System.Windows.FrameworkElement.Name%2A> プロパティを割り当てることによって、既定の <xref:System.Windows.Controls.Grid> 要素 `mainGrid` に名前を指定します。

     [!code-xaml[WPFWithWFAndDatabinding#8](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFWithWFAndDatabinding/CSharp/WPFWithWFAndDatabinding/Window1.xaml#8)]

## <a name="defining-the-data-template"></a>データテンプレートの定義

顧客のマスターリストが <xref:System.Windows.Controls.ListBox> コントロールに表示されます。 次のコード例では、<xref:System.Windows.Controls.ListBox> コントロールのビジュアルツリーを制御する `ListItemsTemplate` という名前の <xref:System.Windows.DataTemplate> オブジェクトを定義します。 この <xref:System.Windows.DataTemplate> は、<xref:System.Windows.Controls.ListBox> コントロールの <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> プロパティに割り当てられます。

### <a name="to-define-the-data-template"></a>データテンプレートを定義するには

- 次の XAML を <xref:System.Windows.Controls.Grid> 要素の宣言にコピーします。

     [!code-xaml[WPFWithWFAndDatabinding#3](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFWithWFAndDatabinding/CSharp/WPFWithWFAndDatabinding/Window1.xaml#3)]

## <a name="specifying-the-form-layout"></a>フォームレイアウトの指定

フォームのレイアウトは、3つの行と3つの列を持つグリッドによって定義されます。 Customers テーブルの各列を識別するために、<xref:System.Windows.Controls.Label> コントロールが用意されています。

### <a name="to-set-up-the-grid-layout"></a>グリッドレイアウトを設定するには

- 次の XAML を <xref:System.Windows.Controls.Grid> 要素の宣言にコピーします。

     [!code-xaml[WPFWithWFAndDatabinding#4](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFWithWFAndDatabinding/CSharp/WPFWithWFAndDatabinding/Window1.xaml#4)]

### <a name="to-set-up-the-label-controls"></a>ラベルコントロールを設定するには

- 次の XAML を <xref:System.Windows.Controls.Grid> 要素の宣言にコピーします。

     [!code-xaml[WPFWithWFAndDatabinding#5](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFWithWFAndDatabinding/CSharp/WPFWithWFAndDatabinding/Window1.xaml#5)]

## <a name="specifying-data-bindings"></a>データバインディングの指定

顧客のマスターリストが <xref:System.Windows.Controls.ListBox> コントロールに表示されます。 アタッチされた `ListItemsTemplate` は、<xref:System.Windows.Controls.TextBlock> コントロールをデータベースの `ContactName` フィールドにバインドします。

各顧客レコードの詳細が、いくつかの <xref:System.Windows.Controls.TextBox> コントロールに表示されます。

### <a name="to-specify-data-bindings"></a>データバインディングを指定するには

- 次の XAML を <xref:System.Windows.Controls.Grid> 要素の宣言にコピーします。

     <xref:System.Windows.Data.Binding> クラスは、<xref:System.Windows.Controls.TextBox> コントロールをデータベース内の適切なフィールドにバインドします。

     [!code-xaml[WPFWithWFAndDatabinding#6](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFWithWFAndDatabinding/CSharp/WPFWithWFAndDatabinding/Window1.xaml#6)]

## <a name="displaying-data-by-using-interoperation"></a>相互運用を使用したデータの表示

選択した顧客に対応する注文が `dataGridView1`という名前の <xref:System.Windows.Forms.DataGridView?displayProperty=nameWithType> コントロールに表示されます。 `dataGridView1` コントロールは、分離コードファイル内のデータソースにバインドされます。 <xref:System.Windows.Forms.Integration.WindowsFormsHost> コントロールは、この Windows フォームコントロールの親です。

### <a name="to-display-data-in-the-datagridview-control"></a>DataGridView コントロールにデータを表示するには

- 次の XAML を <xref:System.Windows.Controls.Grid> 要素の宣言にコピーします。

     [!code-xaml[WPFWithWFAndDatabinding#7](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFWithWFAndDatabinding/CSharp/WPFWithWFAndDatabinding/Window1.xaml#7)]

## <a name="adding-the-data-source-to-the-project"></a>プロジェクトへのデータソースの追加

Visual Studio を使用すると、データソースを簡単にプロジェクトに追加できます。 この手順では、厳密に型指定されたデータセットをプロジェクトに追加します。 選択した各テーブルのテーブルアダプターなど、他のいくつかのサポートクラスも追加されています。

### <a name="to-add-the-data-source"></a>データ ソースを追加するには

1. **[データ]** メニューの **[新しいデータソースの追加]** をクリックします。

2. **データソース構成ウィザード**で、データセットを使用して Northwind データベースへの接続を作成します。 詳細については、「 [How to: Connect to Data in a Database](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/fxk9yw1t(v=vs.120))」を参照してください。

3. **データソース構成ウィザード**によってメッセージが表示されたら、`NorthwindConnectionString`として接続文字列を保存します。

4. データベースオブジェクトの選択を求めるメッセージが表示されたら、`Customers` テーブルと `Orders` テーブルを選択し、生成されたデータセットに `NorthwindDataSet`という名前を指定します。

## <a name="binding-to-the-data-source"></a>データソースへのバインド

<xref:System.Windows.Forms.BindingSource?displayProperty=nameWithType> コンポーネントは、アプリケーションのデータソースに対して統一されたインターフェイスを提供します。 データソースへのバインドは、分離コードファイルに実装されています。

### <a name="to-bind-to-the-data-source"></a>データソースにバインドするには

1. Mainwindow.xaml または MainWindow.xaml.cs という名前の分離コードファイルを開きます。

2. 次のコードを `MainWindow` クラス定義にコピーします。

     このコードは、データベースに接続する <xref:System.Windows.Forms.BindingSource> コンポーネントと関連付けられたヘルパークラスを宣言します。

     [!code-csharp[WPFWithWFAndDatabinding#11](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFWithWFAndDatabinding/CSharp/WPFWithWFAndDatabinding/Window1.xaml.cs#11)]
     [!code-vb[WPFWithWFAndDatabinding#11](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFWithWFAndDatabinding/VisualBasic/WPFWithWFAndDatabinding/Window1.xaml.vb#11)]

3. 次のコードをコンストラクターにコピーします。

     このコードでは、<xref:System.Windows.Forms.BindingSource> コンポーネントを作成して初期化します。

     [!code-csharp[WPFWithWFAndDatabinding#12](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFWithWFAndDatabinding/CSharp/WPFWithWFAndDatabinding/Window1.xaml.cs#12)]
     [!code-vb[WPFWithWFAndDatabinding#12](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFWithWFAndDatabinding/VisualBasic/WPFWithWFAndDatabinding/Window1.xaml.vb#12)]

4. MainWindow.xaml を開きます。

5. デザインビューまたは XAML ビューで、<xref:System.Windows.Window> 要素を選択します。

6. プロパティウィンドウで、 **[イベント]** タブをクリックします。

7. <xref:System.Windows.FrameworkElement.Loaded> イベントをダブルクリックします。

8. 次のコードを <xref:System.Windows.FrameworkElement.Loaded> イベントハンドラーにコピーします。

     このコードでは、<xref:System.Windows.Forms.BindingSource> コンポーネントをデータコンテキストとして割り当て、`Customers` および `Orders` アダプターオブジェクトを設定します。

     [!code-csharp[WPFWithWFAndDatabinding#13](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFWithWFAndDatabinding/CSharp/WPFWithWFAndDatabinding/Window1.xaml.cs#13)]
     [!code-vb[WPFWithWFAndDatabinding#13](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFWithWFAndDatabinding/VisualBasic/WPFWithWFAndDatabinding/Window1.xaml.vb#13)]

9. 次のコードを `MainWindow` クラス定義にコピーします。

     このメソッドは、<xref:System.Windows.Data.CollectionView.CurrentChanged> イベントを処理し、データバインディングの現在の項目を更新します。

     [!code-csharp[WPFWithWFAndDatabinding#14](~/samples/snippets/csharp/VS_Snippets_Wpf/WPFWithWFAndDatabinding/CSharp/WPFWithWFAndDatabinding/Window1.xaml.cs#14)]
     [!code-vb[WPFWithWFAndDatabinding#14](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WPFWithWFAndDatabinding/VisualBasic/WPFWithWFAndDatabinding/Window1.xaml.vb#14)]

10. F5 キーを押して、アプリケーションをビルドして実行します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [Visual Studio で XAML をデザインする](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)
- [ハイブリッドアプリケーションでのデータバインディングのサンプル](https://github.com/microsoft/WPF-Samples/tree/master/Migration%20and%20Interoperability/WPFWithWFAndDatabinding)
- [チュートリアル: WPF での Windows フォーム複合コントロールのホスト](walkthrough-hosting-a-windows-forms-composite-control-in-wpf.md)
- [チュートリアル: Windows フォームでの WPF 複合コントロールのホスト](walkthrough-hosting-a-wpf-composite-control-in-windows-forms.md)
