---
title: 初めての LINQ クエリの作成
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], writing
- LINQ queries [Visual Basic]
- LINQ [Visual Basic], writing queries
ms.assetid: 4affb732-3e9b-4479-aa31-1f9bd8183cbe
ms.openlocfilehash: addf35afa2a4c88faf73ebc3d60fbcf9c4db1518
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636706"
---
# <a name="writing-your-first-linq-query-visual-basic"></a>初めての LINQ クエリの作成 (Visual Basic)
"*クエリ*" は、データ ソースからデータを取得する式です。 クエリは専用のクエリ言語で表現されます。 時間の経過と共に、さまざまな種類のデータソースに対してさまざまな言語が開発されています。たとえば、リレーショナルデータベース用の SQL や XML 用の XQuery などです。 これにより、アプリケーション開発者は、サポートされているデータソースの種類やデータ形式ごとに新しいクエリ言語を習得する必要があります。  
  
 統合言語クエリ (LINQ) を使用すると、さまざまな種類のデータソースと形式でデータを操作するための一貫したモデルを提供することで、状況を簡単にすることができます。 LINQ クエリでは、常にオブジェクトを操作します。 同じ基本的なコーディングパターンを使用して、XML ドキュメント、SQL データベース、ADO.NET データセットとエンティティ、.NET Framework コレクション、LINQ プロバイダーが使用可能なその他のソースまたは形式のデータのクエリと変換を行います。 このドキュメントでは、基本的な LINQ クエリの作成と使用に関する3つのフェーズについて説明します。  
  
## <a name="three-stages-of-a-query-operation"></a>クエリ操作の3つの段階  
 LINQ クエリ操作は、次の3つのアクションで構成されます。  
  
1. データソースを取得します。  
  
2. クエリを作成します。  
  
3. クエリを実行します。  
  
 LINQ では、クエリの実行はクエリの作成とは異なります。 クエリを作成するだけでは、データを取得しません。 この点については、後で詳しく説明します。  
  
 次の例では、クエリ操作の3つの部分について説明します。 この例では、デモンストレーション用の便利なデータソースとして整数の配列を使用します。 ただし、同じ概念が他のデータソースにも適用されます。  
  
> [!NOTE]
> [[コンパイル] ページのプロジェクトデザイナー (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)で、[**推定]** が **[オン**] に設定されていることを確認します。  
  
 [!code-vb[VbLINQFirstQuery#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#1)]  
  
 Output:  
  
 `0 2 4 6`  
  
## <a name="the-data-source"></a>データ ソース  
 前の例のデータソースは配列であるため、ジェネリック <xref:System.Collections.Generic.IEnumerable%601> インターフェイスを暗黙的にサポートしています。 これは、LINQ クエリのデータソースとして配列を使用できるようにするためのものです。 <xref:System.Collections.Generic.IEnumerable%601> をサポートする型や、ジェネリック <xref:System.Linq.IQueryable%601> などの派生インターフェイスは、*クエリ可能型*と呼ばれます。  
  
 暗黙的にクエリ可能な型として、配列は LINQ データソースとして機能するための変更や特別な処理を必要としません。 これは、<xref:System.Collections.Generic.IEnumerable%601>をサポートする任意のコレクション型 (ジェネリック <xref:System.Collections.Generic.List%601>、<xref:System.Collections.Generic.Dictionary%602>、および .NET Framework クラスライブラリ内のその他のクラスを含む) にも当てはまります。  
  
 ソースデータに <xref:System.Collections.Generic.IEnumerable%601>がまだ実装されていない場合、そのデータソースの*標準クエリ演算子*の機能を実装するには LINQ プロバイダーが必要です。 たとえば、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] は、次の例に示すように、クエリ可能な <xref:System.Xml.Linq.XElement> 型に XML ドキュメントを読み込む作業を処理します。 標準クエリ演算子の詳細については、「[標準クエリ演算子の概要 (Visual Basic)](standard-query-operators-overview.md)」を参照してください。  
  
 [!code-vb[VbLINQFirstQuery#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#2)]  
  
 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)]では、最初に、手動で、または visual studio の[Visual studio の LINQ to SQL ツール](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2)を使用して、デザイン時にオブジェクトリレーショナルマッピングを作成します。 オブジェクトに対するクエリを記述すると、実行時には、[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] によってデータベースとの通信が処理されます。 次の例では、`customers` はデータベース内の特定のテーブルを表し、<xref:System.Data.Linq.Table%601> は汎用 <xref:System.Linq.IQueryable%601>をサポートしています。  
  
```vb  
' Create a data source from a SQL table.  
Dim db As New DataContext("C:\Northwind\Northwnd.mdf")  
Dim customers As Table(Of Customer) = db.GetTable(Of Customer)  
```  
  
 特定の種類のデータソースを作成する方法の詳細については、さまざまな LINQ プロバイダーのドキュメントを参照してください。 (これらのプロバイダーの一覧については、「 [LINQ (統合言語クエリ)](../../../../visual-basic/programming-guide/concepts/linq/index.md)」を参照してください)。基本的な規則は単純です。 LINQ データソースは、ジェネリック <xref:System.Collections.Generic.IEnumerable%601> インターフェイスをサポートする任意のオブジェクト、またはそれを継承するインターフェイスです。  
  
> [!NOTE]
> 非ジェネリック <xref:System.Collections.IEnumerable> インターフェイスをサポートする <xref:System.Collections.ArrayList> などの型は、LINQ データソースとしても使用できます。 <xref:System.Collections.ArrayList>を使用する例については、「[方法: LINQ を使用して ArrayList をクエリする (Visual Basic)](how-to-query-an-arraylist-with-linq.md)」を参照してください。  
  
## <a name="the-query"></a>クエリ  
 このクエリでは、データソースから取得する情報を指定します。 また、返される前に、その情報を並べ替え、グループ化、または構造化する方法を指定することもできます。 クエリの作成を有効にするために、Visual Basic は新しいクエリ構文を言語に組み込みました。  
  
 次の例のクエリを実行すると、整数配列のすべての偶数が返されます (`numbers`)。  
  
 [!code-vb[VbLINQFirstQuery#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#1)]  
  
 クエリ式には、`From`、`Where`、および `Select`の3つの句が含まれています。 各クエリ式句の特定の関数と目的については、 [「基本的なクエリ操作 (Visual Basic)](basic-query-operations.md)」で説明されています。 詳細については、「[クエリ](../../../../visual-basic/language-reference/queries/index.md)」を参照してください。 LINQ では、多くの場合、クエリ定義は変数に格納され、後で実行されることに注意してください。 前の例の `evensQuery` などのクエリ変数は、クエリ可能な型である必要があります。 `evensQuery` の型が `IEnumerable(Of Integer)`、ローカル型推論を使用してコンパイラによって割り当てられています。  
  
 クエリ変数自体は何も処理を行わず、データを返さないことに注意する必要があります。 クエリ定義だけが格納されます。 前の例では、クエリを実行する `For Each` ループです。  
  
## <a name="query-execution"></a>クエリの実行  
 クエリの実行は、クエリの作成とは別のものです。 クエリの作成ではクエリを定義しますが、実行は別のメカニズムによってトリガーされます。 クエリは、定義直後 (*即時実行*) に実行することも、定義を保存し、後でクエリを実行することもできます (*遅延実行*)。  
  
### <a name="deferred-execution"></a>遅延実行  
 一般的な LINQ クエリは、`evensQuery` が定義されている前の例のものに似ています。 クエリは作成されますが、すぐには実行されません。 代わりに、クエリ定義は `evensQuery`クエリ変数に格納されます。 後でクエリを実行します。通常は、値のシーケンスを返す `For Each` ループを使用するか、`Count` や `Max`などの標準クエリ演算子を適用します。 このプロセスを*遅延実行*と呼びます。  
  
 [!code-vb[VbLINQFirstQuery#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#7)]  
  
 値のシーケンスの場合は、`For Each` ループの繰り返し変数 (前の例では`number`) を使用して、取得したデータにアクセスします。 クエリ変数 `evensQuery`では、クエリの結果ではなくクエリの定義が保持されるので、クエリ変数を複数回使用して、クエリを必要なだけ実行することができます。 たとえば、アプリケーション内に、別のアプリケーションによって継続的に更新されるデータベースがあるとします。 そのデータベースからデータを取得するクエリを作成したら、`For Each` ループを使用してクエリを繰り返し実行し、毎回最新のデータを取得することができます。  
  
 次の例は、遅延実行のしくみを示しています。 前の例のように、`evensQuery2` を定義して実行 `For Each` すると、データソース `numbers` 内の一部の要素が変更されます。 次に、2番目の `For Each` ループが `evensQuery2` 実行されます。 `For Each` ループでは `numbers`の新しい値を使用してクエリが再実行されるため、結果は2回目に異なります。  
  
 [!code-vb[VbLINQFirstQuery#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#3)]  
  
 Output:  
  
 `Evens in original array:`  
  
 `0  2  4  6`  
  
 `Evens in changed array:`  
  
 `0  10  2  22  8`  
  
### <a name="immediate-execution"></a>即時実行  
 クエリの遅延実行では、クエリ定義は、後で実行するためにクエリ変数に格納されます。 即時実行では、クエリは定義時に実行されます。 クエリ結果の個々の要素へのアクセスを必要とするメソッドを適用すると、実行がトリガーされます。 即時実行は、通常、単一の値を返す標準クエリ演算子の1つを使用することによって強制されます。 例として、`Count`、`Max`、`Average`、および `First`があります。 これらの標準クエリ演算子は、シングルトンの結果を計算して返すために、適用されるとすぐにクエリを実行します。 単一の値を返す標準クエリ演算子の詳細については、「[集計操作](aggregation-operations.md)、[要素の操作](element-operations.md)、および[量指定子の操作](quantifier-operations.md)」を参照してください。  
  
 次のクエリでは、整数の配列に含まれる偶数の数が返されます。 クエリ定義は保存されず、`numEvens` は単純な `Integer`です。  
  
 [!code-vb[VbLINQFirstQuery#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#4)]  
  
 `Aggregate` メソッドを使用すると、同じ結果を得ることができます。  
  
 [!code-vb[VbLINQFirstQuery#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#5)]  
  
 また、次のコードに示すように、クエリ (イミディエイト) またはクエリ変数 (遅延) で `ToList` または `ToArray` メソッドを呼び出すことによって、クエリの実行を強制することもできます。  
  
 [!code-vb[VbLINQFirstQuery#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQFirstQuery/VB/Class1.vb#6)]  
  
 前の例では、`evensQuery3` はクエリ変数ですが、`evensList` はリストであり、`evensArray` は配列です。  
  
 `ToList` または `ToArray` を使用して即時実行を強制するのは、クエリをすぐに実行し、結果を単一のコレクションオブジェクトにキャッシュする場合に特に便利です。 これらのメソッドの詳細については、「[データ型の変換](converting-data-types.md)」を参照してください。  
  
 また、<xref:Microsoft.VisualBasic.Collection.System%23Collections%23IEnumerable%23GetEnumerator%2A> メソッドなどの `IEnumerable` メソッドを使用してクエリを実行することもできます。  
  
## <a name="see-also"></a>関連項目

- [Visual Basic の LINQ の概要](getting-started-with-linq.md)
- [ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
- [標準クエリ演算子の概要 (Visual Basic)](standard-query-operators-overview.md)
- [Visual Basic における LINQ の概要](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
- [クエリ](../../../../visual-basic/language-reference/queries/index.md)
