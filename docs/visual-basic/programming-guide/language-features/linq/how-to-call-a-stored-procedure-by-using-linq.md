---
title: '方法: LINQ を使用してストアドプロシージャを呼び出す'
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], stored procedure calls
- stored procedures sample [Visual Basic]
- stored procedures [LINQ to SQL]
- queries [LINQ in Visual Basic], how-to topics
ms.assetid: 6436d384-d1e0-40aa-8afd-451007477260
ms.openlocfilehash: f91ccda1842887b3785ce304fd41bdd020a55479
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345020"
---
# <a name="how-to-call-a-stored-procedure-by-using-linq-visual-basic"></a>方法 : LINQ を使用してストアド プロシージャを呼び出す (Visual Basic)
統合言語クエリ (LINQ) を使用すると、ストアドプロシージャなどのデータベースオブジェクトを含むデータベース情報に簡単にアクセスできます。  
  
 次の例では、SQL Server データベースでストアドプロシージャを呼び出すアプリケーションを作成する方法を示します。 このサンプルでは、データベースで2つの異なるストアドプロシージャを呼び出す方法を示します。 各プロシージャは、クエリの結果を返します。 1つのプロシージャは入力パラメーターを受け取り、もう一方のプロシージャはパラメーターを受け取りません。  
  
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
  
### <a name="to-add-stored-procedures-to-the-or-designer"></a>ストアドプロシージャを O/R デザイナーに追加するには  
  
1. **サーバーエクスプローラー**/**データベースエクスプローラー**で、Northwind データベースへの接続を展開します。 **[ストアドプロシージャ]** フォルダーを展開します。  
  
     O/R デザイナーを閉じた場合は、前の手順で追加した northwind .dbml ファイルをダブルクリックして再度開くことができます。  
  
2. **[Sales By Year]** ストアドプロシージャをクリックし、デザイナーの右ペインにドラッグします。 **[最もコストの高い10個の製品]** ストアドプロシージャをクリックして、デザイナーの右ペインにドラッグします。  
  
3. 変更を保存し、デザイナーを閉じます。  
  
4. プロジェクトを保存します。  
  
### <a name="to-add-code-to-display-the-results-of-the-stored-procedures"></a>ストアドプロシージャの結果を表示するコードを追加するには  
  
1. **[ツールボックス]** から、[<xref:System.Windows.Forms.DataGridView>] コントロールをプロジェクト Form1 の既定の Windows フォームにドラッグします。  
  
2. Form1 をダブルクリックして、`Load` イベントにコードを追加します。  
  
3. ストアドプロシージャを O/R デザイナーに追加したときに、デザイナーによってプロジェクトの <xref:System.Data.Linq.DataContext> オブジェクトが追加されました。 このオブジェクトには、これらのプロシージャにアクセスするために必要なコードが含まれています。 プロジェクトの <xref:System.Data.Linq.DataContext> オブジェクトの名前は、.dbml ファイルの名前に基づいて付けられます。 このプロジェクトでは、<xref:System.Data.Linq.DataContext> オブジェクトに `northwindDataContext`という名前が付けられています。  
  
     コード内に <xref:System.Data.Linq.DataContext> のインスタンスを作成し、O/R デザイナーによって指定されたストアドプロシージャメソッドを呼び出すことができます。 <xref:System.Windows.Forms.DataGridView> オブジェクトにバインドするには、ストアドプロシージャの結果に対して <xref:System.Linq.Enumerable.ToList%2A> メソッドを呼び出すことによって、クエリの実行を直ちに強制する必要がある場合があります。  
  
     次のコードを `Load` イベントに追加して、データコンテキストのメソッドとして公開されているストアドプロシージャのいずれかを呼び出すことができます。  
  
     [!code-vb[VbLINQtoSQLHowTos#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form3.vb#1)]  
    [!code-vb[VbLINQtoSQLHowTos#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form3.vb#2)]  
  
4. F5 キーを押してプロジェクトを実行し、結果を表示します。  
  
## <a name="see-also"></a>関連項目

- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
- [クエリ](../../../../visual-basic/language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [DataContext メソッド (O/R デザイナー)](/visualstudio/data-tools/datacontext-methods-o-r-designer)
- [方法: 更新、挿入、および削除を実行するストアド プロシージャを割り当てる (O/R デザイナー)](/visualstudio/data-tools/how-to-assign-stored-procedures-to-perform-updates-inserts-and-deletes-o-r-designer)
