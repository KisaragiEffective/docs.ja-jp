---
title: クエリの基本操作
ms.date: 07/20/2015
helpviewer_keywords:
- data sources [LINQ in Visual Basic]
- Join clause [LINQ in Visual Basic]
- ordering data [LINQ in Visual Basic]
- projections [LINQ in Visual Basic]
- LINQ [Visual Basic], query operations
- Order By clause [LINQ in Visual Basic]
- joining data [LINQ in Visual Basic]
- queries [LINQ in Visual Basic], basic operations
- selecting data [LINQ in Visual Basic]
- Group By clause [LINQ in Visual Basic]
- grouping data [LINQ in Visual Basic]
- Select clause [LINQ in Visual Basic]
ms.assetid: 1146f6d0-fcb8-4f4d-8223-c9db52620d21
ms.openlocfilehash: b9216dba23f49e4d9fd99687e38f5c13addde8fb
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636875"
---
# <a name="basic-query-operations-visual-basic"></a>基本的なクエリ操作 (Visual Basic)
このトピックでは、Visual Basic での統合言語クエリ (LINQ) 式について簡単に紹介し、クエリで実行する一般的な操作の一部について説明します。 詳細については、以下のトピックを参照してください。  
  
 [Visual Basic における LINQ の概要](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)  
  
 [クエリ](../../../../visual-basic/language-reference/queries/index.md)  
  
 [チュートリアル: Visual Basic でのクエリの作成](../../../../visual-basic/programming-guide/concepts/linq/walkthrough-writing-queries.md)  
  
## <a name="specifying-the-data-source-from"></a>データソースの指定 (から)  
 LINQ クエリでは、最初の手順として、クエリを実行するデータソースを指定します。 したがって、クエリ内の `From` 句は常に最初になります。 クエリ演算子ソースの種類に基づいて、結果を選択して整形します。  
  
 [!code-vb[VbLINQBasicOps#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#1)]  
  
 `From` 句では、データソース、`customers`、および*範囲変数*`cust`を指定します。 範囲変数はループ反復変数に似ていますが、クエリ式では実際のイテレーションは発生しません。 クエリが実行されると、多くの場合、`For Each` ループを使用して、範囲変数は `customers`内の連続する各要素への参照として機能します。 `cust` の型はコンパイラで推論できるため、明示的に指定する必要はありません。 明示的な型指定なしで記述されたクエリの例については、「[クエリ操作での型の関係 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/type-relationships-in-query-operations.md)」を参照してください。  
  
 Visual Basic で `From` 句を使用する方法の詳細については、「 [From 句](../../../../visual-basic/language-reference/queries/from-clause.md)」を参照してください。  
  
## <a name="filtering-data-where"></a>データのフィルター処理 (Where)  
 最も一般的なクエリ操作では、ブール式の形式でフィルターが適用されている可能性があります。 次に、式が true になっている要素のみが返されます。 フィルター処理を実行するには、`Where` 句を使用します。 フィルターでは、結果のシーケンスに含めるデータソース内の要素を指定します。 次の例では、ロンドンに住所を持つ顧客のみが含まれています。  
  
 [!code-vb[VbLINQBasicOps#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#2)]  
  
 `And` や `Or` などの論理演算子を使用して、`Where` 句でフィルター式を結合できます。 たとえば、名前が Devon であるロンドンからの顧客のみを返すには、次のコードを使用します。  
  
```vb  
Where cust.City = "London" And cust.Name = "Devon"   
```  
  
 ロンドンまたはパリの顧客を返すには、次のコードを使用します。  
  
```vb  
Where cust.City = "London" Or cust.City = "Paris"   
```  
  
 Visual Basic で `Where` 句を使用する方法の詳細については、「 [Where 句](../../../../visual-basic/language-reference/queries/where-clause.md)」を参照してください。  
  
## <a name="ordering-data-order-by"></a>データの並べ替え (Order By)  
 多くの場合、返されたデータを特定の順序に並べ替えると便利です。 `Order By` 句により、返されたシーケンス内の要素が、指定されたフィールドで並べ替えられます。 たとえば、次のクエリでは、`Name` プロパティに基づいて結果を並べ替えます。 `Name` が文字列であるため、返されるデータはアルファベット順 (A から Z) に並べ替えられます。  
  
 [!code-vb[VbLINQBasicOps#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#3)]  
  
 結果を逆の順序 (Z から A) で並び替えるには、`Order By...Descending` 句を使用します。 `Ascending` も `Descending` も指定されていない場合、既定値は `Ascending` です。  
  
 Visual Basic で `Order By` 句を使用する方法の詳細については、「 [Order By 句](../../../../visual-basic/language-reference/queries/order-by-clause.md)」を参照してください。  
  
## <a name="selecting-data-select"></a>データの選択 (Select)  
 `Select` 句は、返される要素のフォームとコンテンツを指定します。 たとえば、結果を、完全な `Customer` オブジェクト、1つの `Customer` プロパティ、プロパティのサブセット、さまざまなデータソースからのプロパティの組み合わせ、または計算に基づいた新しい結果の型のいずれで構成するかを指定できます。 `Select` 句でソース要素のコピー以外のものを生成する場合、その操作は*投影*と呼ばれます。  
  
 完全な `Customer` オブジェクトで構成されるコレクションを取得するには、範囲変数自体を選択します。  
  
 [!code-vb[VbLINQBasicOps#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#4)]  
  
 `Customer` インスタンスが多数のフィールドを持つ大きなオブジェクトで、取得するすべてのが名前である場合は、次の例に示すように、[`cust.Name`] を選択できます。 ローカル型の推論では、結果の型が `Customer` オブジェクトのコレクションから文字列のコレクションに変更されることが認識されます。  
  
 [!code-vb[VbLINQBasicOps#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#5)]  
  
 データソースから複数のフィールドを選択するには、次の2つの選択肢があります。  
  
- `Select` 句で、結果に含めるフィールドを指定します。 コンパイラは、これらのフィールドをプロパティとして持つ匿名型を定義します。 詳細については、「[匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)」を参照してください。  
  
     次の例で返される要素は匿名型のインスタンスなので、コード内の他の場所で型を名前で参照することはできません。 コンパイラによって指定された型の名前に、通常の Visual Basic コードでは無効な文字が含まれています。 次の例では、`londonCusts4` のクエリによって返されるコレクション内の要素は匿名型のインスタンスです。  
  
     [!code-vb[VbLINQBasicOps#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#6)]  
  
     -または-  
  
- 結果に含める特定のフィールドを含む名前付きの型を定義し、`Select` 句でその型のインスタンスを作成および初期化します。 このオプションは、返されるコレクションの外部で個々の結果を使用する必要がある場合、またはメソッドの呼び出しでパラメーターとして渡す必要がある場合にのみ使用します。 次の例の `londonCusts5` の種類は、IEnumerable (Of NamePhone) です。  
  
     [!code-vb[VbLINQBasicOps#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#7)]  
  
     [!code-vb[VbLINQBasicOps#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#8)]  
  
 Visual Basic で `Select` 句を使用する方法の詳細については、「 [Select 句](../../../../visual-basic/language-reference/queries/select-clause.md)」を参照してください。  
  
## <a name="joining-data-join-and-group-join"></a>データの結合 (結合とグループ結合)  
 複数のデータソースを `From` 句で組み合わせるには、いくつかの方法があります。 たとえば、次のコードでは、2つのデータソースを使用し、両方のプロパティを結果に暗黙的に結合しています。 このクエリでは、姓が母音で始まる学生を選択します。  
  
 [!code-vb[VbLINQBasicOps#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#9)]  
  
> [!NOTE]
> [「方法: 項目のリストを作成](../../../../visual-basic/programming-guide/concepts/linq/how-to-create-a-list-of-items.md)する」で作成した学生の一覧を使用して、このコードを実行できます。  
  
 `Join` キーワードは、SQL の `INNER JOIN` と同じです。 2つのコレクションの要素間でのキー値の一致に基づいて、2つのコレクションを結合します。 このクエリでは、キー値が一致するコレクション要素のすべてまたは一部が返されます。 たとえば、次のコードでは、前の暗黙的な結合のアクションが複製されます。  
  
 [!code-vb[VbLINQBasicOps#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#10)]  
  
 `Group Join` は、SQL の `LEFT JOIN` と同様に、コレクションを1つの階層コレクションに結合します。 詳細については、「 [Join 句](../../../../visual-basic/language-reference/queries/join-clause.md)と[Group join 句](../../../../visual-basic/language-reference/queries/group-join-clause.md)」を参照してください。  
  
## <a name="grouping-data-group-by"></a>データのグループ化 (Group By)  
 要素の1つまたは複数のフィールドに従って、クエリ結果の要素をグループ化するには、`Group By` 句を追加します。 たとえば、次のコードでは、学生をクラス年別にグループ化しています。  
  
 [!code-vb[VbLINQBasicOps#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#11)]  
  
 「[方法: 項目のリストを作成](../../../../visual-basic/programming-guide/concepts/linq/how-to-create-a-list-of-items.md)する」で作成した学生の一覧を使用してこのコードを実行すると、`For Each` ステートメントからの出力は次のようになります。  
  
 年: 初級  
  
 Tucker、Michael  
  
 お金  
  
 Garcia、Debra  
  
 Tucker、Lance  
  
 年: シニア  
  
 Omelchenko、Svetlana  
  
 Osada、Michiko  
  
 Fakhouri、Fadi  
  
 Feng  
  
 Adams、マネージャー  
  
 Year: Freshman  
  
 Sven  
  
 Cesar、  
  
 次のコードに示すバリエーションでは、クラス年を順に並べ替え、各年の各学生を姓で並べ替えます。  
  
 [!code-vb[VbLINQBasicOps#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQBasicOps/VB/Class1.vb#12)]  
  
 `Group By`の詳細については、「 [Group By 句](../../../../visual-basic/language-reference/queries/group-by-clause.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Collections.Generic.IEnumerable%601>
- [Visual Basic の LINQ の概要](../../../../visual-basic/programming-guide/concepts/linq/getting-started-with-linq.md)
- [クエリ](../../../../visual-basic/language-reference/queries/index.md)
- [標準クエリ演算子の概要 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/standard-query-operators-overview.md)
- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
