---
title: '方法: LINQ クエリのカスタム メソッドを追加する'
ms.date: 07/20/2015
ms.assetid: 099b2e2a-83cd-45c6-aa4d-01b398b5faaf
ms.openlocfilehash: 3004a9c9c7abeffd9993b848ad765e7ae2dc8876
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353374"
---
# <a name="how-to-add-custom-methods-for-linq-queries-visual-basic"></a>方法: LINQ クエリのカスタムメソッドを追加する (Visual Basic)

<xref:System.Collections.Generic.IEnumerable%601> インターフェイスに拡張メソッドを追加することで、LINQ クエリに使用できるメソッド セットを拡張できます。 たとえば一連の値から単一の値を求めるために、平均値や最大値を求める標準的な演算に加えて、独自の集計メソッドを作成することができます。 また、一連の値を受け取って別の一連の値を返す特定のデータ変換やカスタム フィルターの働きを持ったメソッドを作成することもできます。 このようなメソッドには、<xref:System.Linq.Enumerable.Distinct%2A>、<xref:System.Linq.Enumerable.Skip%2A>、<xref:System.Linq.Enumerable.Reverse%2A> があります。

<xref:System.Collections.Generic.IEnumerable%601> インターフェイスを拡張すると、列挙可能なコレクションにカスタム メソッドを適用できます。 詳細については、「[拡張メソッド](../../../../visual-basic/programming-guide/language-features/procedures/extension-methods.md)」を参照してください。

## <a name="adding-an-aggregate-method"></a>集計メソッドの追加

集計メソッドは、一連の値から単一の値を計算するメソッドです。 LINQ は、<xref:System.Linq.Enumerable.Average%2A>、<xref:System.Linq.Enumerable.Min%2A>、<xref:System.Linq.Enumerable.Max%2A> などの集計メソッドを提供します。 <xref:System.Collections.Generic.IEnumerable%601> インターフェイスに拡張メソッドを追加することで、独自の集計メソッドを作成できます。

次のコード例は、`Median` 型の一連の数値から中央値を求める `double` という拡張メソッドの作成方法を示しています。

```vb
Imports System.Runtime.CompilerServices

Module LINQExtension

    ' Extension method for the IEnumerable(of T) interface.
    ' The method accepts only values of the Double type.
    <Extension()>
    Function Median(ByVal source As IEnumerable(Of Double)) As Double
        If source.Count = 0 Then
            Throw New InvalidOperationException("Cannot compute median for an empty set.")
        End If

        Dim sortedSource = From number In source
                           Order By number

        Dim itemIndex = sortedSource.Count \ 2

        If sortedSource.Count Mod 2 = 0 Then
            ' Even number of items in list.
            Return (sortedSource(itemIndex) + sortedSource(itemIndex - 1)) / 2
        Else
            ' Odd number of items in list.
            Return sortedSource(itemIndex)
        End If
    End Function
End Module
```

この拡張メソッドは、<xref:System.Collections.Generic.IEnumerable%601> インターフェイスにある他の集計メソッドを呼び出すときと同じように、列挙可能な任意のコレクションに対して呼び出すことができます。

> [!NOTE]
> Visual Basic では、メソッド呼び出しを使用することも、`Aggregate` または `Group By` 句に対して標準クエリ構文を使用することもできます。 詳細については、「 [Aggregate 句](../../../../visual-basic/language-reference/queries/aggregate-clause.md)と[Group by 句](../../../../visual-basic/language-reference/queries/group-by-clause.md)」を参照してください。

`Median` 型の配列に対して `double` メソッドを使用する方法を次のコード例に示します。

```vb
Dim numbers1() As Double = {1.9, 2, 8, 4, 5.7, 6, 7.2, 0}

Dim query1 = Aggregate num In numbers1 Into Median()

Console.WriteLine("Double: Median = " & query1)
```

```vb
' This code produces the following output:
'
' Double: Median = 4.85
```

### <a name="overloading-an-aggregate-method-to-accept-various-types"></a>さまざまな型を受け取るために集計メソッドをオーバーロードする

集計メソッドでさまざまな型を受け取るように、集計メソッドをオーバーロードすることができます。 その標準的な方法として、型ごとにオーバーロードを作成します。 または、ジェネリック型を受け取り、デリゲートを使って特定の型に変換するオーバーロードを作成する方法もあります。 その両方の方法を組み合わせることもできます。

#### <a name="to-create-an-overload-for-each-type"></a>型ごとにオーバーロードを作成するには

サポート予定の型ごとに固有のオーバーロードを作成できます。 次のコード例では、`Median` 型の `integer` メソッドのオーバーロードを示します。

```vb
' Integer overload

<Extension()>
Function Median(ByVal source As IEnumerable(Of Integer)) As Double
    Return Aggregate num In source Select CDbl(num) Into med = Median()
End Function
```

これで、次のコードに示すように、`Median` 型と `integer` 型の両方に対して、`double` のオーバーロードを呼び出すことができるようになりました。

```vb
Dim numbers1() As Double = {1.9, 2, 8, 4, 5.7, 6, 7.2, 0}

Dim query1 = Aggregate num In numbers1 Into Median()

Console.WriteLine("Double: Median = " & query1)
```

```vb
Dim numbers2() As Integer = {1, 2, 3, 4, 5}

Dim query2 = Aggregate num In numbers2 Into Median()

Console.WriteLine("Integer: Median = " & query2)
```

```vb
' This code produces the following output:
'
' Double: Median = 4.85
' Integer: Median = 3
```

#### <a name="to-create-a-generic-overload"></a>ジェネリック オーバーロードを作成するには

一連のジェネリック オブジェクトを受け取るオーバーロードを作成することもできます。 このオーバーロードは、デリゲートをパラメーターとして受け取り、ジェネリック型の一連のオブジェクトを特定の型に変換します。

次のコードは、`Median` デリゲートをパラメーターとして受け取る <xref:System.Func%602> メソッドのオーバーロードを示しています。 このデリゲートは、ジェネリック型 T のオブジェクトを受け取り、`double` 型のオブジェクトを返します。

```vb
' Generic overload.

<Extension()>
Function Median(Of T)(ByVal source As IEnumerable(Of T),
                      ByVal selector As Func(Of T, Double)) As Double
    Return Aggregate num In source Select selector(num) Into med = Median()
End Function
```

これで、任意の型の一連のオブジェクトに対して `Median` メソッドを呼び出すことができます。 型に固有のメソッド オーバーロードがない場合は、デリゲート パラメーターを渡す必要があります。 Visual Basic では、この目的にラムダ式を使用できます。 また、メソッドの呼び出しではなく、`Aggregate` または `Group By` 句を使用する場合は、この句のスコープ内にある任意の値または式を渡すことができます。

次のコード例では、整数の配列と文字列の配列に対して `Median` メソッドを呼び出す方法を示します。 文字列の場合は、配列に格納されている文字列の長さの中央値が計算されます。 この例は、それぞれのケースについて、<xref:System.Func%602> メソッドに `Median` デリゲート パラメーターを渡す方法を示しています。

```vb
Dim numbers3() As Integer = {1, 2, 3, 4, 5}

' You can use num as a parameter for the Median method
' so that the compiler will implicitly convert its value to double.
' If there is no implicit conversion, the compiler will
' display an error message.

Dim query3 = Aggregate num In numbers3 Into Median(num)

Console.WriteLine("Integer: Median = " & query3)

Dim numbers4() As String = {"one", "two", "three", "four", "five"}

' With the generic overload, you can also use numeric properties of objects.

Dim query4 = Aggregate str In numbers4 Into Median(str.Length)

Console.WriteLine("String: Median = " & query4)

' This code produces the following output:
'
' Integer: Median = 3
' String: Median = 4
```

## <a name="adding-a-method-that-returns-a-collection"></a>コレクションを返すメソッドを追加する

<xref:System.Collections.Generic.IEnumerable%601> インターフェイスは、一連の値を返すカスタム クエリ メソッドを追加することで拡張できます。 その場合、メソッドで型 <xref:System.Collections.Generic.IEnumerable%601> のコレクションを返す必要があります。 このようなメソッドを使用すると、一連の値にフィルターまたはデータ変換を適用することができます。

次の例では、コレクション内の最初の要素から 1 つおきに要素を返す `AlternateElements` という名前の拡張メソッドを作成する方法を示しています。

```vb
' Extension method for the IEnumerable(of T) interface.
' The method returns every other element of a sequence.

<Extension()>
Function AlternateElements(Of T)(
    ByVal source As IEnumerable(Of T)
    ) As IEnumerable(Of T)

    Dim list As New List(Of T)
    Dim i = 0
    For Each element In source
        If (i Mod 2 = 0) Then
            list.Add(element)
        End If
        i = i + 1
    Next
    Return list
End Function
```

この拡張メソッドは、次のコードに示すとおり、<xref:System.Collections.Generic.IEnumerable%601> インターフェイスにある他のメソッドを呼び出すときと同じように、列挙可能な任意のコレクションに対して呼び出すことができます。

```vb
Dim strings() As String = {"a", "b", "c", "d", "e"}

Dim query = strings.AlternateElements()

For Each element In query
    Console.WriteLine(element)
Next

' This code produces the following output:
'
' a
' c
' e
```

## <a name="see-also"></a>関連項目

- <xref:System.Collections.Generic.IEnumerable%601>
- [拡張メソッド](../../../../visual-basic/programming-guide/language-features/procedures/extension-methods.md)
