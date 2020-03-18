---
title: 'チュートリアル: DataGrid コントロールで SQL Server データベースのデータを表示する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGrid [WPF], displaying data from SQL Server
- controls [WPF], DataGrid
ms.assetid: 6810b048-0a23-4f86-bfa5-97f92b3cfab4
ms.openlocfilehash: fc8b35c89e76a415529d76db687bc96767384e11
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452124"
---
# <a name="walkthrough-display-data-from-a-sql-server-database-in-a-datagrid-control"></a>チュートリアル: DataGrid コントロールでの SQL Server データベースのデータの表示

このチュートリアルでは、SQL Server データベースからデータを取得し、そのデータを <xref:System.Windows.Controls.DataGrid> コントロールに表示します。 データを表すエンティティクラスを作成するには、ADO.NET Entity Framework を使用します。また、LINQ を使用して、エンティティクラスから指定されたデータを取得するクエリを記述します。

## <a name="prerequisites"></a>前提条件

このチュートリアルを実行するには、次のコンポーネントが必要です。

- Visual Studio.

- AdventureWorks サンプルデータベースがアタッチされている SQL Server または SQL Server Express の実行中のインスタンスへのアクセス。 AdventureWorks データベースは[GitHub](https://github.com/Microsoft/sql-server-samples/releases)からダウンロードできます。

## <a name="create-entity-classes"></a>エンティティクラスの作成

1. Visual Basic またはC#で新しい WPF アプリケーションプロジェクトを作成し、`DataGridSQLExample`という名前を指定します。

2. ソリューションエクスプローラーで、プロジェクトを右クリックして **[追加]** をポイントし、 **[新しい項目]** をクリックします。

     [新しい項目の追加] ダイアログ ボックスが表示されます。

3. インストールされたテンプレート ペインで **データ** を選択し、テンプレートの一覧で **ADO.NET Entity Data Model** を選択します。

     ![ADO.NET Entity Data Model 項目テンプレート](../../wcf/feature-details/./media/ado-net-entity-data-model-item-template.png)

4. ファイルに `AdventureWorksModel.edmx` 名前を指定し、 **[追加]** をクリックします。

     Entity Data Model ウィザードが表示されます。

5. モデルのコンテンツの選択 画面で、**データベースから EF デザイナー** を選択し、**次へ** をクリックします。

6. [データ接続の選択] 画面で、AdventureWorksLT2008 データベースへの接続を指定します。 詳細については、「 [[データ接続の選択] ダイアログボックス](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb399244(v=vs.100))」を参照してください。

    名前が `AdventureWorksLT2008Entities` こと、 **[エンティティ接続設定を app.config に保存]** する チェックボックスがオンになっていることを確認し、 **[次へ]** をクリックします。

7. [データベースオブジェクトの選択] 画面で、[テーブル] ノードを展開し、 **product**テーブルと**productcategory**テーブルを選択します。

     すべてのテーブルに対してエンティティクラスを生成できます。ただし、この例では、これら2つのテーブルからのみデータを取得します。

     ![テーブルから Product および ProductCategory を選択する](./media/datagrid-sql-ef-step4.png "DataGrid_SQL_EF_Step4")

8. **[完了]** をクリックします。

     Product および ProductCategory エンティティが Entity Designer に表示されます。

     ![Product および ProductCategory エンティティモデル](./media/datagrid-sql-ef-step5.png "DataGrid_SQL_EF_Step5")

## <a name="retrieve-and-present-the-data"></a>データの取得と表示

1. Mainwindow.xaml ファイルを開きます。

2. <xref:System.Windows.Window> の <xref:System.Windows.FrameworkElement.Width%2A> プロパティを450に設定します。

3. XAML エディターで、`<Grid>` タグと `</Grid>` タグの間に次の <xref:System.Windows.Controls.DataGrid> タグを追加して、`dataGrid1`という名前の <xref:System.Windows.Controls.DataGrid> を追加します。

     [!code-xaml[DataGrid_SQL_EF_Walkthrough#3](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_SQL_EF_Walkthrough/CS/MainWindow.xaml#3)]

     ![DataGrid を含むウィンドウ](./media/datagrid-sql-ef-step6.png "DataGrid_SQL_EF_Step6")

4. <xref:System.Windows.Window> を選択します。

5. プロパティウィンドウまたは XAML エディターを使用して、<xref:System.Windows.FrameworkElement.Loaded> イベントに `Window_Loaded` という名前の <xref:System.Windows.Window> のイベントハンドラーを作成します。 詳細については、「[方法: 単純なイベントハンドラーを作成](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/bb675300(v=vs.100))する」を参照してください。

     Mainwindow.xaml の XAML を次に示します。

    > [!NOTE]
    > Visual Basic を使用する場合は、Mainwindow.xaml の最初の行で `x:Class="DataGridSQLExample.MainWindow"` を `x:Class="MainWindow"`に置き換えます。

     [!code-xaml[DataGrid_SQL_EF_Walkthrough#1](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_SQL_EF_Walkthrough/CS/MainWindow.xaml#1)]

6. <xref:System.Windows.Window>の分離コードファイル (Mainwindow.xaml または MainWindow.xaml.cs) を開きます。

7. 次のコードを追加して、結合されたテーブルから特定の値のみを取得し、<xref:System.Windows.Controls.DataGrid> の <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> プロパティをクエリの結果に設定します。

     [!code-csharp[DataGrid_SQL_EF_Walkthrough#2](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_SQL_EF_Walkthrough/CS/MainWindow.xaml.cs#2)]
     [!code-vb[DataGrid_SQL_EF_Walkthrough#2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DataGrid_SQL_EF_Walkthrough/VB/MainWindow.xaml.vb#2)]

8. 例を実行します。

     データを表示する <xref:System.Windows.Controls.DataGrid> が表示されます。

     ![SQL database のデータを使用した DataGrid](./media/datagrid-sql-ef-step7.png "DataGrid_SQL_EF_Step7")

## <a name="see-also"></a>参照

- <xref:System.Windows.Controls.DataGrid>
