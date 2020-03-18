---
title: LINQ の概要
ms.date: 08/28/2018
helpviewer_keywords:
- queries [LINQ in Visual Basic], about LINQ in Visual Basic queries
- query execution [LINQ in Visual Basic]
- range variables [Visual Basic]
- LINQ [Visual Basic]
- LINQ [Visual Basic], about LINQ in Visual Basic
- query expressions [LINQ in Visual Basic]
- LINQ
- deferred execution
- iteration variables [Visual Basic]
ms.assetid: 3047d86e-0d49-40e2-928b-dc02e46c7984
ms.openlocfilehash: 3f58edf326ab9415d78d7065d74d8c1954fbbf37
ms.sourcegitcommit: 09b4090b78f52fd09b0e430cd4b26576f1fdf96e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2020
ms.locfileid: "76315864"
---
# <a name="introduction-to-linq-in-visual-basic"></a>Visual Basic における LINQ の概要
統合言語クエリ (LINQ) では、Visual Basic にクエリ機能を追加し、あらゆる種類のデータを操作するときに、シンプルで強力な機能を提供します。 LINQ では、クエリをデータベースに送信して処理したり、検索対象のデータの種類ごとに異なるクエリ構文を使用したりするのではなく、Visual Basic 言語の一部としてクエリを導入します。 LINQ では、データの型に関係なく、統一された構文を使用します。  
  
 LINQ を使用すると、SQL Server データベース、XML、メモリ内の配列とコレクション、ADO.NET データセット、または LINQ をサポートするその他のリモートまたはローカルデータソースのデータに対してクエリを実行できます。 これは、共通の Visual Basic 言語要素を使用して行うことができます。 クエリは Visual Basic 言語で記述されているため、クエリ結果は厳密に型指定されたオブジェクトとして返されます。 これらのオブジェクトは IntelliSense をサポートするので、コードの記述時間を短縮でき、さらに、クエリに含まれるエラーを実行時ではなくコンパイル時に把握できます。 LINQ クエリは、結果を絞り込むための追加クエリのソースとして使用できます。 クエリ結果をユーザーが簡単に表示および変更できるように、コントロールにバインディングすることもできます。  
  
 たとえば、次のコード例は、コレクションから顧客リストを返し、住所に基づいて顧客をグループ化する LINQ クエリを示しています。  
  
 [!code-vb[VbVbalrIntroToLINQ#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#1)]  
  
## <a name="running-the-examples"></a>例の実行  
 概要と[LINQ クエリセクションの構造](#structure-of-a-linq-query)で例を実行するには、顧客と注文の一覧を返す次のコードを追加します。  
  
 [!code-vb[VbVbalrIntroToLINQ#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#31)]  
  
## <a name="linq-providers"></a>LINQ プロバイダー  
 *Linq プロバイダー*は、クエリ対象のデータソースに Visual Basic linq クエリをマップします。 LINQ クエリを記述すると、そのクエリは、プロバイダーによって、データ ソースが実行できるコマンドに変換されます。 プロバイダーは、ソースのデータを、クエリ結果を構成するオブジェクトに変換する操作も行います。 最後に、プロバイダーは、データ ソースに更新が送信されるときに、オブジェクトをデータに変換します。  
  
 Visual Basic には、次の LINQ プロバイダーが含まれています。  
  
|プロバイダー|説明|  
|---|---|  
|LINQ to Objects|LINQ to Objects プロバイダーは、インメモリ コレクションとインメモリ配列のクエリを可能にします。 オブジェクトが <xref:System.Collections.IEnumerable> インターフェイスまたは <xref:System.Collections.Generic.IEnumerable%601> インターフェイスをサポートしている場合、LINQ to Objects プロバイダーを使用してクエリを実行できます。<br /><br /> LINQ to Objects プロバイダーは、すべての Visual Basic プロジェクトに既定でインポートされる <xref:System.Linq> 名前空間をインポートすることによって有効にすることができます。<br /><br /> LINQ to Objects プロバイダーの詳細については、[LINQ to Objects (Visual Basic)](../../concepts/linq/linq-to-objects.md)に関するページを参照してください。|  
|LINQ to SQL|LINQ to SQL プロバイダーは、SQL Server データベース内のデータのクエリと変更を可能にします。 これにより、アプリケーションのオブジェクト モデルを、データベース内のテーブルとオブジェクトに簡単に対応付けることができます。<br /><br /> Visual Basic を使用すると、オブジェクトリレーショナルデザイナー (O/R デザイナー) を含めることにより、LINQ to SQL を簡単に操作できるようになります。 このデザイナーを使用して、データベース内のオブジェクトに対応付けられるアプリケーション内のオブジェクト モデルを作成します。 O/R デザイナーには、ストアドプロシージャと関数を <xref:System.Data.Linq.DataContext> オブジェクトにマップする機能も用意されています。このオブジェクトは、データベースとの通信を管理し、オプティミスティック同時実行制御チェックの状態を格納します。<br /><br /> LINQ to SQL プロバイダーの詳細については、「 [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)」を参照してください。 オブジェクトリレーショナルデザイナーの詳細については、「 [Visual Studio の LINQ to SQL ツール](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2)」を参照してください。|  
|LINQ to XML|LINQ to XML プロバイダーは、XML のクエリと変更を可能にします。 インメモリ XML を変更することや、XML をファイルから読み込んだりファイルに保存したりすることができます。<br /><br /> さらに、LINQ to XML プロバイダーを使用すると、xml リテラルと XML 軸プロパティを使用して、Visual Basic コードに XML を直接書き込むことができます。 詳細については、「 [XML](../../../../visual-basic/programming-guide/language-features/xml/index.md)」を参照してください。|  
|LINQ to DataSet|LINQ to DataSet プロバイダーを使用すると、ADO.NET データセット内のデータに対してクエリや更新を行うことができます。 データセットを使用するアプリケーションに LINQ を追加することで、データセット内のデータのクエリ、集計、および更新などの機能を単純化すると同時に拡張できます。<br /><br /> 詳細については、「[LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)」を参照してください。|  
  
## <a name="structure-of-a-linq-query"></a>LINQ クエリの構造  
 LINQ クエリ (*クエリ式*と呼ばれることもあります) は、クエリのデータソースと反復変数を識別するクエリ句の組み合わせで構成されます。 クエリ式には、並べ替え、フィルター処理、グループ化、および結合を実行する命令や、ソース データに適用する演算も指定できます。 クエリ式の構文は SQL の構文に似ているので、ほとんどの構文は、改めて覚える必要はありません。  
  
 クエリ式は、`From` 句で始まります。 この句は、クエリのソース データと、ソース データの各要素を個別に参照するために使用される変数を識別します。 これらの変数は、名前付き*範囲変数*または*反復変数*です。 `From` 句は、`Aggregate` クエリ以外のクエリでは必須です。このクエリでは、`From` 句は省略できます。 `From` 句または `Aggregate` 句でクエリのスコープとソースを識別した後、クエリを絞り込むためのクエリ句を自由に組み合わせて記述できます。 クエリ句の詳細については、このトピックで後述する「LINQ クエリ演算子の Visual Basic」を参照してください。 たとえば、次のクエリでは、顧客データのソース コレクションを `customers` 変数として識別し、`cust` という名前の反復変数を識別します。  
  
 [!code-vb[VbVbalrIntroToLINQ#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#2)]  
  
 この例はそのままでも有効なクエリですが、結果を絞り込むクエリ句を追加すると、はるかに強力なクエリになります。 たとえば、`Where` 句を追加して、結果を 1 つ以上の値でフィルター処理できます。 クエリ式は、1 行のコードです。追加のクエリ句は、クエリの末尾に単純に追加できます。 アンダースコア (\_) の行連結文字を使用すると、複数行のテキストにまたがるクエリを分割して読みやすくすることができます。 次のコード例は、`Where` 句を含むクエリの例を示しています。  
  
 [!code-vb[VbVbalrIntroToLINQ#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#3)]  
  
 別の強力なクエリ句として、`Select` 句があります。この句を使用すると、データ ソースから選択したフィールドだけを返すことができます。 LINQ クエリは、厳密に型指定されたオブジェクトの列挙可能なコレクションを返します。 クエリは、匿名型または名前付きの型のコレクションを返すことができます。 `Select` 句を使用して、データ ソースから単一のフィールドを返すことができます。 これを行った場合、返されるコレクションの型は、その単一のフィールドの型になります。 `Select` 句を使用して、データ ソースから複数のフィールドを返すこともできます。 これを行った場合、返されるコレクションの型は、新しい匿名型になります。 クエリで返されたフィールドを、指定した名前付きの型のフィールドと一致させることもできます。 次のコード例は、データ ソースから選択されたフィールドのデータが設定されたメンバーのコレクションで、匿名型のコレクションを返すクエリ式を示します。  
  
 [!code-vb[VbVbalrIntroToLINQ#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#4)]  
  
 LINQ クエリを使用して、複数のデータ ソースを結合し、単一の結果を返すこともできます。 これは、1 つまたは複数の `From` 句を使用するか、`Join` クエリ句または `Group Join` クエリ句を使用して実行できます。 次のコード例は、顧客データと注文データを結合し、顧客データと注文データが格納された匿名型のコレクションを返すクエリ式を示します。  
  
 [!code-vb[VbVbalrIntroToLINQ#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#5)]  
  
 `Group Join` 句を使用して、顧客オブジェクトのコレクションを格納する階層的なクエリ結果を作成できます。 各顧客オブジェクトには、その顧客のすべての注文のコレクションを含むプロパティがあります。 次のコード例は、顧客データと注文データを階層的な結果として結合し、匿名型のコレクションを返すクエリ式を示します。 このクエリは、顧客の注文データのコレクションを格納する `CustomerOrders` プロパティを含む型を返します。 その顧客のすべての注文の合計を格納する `OrderTotal` プロパティも含まれます (このクエリは、LEFT OUTER JOIN と同等です)。  
  
 [!code-vb[VbVbalrIntroToLINQ#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/class2.vb#6)]  
  
 上記以外にも、強力なクエリ式を作成するために使用できる、さまざまな LINQ クエリ演算子があります。 このトピックの次のセクションで、クエリ式で使用できるさまざまなクエリ句について説明します。 Visual Basic のクエリ句の詳細については、「[クエリ](../../../../visual-basic/language-reference/queries/index.md)」を参照してください。  
  
## <a name="visual-basic-linq-query-operators"></a>LINQ クエリ演算子の Visual Basic  

LINQ クエリをサポートする <xref:System.Linq> 名前空間とその他の名前空間のクラスには、アプリケーションの要件に基づいてクエリの作成と絞り込みを行うために呼び出すことができるメソッドが含まれています。 Visual Basic には、次の一般的なクエリ句のキーワードが含まれています。 Visual Basic のクエリ句の詳細については、「[クエリ](../../../language-reference/queries/index.md)」を参照してください。

### <a name="from-clause"></a>From 句

クエリを開始するには、 [`From` 句](../../../../visual-basic/language-reference/queries/from-clause.md)または `Aggregate` 句のいずれかが必要です。 `From` 句は、クエリのソース コレクションと反復変数を指定します。 例:

 [!code-vb[VbVbalrIntroToLINQ#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#7)]

### <a name="select-clause"></a>Select 句

省略可。 [`Select` 句](../../../../visual-basic/language-reference/queries/select-clause.md)は、クエリの一連の反復変数を宣言します。 例:

 [!code-vb[VbVbalrIntroToLINQ#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#8)]

`Select` 句の指定がない場合、クエリの反復変数は、`From` 句または `Aggregate` 句で指定された反復変数で構成されます。

### <a name="where-clause"></a>Where 句

省略可。 [`Where` 句](../../../../visual-basic/language-reference/queries/where-clause.md)は、クエリのフィルター条件を指定します。 例:

 [!code-vb[VbVbalrIntroToLINQ#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#9)]

### <a name="order-by-clause"></a>Order By 句

省略可。 [`Order By` 句](../../../../visual-basic/language-reference/queries/order-by-clause.md)は、クエリ内の列の並べ替え順序を指定します。 例:

 [!code-vb[VbVbalrIntroToLINQ#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#10)]

### <a name="join-clause"></a>Join 句

省略可。 [`Join` 句](../../../../visual-basic/language-reference/queries/join-clause.md)は、2つのコレクションを結合して1つのコレクションにします。 例:

 [!code-vb[VbVbalrIntroToLINQ#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#11)]

### <a name="group-by-clause"></a>Group By 句

省略可。 [`Group By` 句](../../../../visual-basic/language-reference/queries/group-by-clause.md)は、クエリ結果の要素をグループ化します。 各グループに集計関数を適用するために使用できます。 例:

 [!code-vb[VbVbalrIntroToLINQ#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#12)]

### <a name="group-join-clause"></a>Group Join 句

省略可。 [`Group Join` 句](../../../../visual-basic/language-reference/queries/group-join-clause.md)は、2つのコレクションを1つの階層コレクションに結合します。 例:

 [!code-vb[VbVbalrIntroToLINQ#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#13)]

### <a name="aggregate-clause"></a>Aggregate 句

クエリを開始するには、 [`Aggregate` 句](../../../../visual-basic/language-reference/queries/aggregate-clause.md)または `From` 句のいずれかが必要です。 `Aggregate` 句は、1 つ以上の集計関数をコレクションに適用します。 たとえば、次の例のように、`Aggregate` 句を使用して、クエリによって返されるすべての要素の合計を計算できます。

 [!code-vb[VbVbalrIntroToLINQ#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#14)]

`Aggregate` 句を使用してクエリを変更することもできます。 たとえば、`Aggregate` 句を使用して、関連するクエリ コレクションに対して計算を実行できます。 例:

 [!code-vb[VbVbalrIntroToLINQ#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#15)]

### <a name="let-clause"></a>Let 句

省略可。 [`Let` 句](../../../../visual-basic/language-reference/queries/let-clause.md)は、値を計算してクエリの新しい変数に代入します。 例:

 [!code-vb[VbVbalrIntroToLINQ#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#16)]

### <a name="distinct-clause"></a>Distinct 句

省略可。 `Distinct` 句では、現在の反復変数の値を制限して、クエリ結果内の重複する値を除去します。 例:

 [!code-vb[VbVbalrIntroToLINQ#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#17)]

### <a name="skip-clause"></a>Skip 句

省略可。 [`Skip` 句](../../../../visual-basic/language-reference/queries/skip-clause.md)は、コレクション内の指定された数の要素をバイパスし、残りの要素を返します。 例:

 [!code-vb[VbVbalrIntroToLINQ#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#18)]

### <a name="skip-while-clause"></a>Skip While 句

省略可。 [`Skip While` 句](../../../../visual-basic/language-reference/queries/skip-while-clause.md)は、指定された条件が `true` 場合に限り、コレクション内の要素をバイパスし、残りの要素を返します。 例:

 [!code-vb[VbVbalrIntroToLINQ#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#19)]

### <a name="take-clause"></a>Take 句

省略可。 [`Take` 句](../../../../visual-basic/language-reference/queries/take-clause.md)は、コレクションの先頭から指定された数の連続する要素を返します。 例:

 [!code-vb[VbVbalrIntroToLINQ#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#20)]

### <a name="take-while-clause"></a>Take While 句

省略可。 指定された条件が `true` で残りの要素がバイパスされる限り、 [`Take While` 句](../../../../visual-basic/language-reference/queries/take-while-clause.md)にはコレクション内の要素が含まれます。 例:

 [!code-vb[VbVbalrIntroToLINQ#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#21)]
  
## <a name="use-additional-linq-query-features"></a>追加の LINQ クエリ機能を使用する  
  
LINQ によって提供される列挙可能でクエリ可能な型のメンバーを呼び出すことで、追加の LINQ クエリ機能を使用できます。 これらの追加機能は、クエリ式の結果に対して特定のクエリ演算子を呼び出すことで使用できます。 たとえば、次の例では、<xref:System.Linq.Enumerable.Union%2A?displayProperty=nameWithType> メソッドを使用して、2つのクエリの結果を1つのクエリ結果に結合しています。 また、<xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> メソッドを使用して、クエリ結果をジェネリック リストとして返します。
  
 [!code-vb[VbVbalrIntroToLINQ#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrIntroToLINQ/VB/Class1.vb#22)]  
  
 その他の LINQ 機能の詳細については、「[標準クエリ演算子の概要](../../concepts/linq/standard-query-operators-overview.md)」を参照してください。  
  
## <a name="connect-to-a-database-by-using-linq-to-sql"></a>LINQ to SQL を使用してデータベースに接続する  
 Visual Basic では、LINQ to SQL ファイルを使用してアクセスする SQL Server データベースオブジェクト (テーブル、ビュー、ストアドプロシージャなど) を指定します。 LINQ to SQL ファイルには、.dbml という拡張子が付きます。  
  
 SQL Server データベースへの有効な接続がある場合は、 **LINQ to SQL クラス**項目テンプレートをプロジェクトに追加できます。 これを行うと、オブジェクト リレーショナル デザイナー (O/R デザイナー) が表示されます。 O/R デザイナーを使用すると、コード内でアクセスする項目を**サーバーエクスプローラー**/**データベースエクスプローラー**からデザイナー画面にドラッグできます。 LINQ to SQL ファイルは、<xref:System.Data.Linq.DataContext> オブジェクトをプロジェクトに追加します。 このオブジェクトには、アクセスするテーブルとビューのプロパティおよびコレクションと、呼び出すストアド プロシージャのメソッドが格納されます。 変更を LINQ to SQL (.dbml) ファイルに保存した後、O/R デザイナーで定義された <xref:System.Data.Linq.DataContext> オブジェクトを参照することで、コード内でこれらのオブジェクトにアクセスできます。 プロジェクトの <xref:System.Data.Linq.DataContext> オブジェクトには、LINQ to SQL ファイルの名前に基づいて名前が付けられます。 たとえば、Northwind.dbml という名前の LINQ to SQL ファイルの場合は、<xref:System.Data.Linq.DataContext> という名前の `NorthwindDataContext` オブジェクトが作成されます。  
  
 詳細な手順の例については、「[方法: データベースを照会](how-to-query-a-database-by-using-linq.md)する」および「[方法: ストアドプロシージャを呼び出す](how-to-call-a-stored-procedure-by-using-linq.md)」を参照してください。  
  
## <a name="visual-basic-features-that-support-linq"></a>LINQ をサポートする Visual Basic 機能  
 Visual Basic には、LINQ の使用を単純化し、LINQ クエリを実行するために記述する必要があるコードの量を減らすための重要な機能が含まれています。 リポジトリには、次のものが含まれます。  
  
- **匿名型**。これにより、クエリ結果に基づいて新しい型を作成できます。  
  
- **暗黙的**に型指定された変数。型の指定を遅らせ、クエリ結果に基づいてコンパイラが型を推論できるようにします。  
  
- **拡張メソッド**。型自体を変更せずに、独自のメソッドで既存の型を拡張することができます。  
  
 詳細については、「 [LINQ をサポートする Visual Basic 機能](../../concepts/linq/features-that-support-linq.md)」を参照してください。  
  
## <a name="deferred-and-immediate-query-execution"></a>クエリの遅延実行と即時実行

 クエリの実行とクエリの作成は分離しています。 クエリを作成した後、その実行は、別のメカニズムによってトリガーされます。 クエリは、定義直後 (*即時実行*) に実行することも、定義を保存し、後でクエリを実行することもできます (*遅延実行*)。  
  
 既定では、クエリを作成しても、クエリ自体が直ちに実行されることはありません。 代わりに、クエリ結果を参照するために使用される変数にクエリ定義が格納されます。 そのクエリ結果変数が、後でコード内の `For…Next` ループなどでアクセスされると、クエリが実行されます。 このプロセスを*遅延実行*と呼びます。  
  
 クエリは、定義されている場合にも実行できます。これは*即時実行*と呼ばれます。 即時実行は、クエリ結果の個々の要素にアクセスする必要があるメソッドを適用することで開始できます。 `Count`、`Sum`、`Average`、`Min`、`Max` などの集計関数を含めることで、この結果を得ることができます。 集計関数の詳細については、「 [Aggregate 句](../../../language-reference/queries/aggregate-clause.md)」を参照してください。  
  
 `ToList` メソッドまたは `ToArray` メソッドを使用することでも、即時実行を開始できます。 これは、クエリを直ちに実行し、結果をキャッシュする場合に役に立ちます。 これらのメソッドの詳細については、「[データ型の変換](../../concepts/linq/converting-data-types.md)」を参照してください。  
  
 クエリ実行の詳細については、「初めての[LINQ クエリの作成](../../concepts/linq/writing-your-first-linq-query.md)」を参照してください。  
  
## <a name="xml-in-visual-basic"></a>Visual Basic における XML  
 Visual Basic の XML 機能には、xml リテラルと XML 軸プロパティが含まれています。これにより、コード内の XML の作成、アクセス、クエリ、および変更を簡単に行うことができます。 XML リテラルを使用すると、XML をコード内に直接記述できます。 Visual Basic コンパイラは、XML を、最初のクラスのデータ オブジェクトとして処理します。  
  
 次のコード例は、XML 要素を作成し、そのサブ要素と属性にアクセスし、LINQ を使用してその要素の内容を照会する方法を示しています。  
  
 [!code-vb[VbXmlSamples#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples3.vb#8)]  
  
 詳細については、「 [XML](../xml/index.md)」を参照してください。  
  
## <a name="related-resources"></a>関連資料  
  
|トピック|説明|  
|---|---|  
|[XML](../../language-features/xml/index.md)|Visual Basic の XML 機能について説明します。この機能を使用すると、クエリを実行したり、Visual Basic コードに XML をファーストクラスのデータオブジェクトとして含めることができます。|  
|[クエリ](../../../language-reference/queries/index.md)|Visual Basic で使用できるクエリ句に関するリファレンス情報を提供します。|  
|[統合言語クエリ (LINQ)](../../concepts/linq/index.md)|LINQ の概要、プログラミング ガイド、およびサンプルを示します。|  
|[LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)|LINQ to SQL の概要、プログラミング ガイド、およびサンプルを示します。|  
|[LINQ to Objects](../../concepts/linq/linq-to-objects.md)|LINQ to Objects の概要、プログラミング ガイド、およびサンプルを示します。|  
|[LINQ to ADO.NET (ポータル ページ)](../../concepts/linq/linq-to-adonet-portal-page.md)|LINQ to ADO.NET の一般的な情報、プログラミングガイド、およびサンプルへのリンクが含まれています。|  
|[LINQ to XML](../../concepts/linq/linq-to-xml.md)|LINQ to XML の概要、プログラミング ガイド、およびサンプルを示します。|  
  
## <a name="how-to-and-walkthrough-topics"></a>方法とチュートリアルのトピック
 [方法: データベースの照会](how-to-query-a-database-by-using-linq.md)  
  
 [方法 : ストアド プロシージャの呼び出し](how-to-call-a-stored-procedure-by-using-linq.md)  
  
 [方法: データベースのデータの変更](how-to-modify-data-in-a-database-by-using-linq.md)  
  
 [方法 : 結合を使用したデータの結合](how-to-combine-data-with-linq-by-using-joins.md)  
  
 [方法: クエリ結果を並べ替える](how-to-sort-query-results-by-using-linq.md)  
  
 [方法 : クエリ結果のフィルター処理](how-to-filter-query-results-by-using-linq.md)  
  
 [方法 : データの数、合計、または平均の算出](how-to-count-sum-or-average-data-by-using-linq.md)  
  
 [方法 : クエリ結果内の最小値と最大値の検索](how-to-find-the-minimum-or-maximum-value-in-a-query-result.md)  
  
 [方法: 更新、挿入、および削除を実行するストアド プロシージャを割り当てる (O/R デザイナー)](/visualstudio/data-tools/how-to-assign-stored-procedures-to-perform-updates-inserts-and-deletes-o-r-designer)  
  
## <a name="featured-book-chapters"></a>特集書籍の章  
 [Chapter 17:](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff652502(v=orm.10))プログラミングにおける LINQ [Visual Basic 2008](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff652504(v=orm.10))  
  
## <a name="see-also"></a>関連項目

- [統合言語クエリ (LINQ)](../../concepts/linq/index.md)
- [Visual Basic における LINQ to XML の概要](../../language-features/xml/overview-of-linq-to-xml.md)
- [LINQ to DataSet の概要](../../../../framework/data/adonet/linq-to-dataset-overview.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [Visual Studio の LINQ to SQL ツール](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2)
- [DataContext メソッド (O/R デザイナー)](/visualstudio/data-tools/datacontext-methods-o-r-designer)
