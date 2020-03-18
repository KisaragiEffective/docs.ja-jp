---
title: '方法: LINQ を使用してデータベース内のデータを変更する'
ms.date: 07/20/2015
helpviewer_keywords:
- inserting rows [LINQ to SQL]
- deleting rows [LINQ to SQL]
- updating rows [LINQ to SQL]
- inserting data [Visual Basic]
- deleting data
- data [Visual Basic], updating
- updating data [LINQ]
- queries [LINQ in Visual Basic], data changes in database
- queries [LINQ in Visual Basic], how-to topics
ms.assetid: cf52635f-0c1b-46c3-aff1-bdf181cf19b1
ms.openlocfilehash: 9a10efef5ae92dd21888594ae80a3fc07869a8c0
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344943"
---
# <a name="how-to-modify-data-in-a-database-by-using-linq-visual-basic"></a>方法 : LINQ を使用してデータベースのデータを変更する (Visual Basic)

統合言語クエリ (LINQ) クエリを使用すると、データベースの情報にアクセスし、データベース内の値を変更することが簡単にできます。

次の例では、SQL Server データベースの情報を取得および更新する新しいアプリケーションを作成する方法を示します。

このトピックの例では、Northwind サンプルデータベースを使用します。 開発用コンピューターにこのデータベースがない場合は、Microsoft ダウンロード センターからダウンロードします。 手順については、「[サンプルデータベースのダウンロード](../../../../framework/data/adonet/sql/linq/downloading-sample-databases.md)」を参照してください。

### <a name="to-create-a-connection-to-a-database"></a>データベースへの接続を作成するには

1. Visual Studio で **[表示]** メニューをクリックして**サーバーエクスプローラー**/**データベースエクスプローラー**を開き、[**サーバーエクスプローラー**/**データベースエクスプローラー**] を選択します。

2. **サーバーエクスプローラー**/**データベースエクスプローラー**で **[データ接続]** を右クリックし、 **[接続の追加]** をクリックします。

3. Northwind サンプルデータベースへの有効な接続を指定します。

### <a name="to-add-a-project-with-a-linq-to-sql-file"></a>LINQ to SQL ファイルを含むプロジェクトを追加するには

1. Visual Studio で、 **[ファイル]** メニューの **[新規作成]** をポイントし、 **[プロジェクト]** をクリックします。 プロジェクトの種類として [Visual Basic **Windows フォームアプリケーション**] を選択します。

2. **[プロジェクト]** メニューの **[新しい項目の追加]** をクリックします。 **[LINQ to SQL Classes]** 項目テンプレートを選択します。

3. そのファイルに `northwind.dbml` という名前を付けます。 **[追加]** をクリックします。 `northwind.dbml` ファイルのオブジェクトリレーショナルデザイナー (O/R デザイナー) が開きます。

### <a name="to-add-tables-to-query-and-modify-to-the-designer"></a>デザイナーに対してクエリおよび変更するテーブルを追加するには

1. **サーバーエクスプローラー**/**データベースエクスプローラー**で、Northwind データベースへの接続を展開します。 **[テーブル]** フォルダーを展開します。

     O/R デザイナーを閉じた場合は、前の手順で追加した `northwind.dbml` ファイルをダブルクリックして再度開くことができます。

2. Customers テーブルをクリックし、デザイナーの左ペインにドラッグします。

     デザイナーによって、プロジェクトの新しい Customer オブジェクトが作成されます。

3. 変更を保存し、デザイナーを閉じます。

4. プロジェクトを保存します。

### <a name="to-add-code-to-modify-the-database-and-display-the-results"></a>データベースを変更して結果を表示するコードを追加するには

1. **[ツールボックス]** から、[<xref:System.Windows.Forms.DataGridView>] コントロールをプロジェクト Form1 の既定の Windows フォームにドラッグします。

2. O/R デザイナーにテーブルを追加すると、デザイナーによって <xref:System.Data.Linq.DataContext> オブジェクトがプロジェクトに追加されます。 このオブジェクトには、Customers テーブルにアクセスするために使用できるコードが含まれています。 また、ローカルの Customer オブジェクトとテーブルの Customers コレクションを定義するコードも含まれています。 プロジェクトの <xref:System.Data.Linq.DataContext> オブジェクトは、.dbml ファイルの名前に基づいて名前が付けられます。 このプロジェクトでは、<xref:System.Data.Linq.DataContext> オブジェクトに `northwindDataContext`という名前が付けられています。

     コード内に <xref:System.Data.Linq.DataContext> オブジェクトのインスタンスを作成し、O/R デザイナーによって指定された Customers コレクションに対してクエリを実行し、変更することができます。 Customers コレクションに加えた変更は、<xref:System.Data.Linq.DataContext> オブジェクトの <xref:System.Data.Linq.DataContext.SubmitChanges%2A> メソッドを呼び出すことによって送信されるまで、データベースに反映されません。

     Windows フォームの Form1 をダブルクリックして、<xref:System.Windows.Forms.Form.Load> イベントにコードを追加し、<xref:System.Data.Linq.DataContext>のプロパティとして公開されている Customers テーブルに対してクエリを実行します。 次のコードを追加します。

    ```vb
    Private db As northwindDataContext

    Private Sub Form1_Load(ByVal sender As System.Object,
                           ByVal e As System.EventArgs
                          ) Handles MyBase.Load
      db = New northwindDataContext()

      RefreshData()
    End Sub

    Private Sub RefreshData()
      Dim customers = From cust In db.Customers
                      Where cust.City(0) = "W"
                      Select cust

      DataGridView1.DataSource = customers
    End Sub
    ```

3. **ツールボックス**から、3つの <xref:System.Windows.Forms.Button> コントロールをフォームにドラッグします。 最初の `Button` コントロールを選択します。 **[プロパティ]** ウィンドウで、`Button` コントロールの `Name` を `AddButton` に設定し、`Text` を `Add`に設定します。 2番目のボタンを選択し、[`Name`] プロパティを [`UpdateButton`] に設定し、[`Text`] プロパティを [`Update`] に設定します。 3番目のボタンを選択し、`Name` プロパティを `DeleteButton` に設定し、`Text` プロパティを `Delete`に設定します。

4. **[追加]** ボタンをダブルクリックして、`Click` イベントにコードを追加します。 次のコードを追加します。

    ```vb
    Private Sub AddButton_Click(ByVal sender As System.Object,
                                ByVal e As System.EventArgs
                               ) Handles AddButton.Click
      Dim cust As New Customer With {
        .City = "Wellington",
        .CompanyName = "Blue Yonder Airlines",
        .ContactName = "Jill Frank",
        .Country = "New Zealand",
        .CustomerID = "JILLF"}

      db.Customers.InsertOnSubmit(cust)

      Try
        db.SubmitChanges()
      Catch
        ' Handle exception.
      End Try

      RefreshData()
    End Sub
    ```

5. **[更新]** ボタンをダブルクリックして、`Click` イベントにコードを追加します。 次のコードを追加します。

    ```vb
    Private Sub UpdateButton_Click(ByVal sender As System.Object, _
                                   ByVal e As System.EventArgs
                                  ) Handles UpdateButton.Click
      Dim updateCust = (From cust In db.Customers
                        Where cust.CustomerID = "JILLF").ToList()(0)

      updateCust.ContactName = "Jill Shrader"
      updateCust.Country = "Wales"
      updateCust.CompanyName = "Red Yonder Airlines"
      updateCust.City = "Cardiff"

      Try
        db.SubmitChanges()
      Catch
        ' Handle exception.
      End Try

      RefreshData()
    End Sub
    ```

6. **[削除]** ボタンをダブルクリックして、`Click` イベントにコードを追加します。 次のコードを追加します。

    ```vb
    Private Sub DeleteButton_Click(ByVal sender As System.Object, _
                                   ByVal e As System.EventArgs
                                  ) Handles DeleteButton.Click
      Dim deleteCust = (From cust In db.Customers
                        Where cust.CustomerID = "JILLF").ToList()(0)

      db.Customers.DeleteOnSubmit(deleteCust)

      Try
        db.SubmitChanges()
      Catch
        ' Handle exception.
      End Try

      RefreshData()
    End Sub
    ```

7. F5 キーを押してプロジェクトを実行します。 新しいレコードを追加するには、 **[追加]** をクリックします。 新しいレコードを変更するには、 **[更新]** をクリックします。 新しいレコードを削除するには、 **[削除]** をクリックします。

## <a name="see-also"></a>参照

- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
- [クエリ](../../../../visual-basic/language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [DataContext メソッド (O/R デザイナー)](/visualstudio/data-tools/datacontext-methods-o-r-designer)
- [方法: 更新、挿入、および削除を実行するストアド プロシージャを割り当てる (O/R デザイナー)](/visualstudio/data-tools/how-to-assign-stored-procedures-to-perform-updates-inserts-and-deletes-o-r-designer)
