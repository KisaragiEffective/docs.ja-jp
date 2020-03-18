---
title: クエリ操作での型の関係
ms.date: 07/20/2015
helpviewer_keywords:
- variable relationships [LINQ in Visual Basic]
- type information inferred [LINQ in Visual Basic]
- type relationships [LINQ in Visual Basic]
- queries [LINQ in Visual Basic], type relationships
- data sources [LINQ in Visual Basic], type relationships
- LINQ [Visual Basic], type relationships
- inferring type information [LINQ in Visual Basic]
- relationships [LINQ in Visual Basic]
ms.assetid: b5ff4da5-f3fd-4a8e-aaac-1cbf52fa16f6
ms.openlocfilehash: e839271ac254a5e96f8c99f59397016fb99540aa
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636914"
---
# <a name="type-relationships-in-query-operations-visual-basic"></a>クエリ操作での型の関係 (Visual Basic)

統合言語クエリ (LINQ) のクエリ操作で使用される変数は厳密に型指定されており、相互に互換性がある必要があります。 厳密な型指定は、データソース、クエリ自体、およびクエリの実行で使用されます。 次の図は、LINQ クエリの記述に使用される用語を示しています。 クエリの各部分の詳細については、「[クエリの基本操作 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/basic-query-operations.md)」を参照してください。

![要素が強調表示されている擬似コードクエリを示すスクリーンショット。](./media/type-relationships-in-query-operations/linq-query-description-terms.png)

クエリ内の範囲変数の型は、データソース内の要素の型と互換性がある必要があります。 クエリ変数の型は、`Select` 句で定義されている sequence 要素と互換性がある必要があります。 最後に、シーケンス要素の型も、クエリを実行する `For Each` ステートメントで使用されるループコントロール変数の型と互換性がある必要があります。 この厳密な型指定により、コンパイル時に型エラーの識別が容易になります。

Visual Basic は、*暗黙的*な型指定とも呼ばれるローカル型推論を実装することによって、厳密な型指定を容易にします。 この機能は、前の例で使用されており、LINQ のサンプルとドキュメント全体で使用されています。 Visual Basic では、ローカル型の推論は、`As` 句を指定せずに `Dim` ステートメントを使用するだけで行われます。 次の例では、`city` が文字列として厳密に型指定されています。

[!code-vb[VbLINQTypeRels#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQTypeRels/VB/Class1.vb#1)]

> [!NOTE]
> ローカル型の推論は、`Option Infer` が `On`に設定されている場合にのみ機能します。 詳細については、「 [Option 推論ステートメント](../../../../visual-basic/language-reference/statements/option-infer-statement.md)」を参照してください。

ただし、クエリでローカル型の推論を使用する場合でも、データソース内の変数、クエリ変数、およびクエリ実行ループの間に同じ型のリレーションシップが存在します。 LINQ クエリを記述するとき、またはドキュメントのサンプルとコード例を使用するときに、これらの型の関係を基本的に理解しておくと便利です。

データソースから返される型と一致しない範囲変数には、明示的な型を指定する必要がある場合があります。 `As` 句を使用して、範囲変数の型を指定できます。 ただし、変換が[縮小変換](../../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)であり、`Option Strict` が `On`に設定されている場合、エラーが発生します。 したがって、データソースから取得した値に対して変換を実行することをお勧めします。 <xref:System.Linq.Enumerable.Cast%2A> メソッドを使用して、データソースの値を明示的な範囲変数型に変換できます。 また、`Select` 句で選択した値を、範囲変数の型とは異なる明示的な型にキャストすることもできます。 これらの点を次のコードに示します。

[!code-vb[VbLINQTypeRels#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQTypeRels/VB/Class1.vb#4)]

## <a name="queries-that-return-entire-elements-of-the-source-data"></a>ソースデータの要素全体を返すクエリ

次の例は、ソースデータから選択された一連の要素を返す LINQ クエリ操作を示しています。 Source、`names`には文字列の配列が含まれており、クエリ出力は、文字 M で始まる文字列を含むシーケンスです。

[!code-vb[VbLINQTypeRels#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQTypeRels/VB/Class1.vb#2)]

これは次のコードと同じですが、はるかに短く、簡単に記述できます。 クエリでのローカル型の推定への依存は、Visual Basic で推奨されるスタイルです。

[!code-vb[VbLINQTypeRels#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQTypeRels/VB/Class1.vb#3)]

前のコード例では、型が暗黙的または明示的に決定されているかどうかに関係があります。

1. データソース内の要素の型である `names`は、クエリ内の範囲変数の型 `name`です。

2. 選択されているオブジェクトの種類 (`name`) によって、クエリ変数の型 `mNames`が決まります。 ここで `name` は文字列なので、クエリ変数は Visual Basic の IEnumerable (Of String) です。

3. `mNames` で定義されたクエリは、`For Each` ループで実行されます。 ループは、クエリの実行結果を反復処理します。 `mNames`実行すると、は文字列のシーケンスを返し、ループ反復変数、`nm`も文字列になります。

## <a name="queries-that-return-one-field-from-selected-elements"></a>選択した要素から1つのフィールドを返すクエリ

次の例は、データソースから選択された各要素の1つの部分のみを含むシーケンスを返す [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] クエリ操作を示しています。 このクエリは、データソースとして `Customer` オブジェクトのコレクションを取得し、結果の `Name` プロパティのみを射影します。 顧客名は文字列であるため、クエリは文字列のシーケンスを出力として生成します。

```vb
' Method GetTable returns a table of Customer objects.
Dim customers = db.GetTable(Of Customer)()
Dim custNames = From cust In customers
                Where cust.City = "London"
                Select cust.Name

For Each custName In custNames
    Console.WriteLine(custName)
Next
```

変数間のリレーションシップは、単純な例の場合と似ています。

1. データソース内の要素の型である `customers`は、クエリ内の範囲変数の型 `cust`です。 この例では、この型は `Customer`です。

2. `Select` ステートメントは、オブジェクト全体ではなく、各 `Customer` オブジェクトの `Name` プロパティを返します。 `Name` が文字列であるため、クエリ変数の `custNames`は、`Customer`ではなく、IEnumerable (Of String) になります。

3. `custNames` は文字列のシーケンスを表すため、`For Each` ループの反復変数 `custName`は文字列である必要があります。

次の例に示すように、ローカル型の推定を使用しない場合、前の例では記述と理解が煩雑になります。

```vb
' Method GetTable returns a table of Customer objects.
 Dim customers As Table(Of Customer) = db.GetTable(Of Customer)()
 Dim custNames As IEnumerable(Of String) =
     From cust As Customer In customers
     Where cust.City = "London"
     Select cust.Name

 For Each custName As String In custNames
     Console.WriteLine(custName)
 Next
```

## <a name="queries-that-require-anonymous-types"></a>匿名型を必要とするクエリ

次の例は、より複雑な状況を示しています。 前の例では、すべての変数の型を明示的に指定するのは不便でした。 この例では、これは不可能です。 このクエリの `Select` 句は、データソースの `Customer` 要素全体、または各要素の1つのフィールドを選択するのではなく、元の `Customer` オブジェクトの2つのプロパティ `Name` と `City`を返します。 `Select` 句に対する応答として、コンパイラは、これら2つのプロパティを含む匿名型を定義します。 `For Each` ループで `nameCityQuery` を実行した結果は、新しい匿名型のインスタンスのコレクションになります。 匿名型には使用可能な名前がないため、`nameCityQuery` の型や `custInfo` 明示的に指定することはできません。 つまり、匿名型の場合、`IEnumerable(Of String)`の `String` の代わりに使用する型名はありません。 詳細については、「[匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)」を参照してください。

```vb
' Method GetTable returns a table of Customer objects.
Dim customers = db.GetTable(Of Customer)()
Dim nameCityQuery = From cust In customers
                    Where cust.City = "London"
                    Select cust.Name, cust.City

For Each custInfo In nameCityQuery
    Console.WriteLine(custInfo.Name)
Next
```

前の例のすべての変数の型を指定することはできませんが、リレーションシップは同じままです。

1. データソース内の要素の型は、クエリ内の範囲変数の型になります。 この例では、`cust` は `Customer`のインスタンスです。

2. `Select` ステートメントによって匿名型が生成されるため、クエリ変数の `nameCityQuery`は、匿名型として暗黙的に型指定する必要があります。 匿名型には使用可能な名前がないため、明示的に指定することはできません。

3. `For Each` ループ内の反復変数の型は、手順 2. で作成した匿名型です。 型には使用可能な名前がないため、ループ反復変数の型を暗黙的に決定する必要があります。

## <a name="see-also"></a>関連項目

- [Visual Basic の LINQ の概要](../../../../visual-basic/programming-guide/concepts/linq/getting-started-with-linq.md)
- [匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)
- [ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
- [Visual Basic における LINQ の概要](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
- [クエリ](../../../../visual-basic/language-reference/queries/index.md)
