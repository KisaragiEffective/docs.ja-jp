---
title: LINQ によるデータ変換 (C#)
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ [C#], data transformations
- source elements [LINQ in C#]
- joining multiple inputs [LINQ in C#]
- multiple outputs for one output sequence [LINQ in C#]
- subset of source elements [LINQ in C#]
- data sources [LINQ in C#], data transformations
- data transformations [LINQ in C#]
ms.assetid: 674eae9e-bc72-4a88-aed3-802b45b25811
ms.openlocfilehash: 393e3bd24c4bc8b89064e01e1048b24254f5f83b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75635952"
---
# <a name="data-transformations-with-linq-c"></a>LINQ によるデータ変換 (C#)
統合言語クエリ (LINQ) で行うことができるのは、データの取得だけではありません。 データ変換のための強力なツールとしても使用できます。 LINQ クエリを使用することにより、ソース シーケンスを入力として使用し、さまざまな方法で加工して新しい出力シーケンスを作成できます。 要素自体を変更せずに、並べ替えやグループ化してシーケンス自体を変更できます。 しかし、LINQ クエリの最も強力な機能は、新しい型を作成する機能です。 この操作は [select](../../../language-reference/keywords/select-clause.md) 句内で行います。 たとえば、次のタスクを実行できます。  
  
- 複数の入力シーケンスを結合して、新しい型の単一の出力シーケンスを作成する。  
  
- ソース シーケンス内の各要素の単一のプロパティまたは複数のプロパティを構成要素とする出力シーケンスを作成する。  
  
- ソース データに対して実行した操作の結果を構成要素とする出力シーケンスを作成する。  
  
- 別の形式で出力シーケンスを作成する。 たとえば、SQL の行またはテキスト ファイルのデータを XML に変換できます。  
  
 これらはほんの一例です。 これらの変換を、同じクエリ内でさまざまな方法で組み合わせて使用することもできます。 また、あるクエリの出力シーケンスを別のクエリの入力シーケンスとして使用することもできます。  
  
## <a name="joining-multiple-inputs-into-one-output-sequence"></a>複数の入力を 1 つの出力シーケンスに結合する  
 LINQ クエリを使用して、複数の入力シーケンスの要素を含む 1 つの出力シーケンスを作成できます。 次の例は、2 つのインメモリ データ構造を結合する方法を示していますが、ソースが XML、SQL、または DataSet のデータを結合する場合にも同じ基本原則を適用できます。 次の 2 つのクラス型があるとします。  
  
 [!code-csharp[CsLINQGettingStarted#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#7)]  
  
 クエリの例を次に示します。  
  
 [!code-csharp[CSLinqGettingStarted#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#8)]  
  
 詳細については、「[join 句](../../../language-reference/keywords/join-clause.md)」および「[select 句](../../../language-reference/keywords/select-clause.md)」を参照してください。  
  
## <a name="selecting-a-subset-of-each-source-element"></a>各ソース要素のサブセットを選択する  
 ソース シーケンスの各要素のサブセットを選択するには、主に次の 2 つの方法があります。  
  
1. ソース要素の 1 つのメンバーのみを選択するには、ドット演算を使用します。 次の例で、`Customer` オブジェクトに `City` という文字列を含むいくつかのパブリック プロパティが含まれているとします。 このクエリを実行すると、文字列の出力シーケンスが生成されます。  
  
    ```csharp
    var query = from cust in Customers  
                select cust.City;  
    ```  
  
2. ソース要素からの複数のプロパティを含む要素を作成するには、名前付きオブジェクトまたは匿名型を指定したオブジェクト初期化子を使用します。 次の例は、匿名型を使用して各 `Customer` 要素の 2 つのプロパティをカプセル化する方法を示しています。  
  
    ```csharp
    var query = from cust in Customer  
                select new {Name = cust.Name, City = cust.City};  
    ```  
  
 詳細については、「[オブジェクト初期化子とコレクション初期化子](../../classes-and-structs/object-and-collection-initializers.md)」および「[匿名型](../../classes-and-structs/anonymous-types.md)」を参照してください。  
  
## <a name="transforming-in-memory-objects-into-xml"></a>インメモリ オブジェクトを XML に変換する  
 LINQ クエリを使用すると、インメモリ データ構造、SQL データベース、ADO.NET データセット、XML ストリーム、または XML ドキュメントの間でデータ変換を簡単に行うことができます。 インメモリ データ構造のオブジェクトを XML 要素に変換する例を次に示します。  
  
 [!code-csharp[CsLINQGettingStarted#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#9)]  
  
 このコードを実行すると、次の XML 出力が生成されます。  
  
```xml  
<Root>  
  <student>  
    <First>Svetlana</First>  
    <Last>Omelchenko</Last>  
    <Scores>97,92,81,60</Scores>  
  </student>  
  <student>  
    <First>Claire</First>  
    <Last>O'Donnell</Last>  
    <Scores>75,84,91,39</Scores>  
  </student>  
  <student>  
    <First>Sven</First>  
    <Last>Mortensen</Last>  
    <Scores>88,94,65,91</Scores>  
  </student>  
</Root>  
```  
  
 詳細については、「[C# での XML ツリーの作成 (LINQ to XML)](./creating-xml-trees-linq-to-xml-2.md)」を参照してください。  
  
## <a name="performing-operations-on-source-elements"></a>ソース要素に対して操作を実行する  
 出力シーケンスには、ソース シーケンスの要素または要素のプロパティが 1 つも含まれない場合があります。 代わりに、ソース要素を入力引数として使用することで計算された値のシーケンスを出力として生成できます。 次の簡単なクエリを実行すると、`double` 型の要素のソース シーケンスに基づく計算を表す値を含む文字列のシーケンスが出力されます。  
  
> [!NOTE]
> クエリが他のドメインに変換される場合、クエリ式でのメソッド呼び出しはサポートされません。 たとえば、[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] で C# の通常のメソッドを呼び出すことはできません。これは、C# のメソッドのコンテキストが SQL Server にないためです。 ただし、ストアド プロシージャをメソッドにマップして呼び出すことは可能です。 詳細については、「[ストアド プロシージャ](../../../../framework/data/adonet/sql/linq/stored-procedures.md)」を参照してください。  
  
 [!code-csharp[CsLINQGettingStarted#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#10)]  
  
## <a name="see-also"></a>参照

- [統合言語クエリ (LINQ) (C#)](./index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)
- [LINQ to XML (C#)](./linq-to-xml-overview.md)
- [LINQ クエリ式](../../../linq/index.md)
- [select 句](../../../language-reference/keywords/select-clause.md)
