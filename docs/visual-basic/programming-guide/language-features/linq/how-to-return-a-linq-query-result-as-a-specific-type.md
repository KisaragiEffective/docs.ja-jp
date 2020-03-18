---
title: '方法 : 特定の型での LINQ クエリ結果の取得'
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], specific type returned
- projection [LINQ in Visual Basic]
- anonymous types [Visual Basic]
- querying databases [LINQ]
- queries [LINQ in Visual Basic], how-to topics
- query samples [Visual Basic]
ms.assetid: 621bb10a-e5d7-44fb-a025-317964b19d92
ms.openlocfilehash: 1084743b0c50d381375b76a3116ed7c9e6f9d769
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354200"
---
# <a name="how-to-return-a-linq-query-result-as-a-specific-type-visual-basic"></a>方法 : LINQ クエリ結果を特定の型で返す (Visual Basic)
統合言語クエリ (LINQ) を使用すると、データベース情報に簡単にアクセスしてクエリを実行できます。 既定では、LINQ クエリは、オブジェクトの一覧を匿名型として返します。 また、`Select` 句を使用して、クエリが特定の型のリストを返すように指定することもできます。  
  
 次の例では、SQL Server データベースに対してクエリを実行する新しいアプリケーションを作成し、その結果を特定の名前付きの型として射影する方法を示します。 詳細については、「[匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)」と「 [Select 句](../../../../visual-basic/language-reference/queries/select-clause.md)」を参照してください。  
  
 このトピックの例では、Northwind サンプルデータベースを使用します。 開発用コンピューターにこのデータベースがない場合は、Microsoft ダウンロード センターからダウンロードします。 手順については、「[サンプルデータベースのダウンロード](../../../../framework/data/adonet/sql/linq/downloading-sample-databases.md)」を参照してください。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
### <a name="to-create-a-connection-to-a-database"></a>データベースへの接続を作成するには  
  
1. Visual Studio で**サーバーエクスプローラー**/**データベースエクスプローラー**を開くには、 **[表示]** メニューの [**サーバーエクスプローラー** **/データベースエクスプローラー** ] をクリックします。  
  
2. **サーバーエクスプローラー**/で **[データ接続]** を右クリックし**データベースエクスプローラー** **[接続の追加]** をクリックします。  
  
3. Northwind サンプルデータベースへの有効な接続を指定します。  
  
### <a name="to-add-a-project-that-contains-a-linq-to-sql-file"></a>LINQ to SQL ファイルを含むプロジェクトを追加するには  
  
1. Visual Studio で、 **[ファイル]** メニューの **[新規作成]** をポイントし、 **[プロジェクト]** をクリックします。 プロジェクトの種類として [Visual Basic **Windows フォームアプリケーション**] を選択します。  
  
2. **[プロジェクト]** メニューの **[新しい項目の追加]** をクリックします。 **[LINQ to SQL Classes]** 項目テンプレートを選択します。  
  
3. そのファイルに `northwind.dbml` という名前を付けます。 **[追加]** をクリックします。 Northwind .dbml ファイルのオブジェクトリレーショナルデザイナー (O/R デザイナー) が開きます。  
  
### <a name="to-add-tables-to-query-to-the-or-designer"></a>O/R デザイナーに対してクエリを実行するテーブルを追加するには  
  
1. **サーバーエクスプローラー**/**データベースエクスプローラー**で、Northwind データベースへの接続を展開します。 **[テーブル]** フォルダーを展開します。  
  
     O/R デザイナーを閉じた場合は、前の手順で追加した northwind .dbml ファイルをダブルクリックして再度開くことができます。  
  
2. Customers テーブルをクリックし、デザイナーの左ペインにドラッグします。  
  
     デザイナーによって、プロジェクトの新しい `Customer` オブジェクトが作成されます。 クエリ結果は、`Customer` の種類として、または作成する型として射影できます。 このサンプルでは、後の手順で新しい型を作成し、その型としてクエリ結果を射影します。  
  
3. 変更を保存し、デザイナーを閉じます。  
  
4. プロジェクトを保存します。  
  
### <a name="to-add-code-to-query-the-database-and-display-the-results"></a>データベースに対してクエリを実行し、結果を表示するコードを追加するには  
  
1. **[ツールボックス]** から、[<xref:System.Windows.Forms.DataGridView>] コントロールをプロジェクト Form1 の既定の Windows フォームにドラッグします。  
  
2. Form1 をダブルクリックして、Form1 クラスを変更します。  
  
3. Form1 クラスの `End Class` ステートメントの後に、次のコードを追加して、このサンプルのクエリ結果を保持する `CustomerInfo` 型を作成します。  
  
     [!code-vb[VbLINQToSQLHowTos#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form8.vb#16)]  
  
4. O/R デザイナーにテーブルを追加すると、デザイナーによって <xref:System.Data.Linq.DataContext> オブジェクトがプロジェクトに追加されます。 このオブジェクトには、これらのテーブルにアクセスし、各テーブルの個々のオブジェクトとコレクションにアクセスするために必要なコードが含まれています。 プロジェクトの <xref:System.Data.Linq.DataContext> オブジェクトは、.dbml ファイルの名前に基づいて名前が付けられます。 このプロジェクトでは、<xref:System.Data.Linq.DataContext> オブジェクトに `northwindDataContext`という名前が付けられています。  
  
     コード内に <xref:System.Data.Linq.DataContext> のインスタンスを作成し、O/R デザイナーによって指定されたテーブルに対してクエリを実行できます。  
  
     Form1 クラスの `Load` イベントで、次のコードを追加して、データコンテキストのプロパティとして公開されているテーブルに対してクエリを実行します。 クエリの `Select` 句は、クエリ結果の各項目に対して匿名型ではなく、新しい `CustomerInfo` 型を作成します。  
  
     [!code-vb[VbLINQToSQLHowTos#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form8.vb#15)]  
  
5. F5 キーを押してプロジェクトを実行し、結果を表示します。  
  
## <a name="see-also"></a>関連項目

- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
- [クエリ](../../../../visual-basic/language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [DataContext メソッド (O/R デザイナー)](/visualstudio/data-tools/datacontext-methods-o-r-designer)
