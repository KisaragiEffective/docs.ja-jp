---
title: LINQ to DataSet でのクエリ
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c1a78fa8-9f0c-40bc-a372-5575a48708fe
ms.openlocfilehash: 092dbb5227e5f9e0ae2a62656a300d2367bcf16b
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634795"
---
# <a name="queries-in-linq-to-dataset"></a>LINQ to DataSet でのクエリ
クエリとは、データ ソースからデータを取得するための式です。 一般に、クエリは専用のクエリ言語で表現されます。たとえば、リレーショナル データベースであれば SQL、XML であれば XQuery が使用されます。 そのため、開発者はクエリの対象となるデータ ソースやデータ形式ごとに新しいクエリ言語を習得する必要があります。 統合言語クエリ (LINQ) は、データ ソースや形式の違いを意識することなくデータを扱うことのできる、より簡素化された一貫したモデルを提供します。 LINQ クエリでは、常にプログラミング オブジェクトを操作することになります。  
  
 LINQ のクエリ操作は、データ ソースを取得し、クエリを作成して、クエリを実行するという 3 つのアクションから成ります。  
  
 <xref:System.Collections.Generic.IEnumerable%601> ジェネリックインターフェイスを実装するデータソースは、LINQ を使用してクエリを実行できます。 <xref:System.Data.DataTable> で <xref:System.Data.DataTableExtensions.AsEnumerable%2A> を呼び出すと、LINQ to DataSet クエリのデータソースとして機能する汎用 <xref:System.Collections.Generic.IEnumerable%601> インターフェイスを実装するオブジェクトが返されます。  
  
 クエリでは、データ ソースから取得する情報を正確に指定できます。 また、並べ替え、グループ化、整形方法を指定して情報を取得することもできます。 LINQ では、クエリが変数に格納されます。 一連の値を返すようにクエリを設計する場合、クエリ変数そのものが列挙可能な型であることが必要です。 このクエリ変数は、クエリの情報を保存するだけで、なんらかのアクションを実行したり、データを返したりすることはありません。 クエリを作成した後、データを取得するには、そのクエリを実行する必要があります。  
  
 一連の値を返すクエリでは、クエリ変数そのものはクエリ結果を保持しません。クエリ変数には、クエリのコマンドが格納されるだけです。 クエリ変数が `foreach` ループまたは `For Each` ループで反復処理されるまで、クエリは実行されません。 これを*遅延実行*と呼びます。つまり、クエリの作成後、しばらくの間クエリの実行が行われます。 これは、任意のタイミングでクエリを実行できるということを意味します。 これは、たとえば他のアプリケーションによって更新されるデータベースがある場合に便利です。 アプリケーションで、最新情報を取得するクエリを作成し、それを繰り返し実行することにより、更新のたびに最新の情報を取得できます。  
  
 遅延実行によって一連の値を返すクエリとは対照的に、シングルトン値を返すクエリは直ちに実行されます。 シングルトン クエリの例としては、<xref:System.Linq.Enumerable.Count%2A>、<xref:System.Linq.Enumerable.Max%2A>、<xref:System.Linq.Enumerable.Average%2A>、<xref:System.Linq.Enumerable.First%2A> があります。 これらのシングルトン クエリは、結果を計算するためにはクエリ結果が必要であるため、直ちに実行されます。 たとえば、クエリ結果の平均を求めるためには、クエリを実行して、平均関数に入力データを与える必要があります。 シングルトン値を生成しないクエリでも、<xref:System.Linq.Enumerable.ToList%2A> メソッドまたは <xref:System.Linq.Enumerable.ToArray%2A> メソッドを使用することによって、即時実行を強制できます。 即時実行を強制するこの手法は、クエリの結果をキャッシュする場合などに使用すると効果的です。
  
## <a name="queries"></a>クエリ  
 LINQ to DataSet クエリは、クエリ式の構文とメソッドベースのクエリ構文という2つの異なる構文で作成できます。  
  
### <a name="query-expression-syntax"></a>のクエリ式の構文  
 クエリ式は宣言型のクエリ構文です。 開発者は SQL に似た構文形式を C# または Visual Basic で用いてクエリを作成できます。 クエリ式の構文を使用することにより、フィルター、並べ替え、グループ化など、データ ソースに対するきわめて複雑な処理を最小限のコードで実行できます。 詳細については、「 [LINQ クエリ式](../../../csharp/linq/index.md#query-expression-overview)」および「[基本的なクエリ操作 (Visual Basic)](../../../visual-basic/programming-guide/concepts/linq/basic-query-operations.md)」を参照してください。
  
 .NET Framework 共通言語ランタイム (CLR) は、クエリ式の構文自体を読み取ることができません。 そのため、クエリ式はコンパイル時に、CLR が理解できる形式 (メソッド呼び出し) へと変換されます。 これらのメソッドは、*標準クエリ演算子*と呼ばれます。 開発者は、クエリ構文を使う代わりに、メソッド構文を使ってそれらを直接呼び出すこともできます。 詳細については、「[LINQ でのクエリ構文とメソッド構文](../../../csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq.md)」を参照してください。 標準クエリ演算子の詳細については、「[標準クエリ演算子の概要](../../../csharp/programming-guide/concepts/linq/standard-query-operators-overview.md)」を参照してください。  
  
 次の例では、<xref:System.Linq.Enumerable.Select%2A> を使用して `Product` テーブルからすべての行を取得し、製品名を表示しています。  
  
 [!code-csharp[DP LINQ to DataSet Examples#SelectSimple1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#selectsimple1)]
 [!code-vb[DP LINQ to DataSet Examples#SelectSimple1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#selectsimple1)]  
  
### <a name="method-based-query-syntax"></a>メソッド ベースのクエリ構文  
 LINQ to DataSet クエリを作成するもう1つの方法は、メソッドベースのクエリを使用することです。 メソッドベースのクエリ構文は、LINQ 演算子メソッドに対するダイレクトメソッド呼び出しのシーケンスであり、ラムダ式をパラメーターとして渡します。 詳しくは、「[ラムダ式](../../../csharp/programming-guide/statements-expressions-operators/lambda-expressions.md)」をご覧ください。  
  
 次の例では、<xref:System.Linq.Enumerable.Select%2A> を使用して `Product` テーブルからすべての行を取得し、製品名を表示しています。  
  
 [!code-csharp[DP LINQ to DataSet Examples#SelectAnonymousTypes_MQ](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#selectanonymoustypes_mq)]
 [!code-vb[DP LINQ to DataSet Examples#SelectAnonymousTypes_MQ](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#selectanonymoustypes_mq)]  
  
## <a name="composing-queries"></a>クエリの作成  
 前述したように、一連の値を返すように設計されたクエリでは、クエリ変数自体にはクエリ コマンドのみが格納されます。 クエリに即時実行を促すメソッドが含まれていなければ、`foreach` ループまたは `For Each` ループでクエリ変数を反復処理するまで、実際にはクエリは実行されません。 遅延実行により、複数のクエリを組み合わせたり、クエリを拡張したりすることが可能となります。 クエリを拡張して新しい操作を追加すると、その変更が最終的な実行時に反映されます。 次の例の最初のクエリでは、すべての製品が返されます。 2 つ目のクエリでは、サイズが "L" のすべての製品を返すように、`Where` を使って 1 つ目のクエリを拡張しています。  
  
 [!code-csharp[DP LINQ to DataSet Examples#Composing](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#composing)]
 [!code-vb[DP LINQ to DataSet Examples#Composing](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#composing)]  
  
 クエリが実行された後は、追加のクエリを作成することはできません。後続のすべてのクエリでは、メモリ内 LINQ 演算子が使用されます。 クエリの実行は、`foreach` または `For Each` ステートメントでクエリ変数を反復処理するとき、または即時実行を引き起こす LINQ 変換演算子の1つを呼び出すことによって行われます。 即時実行を促す演算子としては、<xref:System.Linq.Enumerable.ToList%2A>、<xref:System.Linq.Enumerable.ToArray%2A>、<xref:System.Linq.Enumerable.ToLookup%2A>、<xref:System.Linq.Enumerable.ToDictionary%2A> などがあります。  
  
 次の例の最初のクエリは、すべての製品を表示価格で並べ替えて返します。 <xref:System.Linq.Enumerable.ToArray%2A> メソッドを使用して、クエリの即時実行を強制しています。  
  
 [!code-csharp[DP LINQ to DataSet Examples#ToArray2](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#toarray2)]
 [!code-vb[DP LINQ to DataSet Examples#ToArray2](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#toarray2)]  
  
## <a name="see-also"></a>関連項目

- [プログラミング ガイド](programming-guide-linq-to-dataset.md)
- [DataSet のクエリ](querying-datasets-linq-to-dataset.md)
- [C# の LINQ の概要](../../../csharp/programming-guide/concepts/linq/index.md)
- [Visual Basic の LINQ の概要](../../../visual-basic/programming-guide/concepts/linq/getting-started-with-linq.md)
