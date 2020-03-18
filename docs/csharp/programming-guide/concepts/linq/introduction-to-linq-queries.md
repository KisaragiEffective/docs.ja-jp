---
title: LINQ クエリの概要 (C#)
ms.date: 07/20/2015
helpviewer_keywords:
- deferred execution [LINQ]
- LINQ, queries
- LINQ, deferred execution
- queries [LINQ], about LINQ queries
ms.assetid: 37895c02-268c-41d5-be39-f7d936fa88a8
ms.openlocfilehash: 74a6f2e1e9296551f4faf73a905b49d3e2e3687e
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75635718"
---
# <a name="introduction-to-linq-queries-c"></a>LINQ クエリの概要 (C#)
"*クエリ*" は、データ ソースからデータを取得する式です。 クエリは通常、専用のクエリ言語で表されます。 これまでに、リレーショナル データベース用の SQL や XML 用の XQuery など、データ ソースの種類に合わせてさまざまな言語が開発されてきました。 このため、開発者は、サポートする必要のあるデータ ソースの種類やデータ形式ごとに、新しいクエリ言語を習得する必要がありました。 LINQ は、さまざまな種類のデータ ソースやデータ形式のデータを操作するための一貫したモデルを提供することにより、この負担を軽減します。 LINQ クエリでは、操作の対象は常にオブジェクトになります。 共通の基本的なコーディング パターンを使用することで、LINQ プロバイダーを利用できる XML ドキュメント、SQL データベース、ADO.NET データセット、.NET コレクション、その他の任意の形式のデータを照会したり変換したりできます。  
  
## <a name="three-parts-of-a-query-operation"></a>クエリ操作の 3 つの手順  
 LINQ クエリ操作はすべて、次の 3 つの手順で構成されます。  
  
1. データ ソースを取得します。  
  
2. クエリを作成します。  
  
3. クエリを実行します。  
  
 クエリ操作の 3 つの手順がソース コードでどのように表されるかを次の例に示します。 この例では、わかりやすくするために整数の配列をデータ ソースとして使用していますが、他のデータ ソースを使用する場合にも同じ概念が当てはまります。 このコードは、このトピックの残りの部分全体を通して参照されます。  
  
 [!code-csharp[CsLINQGettingStarted#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#1)]  
  
 次の図は、クエリ操作全体を表しています。 LINQ では、クエリの実行はクエリ自体とは別個のものです。 つまり、クエリ変数を作成するだけでは、データは取得されません。  
  
 ![完全な LINQ クエリ操作の図。](./media/introduction-to-linq-queries/linq-query-complete-operation.png)  
  
## <a name="the-data-source"></a>データ ソース  
 前の例では、データ ソースが配列であるため、暗黙的にジェネリック <xref:System.Collections.Generic.IEnumerable%601> インターフェイスがサポートされます。 つまり、LINQ でクエリを実行できるということです。 クエリは `foreach` ステートメントで実行されますが、`foreach` には <xref:System.Collections.IEnumerable> または <xref:System.Collections.Generic.IEnumerable%601> が必要です。 <xref:System.Collections.Generic.IEnumerable%601> をサポートする型や、ジェネリック <xref:System.Linq.IQueryable%601> などの派生インターフェイスは、*クエリ可能型*と呼ばれます。  
  
 クエリ可能型は、変更や特別な処理を行わなくても、LINQ データ ソースとして使用できます。 ソース データがメモリ内にクエリ可能型として存在していない場合、LINQ プロバイダーは、そのような型としてソース データを表す必要があります。 たとえば、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、クエリ可能な <xref:System.Xml.Linq.XElement> 型に XML ドキュメントが読み込まれます。  
  
 [!code-csharp[CsLINQGettingStarted#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#2)]  
  
 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] では、まず、デザイン時に手動で、または Visual Studio で [LINQ to SQL ツール](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2)を使用して、オブジェクト リレーショナル マッピングを作成します。 オブジェクトに対するクエリを記述すると、実行時には、[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] によってデータベースとの通信が処理されます。 次の例では、`Customers` がデータベース内の特定のテーブルを表し、クエリ結果の型 <xref:System.Linq.IQueryable%601> が <xref:System.Collections.Generic.IEnumerable%601> から派生しています。  
  
```csharp  
Northwnd db = new Northwnd(@"c:\northwnd.mdf");  
  
// Query for customers in London.  
IQueryable<Customer> custQuery =  
    from cust in db.Customers  
    where cust.City == "London"  
    select cust;  
```  
  
 それぞれの種類のデータ ソースを作成する方法の詳細については、対応する LINQ プロバイダーのドキュメントを参照してください。 ただし、基本的な規則は非常に単純です。LINQ データ ソースは、ジェネリック <xref:System.Collections.Generic.IEnumerable%601> インターフェイス、またはこれを継承するインターフェイスをサポートする任意のオブジェクトです。  
  
> [!NOTE]
> 非ジェネリック <xref:System.Collections.IEnumerable> インターフェイスをサポートする <xref:System.Collections.ArrayList> などの型も、LINQ データ ソースとして使用できます。 詳細については、「[LINQ を使用して ArrayList にクエリを実行する方法 (C#)](./how-to-query-an-arraylist-with-linq.md)」を参照してください。  
  
## <a name="query"></a> クエリ  
 クエリでは、データ ソースからどのような情報を取得するかを指定します。 オプションとして、情報が返される前に、その情報を並べ替え、グループ化し、構造化する方法を指定することもできます。 クエリはクエリ変数に格納され、クエリ式で初期化されます。 クエリを簡単に記述できるようにするために、C# に新しいクエリ構文が導入されています。  
  
 前の例のクエリでは、整数の配列からすべての偶数が返されます。 クエリ式には、`from`、`where`、および `select` の 3 つの句が含まれています (SQL に詳しい方は、句の順番が SQL での順番とは逆になっていることに気付かれると思います)。`from` 句はデータ ソースを指定し、`where` 句はフィルターを適用し、`select` 句は返される要素の種類を指定します。 これらのクエリ句およびその他のクエリ句の詳細については、「[LINQ クエリ式](../../../linq/index.md)」セクションで説明しています。 今の段階で重要な点は、LINQ では、クエリ変数自体は何も処理を行わず、データを返さないという点です。 この時点では、後でクエリが実行されるときに結果の生成に必要となる情報が格納されるだけです。 背後でどのようにクエリが構築されるかについては、「[標準クエリ演算子の概要 (C#)](./standard-query-operators-overview.md)」をご覧ください。  
  
> [!NOTE]
> クエリは、メソッド構文を使用して表すこともできます。 詳細については、「[LINQ でのクエリ構文とメソッド構文](./query-syntax-and-method-syntax-in-linq.md)」を参照してください。  
  
## <a name="query-execution"></a>クエリの実行  
  
### <a name="deferred-execution"></a>遅延実行  
 先に説明したように、クエリ変数自体が行うのはクエリ コマンドの格納のみです。 実際のクエリの実行は、`foreach` ステートメントでクエリ変数が反復処理されるまで延期されます。 この概念を "*遅延実行*" と呼びます。遅延実行の例を次に示します。  
  
 [!code-csharp[csLinqGettingStarted#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#4)]  
  
 `foreach` ステートメントでは、クエリ結果の取得も行われます。 たとえば、前のクエリでは、返されるシーケンスの各値が反復変数 `num` に (一度に 1 つずつ) 格納されます。  
  
 クエリ変数自体にはクエリ結果は格納されないので、必要に応じて何度でもクエリを実行できます。 たとえば、別のアプリケーションによって頻繁に更新されるデータベースがあるとします。 自分のアプリケーションでは、最新のデータを取得するクエリを 1 つ作成し、それを一定の間隔で繰り返し実行することで、毎回異なる結果を取得できます。  
  
### <a name="forcing-immediate-execution"></a>即時実行の強制  
 一連のソース要素に対して集計関数を実行するクエリでは、最初にそれらの要素を反復処理する必要があります。 このようなクエリには、`Count`、`Max`、`Average`、`First` などがあります。 これらのクエリでは、明示的に `foreach` ステートメントを使用しなくても同等の処理が実行されます。これは、結果を返すためにクエリ自体が `foreach` を使用する必要があるからです。 これらの種類のクエリでは、`IEnumerable` コレクションではなく、単一の値が返されることにも注意してください。 次のクエリは、ソース配列に含まれている偶数の数を返します。  
  
 [!code-csharp[csLinqGettingStarted#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#5)]  
  
 クエリの即時実行を強制し、その結果をキャッシュするには、<xref:System.Linq.Enumerable.ToList%2A> メソッドまたは <xref:System.Linq.Enumerable.ToArray%2A> メソッドを呼び出します。  
  
 [!code-csharp[csLinqGettingStarted#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#6)]  
  
 クエリ式の直後に `foreach` ループを配置することでも実行を強制できます。 ただし、`ToList` または `ToArray` を呼び出した場合は、単一のコレクション オブジェクトにすべてのデータをキャッシュする処理も行われます。  
  
## <a name="see-also"></a>関連項目

- [C# の LINQ の概要](/dotnet/csharp/programming-guide/concepts/linq/)
- [チュートリアル: C# でのクエリの作成](./walkthrough-writing-queries-linq.md)
- [LINQ クエリ式](../../../linq/index.md)
- [foreach、in](../../../language-reference/keywords/foreach-in.md)
- [クエリ キーワード (LINQ)](../../../language-reference/keywords/query-keywords.md)
