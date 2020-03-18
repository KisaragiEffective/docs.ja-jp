---
title: 標準クエリ演算子の概要
ms.date: 07/20/2015
ms.assetid: 302bd39e-2ec1-495b-94bf-37d370d6f05f
ms.openlocfilehash: 9660e1d92db87e1ae906b3fd6616a51c8b8715fa
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349309"
---
# <a name="standard-query-operators-overview-visual-basic"></a>標準クエリ演算子の概要 (Visual Basic)

"*標準クエリ演算子*" は、LINQ パターンを形成するメソッドです。 これらのメソッドの大部分はシーケンスに対して機能します。ここでシーケンスとは、<xref:System.Collections.Generic.IEnumerable%601> インターフェイスまたは <xref:System.Linq.IQueryable%601> インターフェイスを実装している型のオブジェクトのことです。 標準クエリ演算子には、フィルター処理、プロジェクション、集計、並べ替えなどのクエリ機能が用意されています。

LINQ 標準クエリ演算子には 2 つのセットがあります。1 つは <xref:System.Collections.Generic.IEnumerable%601> 型のオブジェクトを操作する演算子、もう 1 つは <xref:System.Linq.IQueryable%601> 型のオブジェクトを操作する演算子です。 各セットを構成するメソッドは、それぞれ、<xref:System.Linq.Enumerable> および <xref:System.Linq.Queryable> クラスの静的メンバーです。 そのメソッドの操作対象である型の "*拡張メソッド*" として定義されています。 つまり、静的メソッド構文またはインスタンス メソッド構文のいずれかを使用して呼び出すことができます。

さらに、いくつかの標準クエリ演算子メソッドが、<xref:System.Collections.Generic.IEnumerable%601> または <xref:System.Linq.IQueryable%601> を基にする型以外の型を操作します。 <xref:System.Linq.Enumerable> 型は、このような 2 つのメソッドを定義し、その両方が <xref:System.Collections.IEnumerable> 型のオブジェクトを操作します。 これらのメソッド <xref:System.Linq.Enumerable.Cast%60%601%28System.Collections.IEnumerable%29> と <xref:System.Linq.Enumerable.OfType%60%601%28System.Collections.IEnumerable%29> を使用して、LINQ パターンでクエリされるパラメーター化されていないまたは非ジェネリック型のコレクションを有効にすることができます。 これを行うには、厳密に型指定されたオブジェクトのコレクションを作成します。 <xref:System.Linq.Queryable> クラスは、型 <xref:System.Linq.Queryable.Cast%60%601%28System.Linq.IQueryable%29> のオブジェクトを操作する 2 つの類似したメソッド <xref:System.Linq.Queryable.OfType%60%601%28System.Linq.IQueryable%29> と <xref:System.Linq.Queryable> を定義します。

標準クエリ演算子の実行のタイミングは、シングルトン値を返すか、値のシーケンスを返すかで異なります。 これらのシングルトン値を返すメソッド (たとえば、<xref:System.Linq.Enumerable.Average%2A> と <xref:System.Linq.Enumerable.Sum%2A>) は、すぐに実行されます。 シーケンスを返すメソッドは、クエリの実行を遅延させ、列挙可能なオブジェクトを返します。

メモリ内コレクションを操作するメソッド、つまり <xref:System.Collections.Generic.IEnumerable%601> を拡張するメソッドの場合、返される列挙可能なオブジェクトは、メソッドに渡された引数をキャプチャします。 オブジェクトが列挙されると、クエリ演算子のロジックが使用され、クエリ結果が返されます。

一方、<xref:System.Linq.IQueryable%601> を拡張するメソッドはクエリ動作を実装しませんが、実行されるクエリを表す式ツリーを作成します。 クエリの処理は、ソース <xref:System.Linq.IQueryable%601> オブジェクトによって処理されます。

クエリ メソッドの呼び出しは 1 回のクエリにまとめてチェーン化できるため、クエリが複雑になることがあります。

次のコード例は、標準クエリ演算子を使用してシーケンスに関する情報を取得する方法を示しています。

```vb
Dim sentence = "the quick brown fox jumps over the lazy dog"
' Split the string into individual words to create a collection.
Dim words = sentence.Split(" "c)

Dim query = From word In words
            Group word.ToUpper() By word.Length Into gr = Group
            Order By Length _
            Select Length, GroupedWords = gr

Dim output As New System.Text.StringBuilder
For Each obj In query
    output.AppendLine(String.Format("Words of length {0}:", obj.Length))
    For Each word As String In obj.GroupedWords
        output.AppendLine(word)
    Next
Next

'Display the output
MsgBox(output.ToString())

' This code example produces the following output:
'
' Words of length 3:
' THE
' FOX
' THE
' DOG
' Words of length 4:
' OVER
' LAZY
' Words of length 5:
' QUICK
' BROWN
' JUMPS
```

## <a name="query-expression-syntax"></a>クエリ式の構文

頻繁に使用される標準クエリ演算子の中には、C# および Visual Basic 言語専用のキーワード構文が使用されているものがあります。こうした構文では、標準クエリ演算子を、"*クエリ* *式*" の一部として呼び出すことができます。 専用キーワードとそれに対応する構文を持つ標準クエリ演算子の詳細については、「[標準クエリ演算子のクエリ式の構文 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/query-expression-syntax-for-standard-query-operators.md)」を参照してください。

## <a name="extending-the-standard-query-operators"></a>標準クエリ演算子の拡張

標準クエリ演算子のセットを拡張するには、対象のドメインまたはテクノロジに適したドメイン固有のメソッドを作成します。 また、標準クエリ演算子を、リモート評価、クエリ変換、最適化などの追加サービスが用意されている独自の実装で置き換えることもできます。 例については、「<xref:System.Linq.Enumerable.AsEnumerable%2A>」を参照してください。

## <a name="related-sections"></a>関連セクション

次のリンクをクリックすると、さまざまな標準クエリ演算子に関する追加情報を機能別に確認することができます。

- [データの並べ替え](../../../../visual-basic/programming-guide/concepts/linq/sorting-data.md)

- [操作の設定 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/set-operations.md)

- [データのフィルター処理 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/filtering-data.md)

- [量指定子操作 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/quantifier-operations.md)

- [プロジェクション操作 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/projection-operations.md)

- [データのパーティション分割 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/partitioning-data.md)

- [結合操作 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/join-operations.md)

- [データのグループ化 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/grouping-data.md)

- [生成操作 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/generation-operations.md)

- [等値演算 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/equality-operations.md)

- [要素の操作 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/element-operations.md)

- [データ型の変換 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/converting-data-types.md)

- [連結演算 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/concatenation-operations.md)

- [集計操作 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/aggregation-operations.md)

## <a name="see-also"></a>参照

- <xref:System.Linq.Enumerable>
- <xref:System.Linq.Queryable>
- [LINQ の概要 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/introduction-to-linq.md)
- [標準クエリ演算子のクエリ式の構文 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/query-expression-syntax-for-standard-query-operators.md)
- [実行方法による標準クエリ演算子の分類 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/classification-of-standard-query-operators-by-manner-of-execution.md)
- [拡張メソッド](../../../../visual-basic/programming-guide/language-features/procedures/extension-methods.md)
