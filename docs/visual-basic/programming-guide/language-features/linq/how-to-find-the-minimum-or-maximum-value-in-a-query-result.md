---
title: '方法: LINQ を使用してクエリ結果の最小値または最大値を検索する'
ms.date: 07/20/2015
helpviewer_keywords:
- max operator [LINQ in Visual Basic]
- aggregate operator [LINQ in Visual Basic]
- aggregate queries
- minimum values [LINQ in Visual Basic]
- min operator [LINQ in Visual Basic]
- queries [LINQ in Visual Basic], minimum and maximum values
- Max property
- maximum values [LINQ in Visual Basic]
- Aggregate clause [Visual Basic]
- queries [LINQ in Visual Basic], aggregate queries
- queries [LINQ in Visual Basic], how-to topics
ms.assetid: 238b763b-7dcd-4b14-8050-b65500a4f71c
ms.openlocfilehash: bddb90baa0b01fd9d724583b472af9f9fa7569e5
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344977"
---
# <a name="how-to-find-the-minimum-or-maximum-value-in-a-query-result-by-using-linq-visual-basic"></a>方法 : LINQ を使用したクエリ結果内の最小値と最大値の検索 (Visual Basic)
統合言語クエリ (LINQ) を使用すると、データベース情報に簡単にアクセスしてクエリを実行できます。  
  
 次の例では、SQL Server データベースに対してクエリを実行する新しいアプリケーションを作成する方法を示します。 このサンプルでは、`Aggregate` 句と `Group By` 句を使用して、結果の最小値と最大値を決定します。 詳細については、「 [Aggregate 句](../../../../visual-basic/language-reference/queries/aggregate-clause.md)と[Group by 句](../../../../visual-basic/language-reference/queries/group-by-clause.md)」を参照してください。  
  
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
  
2. Customers テーブルをクリックし、デザイナーの左ペインにドラッグします。 [Orders] テーブルをクリックし、デザイナーの左ペインにドラッグします。  
  
     デザイナーによって、プロジェクトの新しい `Customer` と `Order` オブジェクトが作成されます。 デザイナーがテーブル間のリレーションシップを自動的に検出し、関連するオブジェクトの子プロパティを作成することに注意してください。 たとえば、IntelliSense は、`Customer` オブジェクトに、その顧客に関連するすべての注文に対して `Orders` プロパティがあることを示します。  
  
3. 変更を保存し、デザイナーを閉じます。  
  
4. プロジェクトを保存します。  
  
### <a name="to-add-code-to-query-the-database-and-display-the-results"></a>データベースに対してクエリを実行し、結果を表示するコードを追加するには  
  
1. **[ツールボックス]** から、[<xref:System.Windows.Forms.DataGridView>] コントロールをプロジェクト Form1 の既定の Windows フォームにドラッグします。  
  
2. Form1 をダブルクリックして、フォームの `Load` イベントにコードを追加します。  
  
3. O/R デザイナーにテーブルを追加したときに、デザイナーによってプロジェクトの <xref:System.Data.Linq.DataContext> オブジェクトが追加されました。 このオブジェクトには、各テーブルの個々のオブジェクトとコレクションに加えて、それらのテーブルにアクセスするために必要なコードが含まれています。 プロジェクトの <xref:System.Data.Linq.DataContext> オブジェクトは、.dbml ファイルの名前に基づいて名前が付けられます。 このプロジェクトでは、<xref:System.Data.Linq.DataContext> オブジェクトに `northwindDataContext`という名前が付けられています。  
  
     コード内に <xref:System.Data.Linq.DataContext> のインスタンスを作成し、O/R デザイナーによって指定されたテーブルに対してクエリを実行できます。  
  
     `Load` イベントに次のコードを追加します。 このコードは、データコンテキストのプロパティとして公開されているテーブルに対してクエリを実行し、結果の最小値と最大値を決定します。 このサンプルでは、`Aggregate` 句を使用して1つの結果をクエリし、`Group By` 句を使用してグループ化された結果の平均を表示します。  
  
     [!code-vb[VbLINQToSQLHowTos#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form7.vb#14)]  
  
4. F5 キーを押してプロジェクトを実行し、結果を表示します。  
  
## <a name="see-also"></a>参照

- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
- [クエリ](../../../../visual-basic/language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [DataContext メソッド (O/R デザイナー)](/visualstudio/data-tools/datacontext-methods-o-r-designer)
