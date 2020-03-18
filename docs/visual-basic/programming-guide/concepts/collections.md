---
title: コレクション
ms.date: 07/20/2015
ms.assetid: 5f7749f3-aaf2-4319-b63c-bfa72e1e2b7a
ms.openlocfilehash: ba16d04e781bcf69356b1f603d92e104816a0860
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347094"
---
# <a name="collections-visual-basic"></a>コレクション (Visual Basic)

多くのアプリケーションで、関連するオブジェクトのグループの作成および管理が必要になります。 オブジェクトをグループ化するには、オブジェクトの配列を作成する方法と、オブジェクトのコレクションを作成する方法があります。

配列は、数が固定されている厳密に型指定されたオブジェクトの作成および処理に最も適しています。 配列の詳細については、「[配列](../../../visual-basic/programming-guide/language-features/arrays/index.md)」を参照してください。

コレクションは、オブジェクトのグループをより柔軟に処理できます。 配列の場合とは違って、コレクションで扱うオブジェクトのグループは、アプリケーションの変更に伴う必要に応じて動的に拡大および縮小できます。 コレクションによっては、コレクションに含まれるオブジェクトのキーを割り当てると、そのキーを使用してオブジェクトを迅速に取り出すことができます。

コレクションはクラスであるため、そのコレクションに要素を追加するには、事前にそのクラスのインスタンスを宣言する必要があります。

含まれる要素が 1 つのデータ型だけのコレクションの場合は、<xref:System.Collections.Generic?displayProperty=nameWithType> 名前空間のクラスのいずれかを使用できます。 ジェネリック コレクションでは、タイプ セーフが強制されるため、他のデータ型を追加することはできません。 ジェネリック コレクションから要素を取得する場合は、データ型を判断したり、変換したりする必要はありません。

> [!NOTE]
> このトピックの例では、`System.Collections.Generic` および `System.Linq` 名前空間の[Imports](../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)ステートメントを含めます。

<a name="BKMK_SimpleCollection"></a>

## <a name="using-a-simple-collection"></a>単純なコレクションを使用する

このセクションの例では、厳密に型指定されたオブジェクトの一覧を使用できる、ジェネリックの <xref:System.Collections.Generic.List%601> クラスを使用します。

次の例は、文字列のリストを作成し、[For Each...Next](../../../visual-basic/language-reference/statements/for-each-next-statement.md)ステートメントを使用して、文字列を反復処理します。

```vb
' Create a list of strings.
Dim salmons As New List(Of String)
salmons.Add("chinook")
salmons.Add("coho")
salmons.Add("pink")
salmons.Add("sockeye")

' Iterate through the list.
For Each salmon As String In salmons
    Console.Write(salmon & " ")
Next
'Output: chinook coho pink sockeye
```

コレクションのコンテンツが既知の場合、コレクションの初期化に*コレクション初期化子*を使用できます。 詳細については、「[コレクション初期化子](../../../visual-basic/programming-guide/language-features/collection-initializers/index.md)」を参照してください。

次の例は、コレクションへの要素の追加にコレクション初期化子を使用する以外、前の例と同じです。

```vb
' Create a list of strings by using a
' collection initializer.
Dim salmons As New List(Of String) From
    {"chinook", "coho", "pink", "sockeye"}

For Each salmon As String In salmons
    Console.Write(salmon & " ")
Next
'Output: chinook coho pink sockeye
```

`For Each`ステートメントの代わりに、[For...Next](../../../visual-basic/language-reference/statements/for-next-statement.md)ステートメントをコレクションの反復処理に使用することができます。 インデックス位置によってコレクションの要素にアクセスすることで、これを実現します。 要素のインデックスは、0 から開始し、要素の数から 1 少ない値で終了します。

次の例は、`For…Next` の代わりに `For Each` を使用して、コレクションの要素を反復処理します。

```vb
Dim salmons As New List(Of String) From
    {"chinook", "coho", "pink", "sockeye"}

For index = 0 To salmons.Count - 1
    Console.Write(salmons(index) & " ")
Next
'Output: chinook coho pink sockeye
```

次の例は、削除するオブジェクトを指定して、コレクションから要素を削除します。

```vb
' Create a list of strings by using a
' collection initializer.
Dim salmons As New List(Of String) From
    {"chinook", "coho", "pink", "sockeye"}

' Remove an element in the list by specifying
' the object.
salmons.Remove("coho")

For Each salmon As String In salmons
    Console.Write(salmon & " ")
Next
'Output: chinook pink sockeye
```

次の例では、ジェネリック リストからすべての要素を削除します。 `For Each`ステートメントの代わりに、降順に反復処理する[For...Next](../../../visual-basic/language-reference/statements/for-next-statement.md)ステートメントを使用します。 これは、<xref:System.Collections.Generic.List%601.RemoveAt%2A> メソッドを実行すると、削除された要素の後にある要素のインデックス値が小さくなるためです。

```vb
Dim numbers As New List(Of Integer) From
    {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}

' Remove odd numbers.
For index As Integer = numbers.Count - 1 To 0 Step -1
    If numbers(index) Mod 2 = 1 Then
        ' Remove the element by specifying
        ' the zero-based index in the list.
        numbers.RemoveAt(index)
    End If
Next

' Iterate through the list.
' A lambda expression is placed in the ForEach method
' of the List(T) object.
numbers.ForEach(
    Sub(number) Console.Write(number & " "))
' Output: 0 2 4 6 8
```

<xref:System.Collections.Generic.List%601> の要素の型には、独自のクラスも定義できます。 次の例では、<xref:System.Collections.Generic.List%601> が使用する `Galaxy` クラスがコードに定義されています。

```vb
Private Sub IterateThroughList()
    Dim theGalaxies As New List(Of Galaxy) From
        {
            New Galaxy With {.Name = "Tadpole", .MegaLightYears = 400},
            New Galaxy With {.Name = "Pinwheel", .MegaLightYears = 25},
            New Galaxy With {.Name = "Milky Way", .MegaLightYears = 0},
            New Galaxy With {.Name = "Andromeda", .MegaLightYears = 3}
        }

    For Each theGalaxy In theGalaxies
        With theGalaxy
            Console.WriteLine(.Name & "  " & .MegaLightYears)
        End With
    Next

    ' Output:
    '  Tadpole  400
    '  Pinwheel  25
    '  Milky Way  0
    '  Andromeda  3
End Sub

Public Class Galaxy
    Public Property Name As String
    Public Property MegaLightYears As Integer
End Class
```

<a name="BKMK_KindsOfCollections"></a>

## <a name="kinds-of-collections"></a>コレクションの種類

.NET Framework は、多くの共通のコレクションを提供します。 各コレクションの型は、固有の目的に合わせてデザインされています。

このセクションでは、次の共通のコレクション クラスをいつくか説明します:

- <xref:System.Collections.Generic> クラス

- <xref:System.Collections.Concurrent> クラス

- <xref:System.Collections> クラス

- Visual Basic の `Collection` クラス

<a name="BKMK_Generic"></a>

### <a name="systemcollectionsgeneric-classes"></a>System.Collections.Generic のクラス

<xref:System.Collections.Generic> 名前空間の 1 つのクラスを使用すると、ジェネリック コレクションを作成できます。 ジェネリック コレクションは、コレクション内のすべての項目が同じデータ型である場合に便利です。 ジェネリック コレクションには該当するデータ型しか追加できないため、厳密な型指定が適用されます。

次のテーブルは <xref:System.Collections.Generic?displayProperty=nameWithType> 名前空間でよく使用されるクラスの一覧です:

|クラス|説明|
|---|---|
|<xref:System.Collections.Generic.Dictionary%602>|キーに基づいて編成された、キーと値のペアのコレクションを表します。|
|<xref:System.Collections.Generic.List%601>|インデックスを使用してアクセスできる、オブジェクトの一覧を表します。 リストの検索、並べ替え、および変更のメソッドを提供します。|
|<xref:System.Collections.Generic.Queue%601>|先入れ先出し (FIFO) のオブジェクトのコレクションを表します。|
|<xref:System.Collections.Generic.SortedList%602>|関連付けられた <xref:System.Collections.Generic.IComparer%601> 実装に基づいて、キーにより並べ替えられた、キーと値のペアのコレクションを表します。|
|<xref:System.Collections.Generic.Stack%601>|後入れ先出し (LIFO) のオブジェクトのコレクションを表します。|

詳細については、「[一般的に使用されるコレクション型](../../../standard/collections/commonly-used-collection-types.md)」、「[コレクション クラスの選択](../../../standard/collections/selecting-a-collection-class.md)」、「<xref:System.Collections.Generic?displayProperty=nameWithType>」を参照してください。

<a name="BKMK_Concurrent"></a>

### <a name="systemcollectionsconcurrent-classes"></a>System.Collections.Concurrent のクラス

.NET Framework 4 以降では、<xref:System.Collections.Concurrent> 名前空間のコレクションによって、複数のスレッドからコレクション項目にアクセスするための効率的なスレッド セーフ操作が可能になります。

<xref:System.Collections.Concurrent> 名前空間のクラスは、複数のスレッドがコレクションに同時にアクセスするときに、<xref:System.Collections.Generic?displayProperty=nameWithType> 名前空間および <xref:System.Collections?displayProperty=nameWithType> 名前空間の対応する型の代わりに使用する必要があります。 詳しくは、「[スレッド セーフなコレクション](../../../standard/collections/thread-safe/index.md)」と「<xref:System.Collections.Concurrent>」を参照してください。

<xref:System.Collections.Concurrent> 名前空間には、<xref:System.Collections.Concurrent.BlockingCollection%601>、<xref:System.Collections.Concurrent.ConcurrentDictionary%602>、<xref:System.Collections.Concurrent.ConcurrentQueue%601>、および <xref:System.Collections.Concurrent.ConcurrentStack%601> などのクラスがあります。

<a name="BKMK_Collections"></a>

### <a name="systemcollections-classes"></a>System.Collections のクラス

<xref:System.Collections?displayProperty=nameWithType> 名前空間のクラスでは、要素は、固有の型のオブジェクトとしてではなく `Object` 型のオブジェクトとして格納されます。

できる限り、<xref:System.Collections.Generic?displayProperty=nameWithType> 名前空間の従来の型の代わりに、<xref:System.Collections.Concurrent> 名前空間または `System.Collections` 名前空間のジェネリック コレクションを使用します。

次のテーブルは `System.Collections` 名前空間でよく使用されるクラスの一覧です:

|クラス|説明|
|---|---|
|<xref:System.Collections.ArrayList>|必要に応じてサイズが動的に拡大されるオブジェクトの配列を表します。|
|<xref:System.Collections.Hashtable>|キーのハッシュ コードに基づいて編成された、キーと値のペアのコレクションを表します。|
|<xref:System.Collections.Queue>|先入れ先出し (FIFO) のオブジェクトのコレクションを表します。|
|<xref:System.Collections.Stack>|後入れ先出し (LIFO) のオブジェクトのコレクションを表します。|

<xref:System.Collections.Specialized> 名前空間には、文字列専用のコレクションやリンク リスト、ハイブリッド ディクショナリなど、厳密に型指定された専用のコレクション クラスが用意されています。

<a name="BKMK_VisualBasic"></a>

### <a name="visual-basic-collection-class"></a>Visual Basic のコレクション クラス

数値インデックスまたは `String` キーを使用したコレクション項目にアクセスするには、Visual Basic の <xref:Microsoft.VisualBasic.Collection> クラスを使用できます。 キーを指定してもしなくても、項目をコレクション オブジェクトに追加できます。 キーを使わずに項目を追加した場合は、その項目にアクセスするときに数値インデックスを使う必要があります。

Visual Basic の `Collection` クラスは、そのすべての要素を `Object` 型として保存するため、任意のデータ型の項目を追加できます。 不適切なデータ型が追加されないようにする保護機能はありません。

Visual Basic の `Collection` クラスを使用すると、コレクション内の最初の項目はインデックス 1 となります。 これは、インデックスが 0 から開始される .NET Framework のコレクション クラスとは異なります。

できる限り、Visual Basic の `Collection` クラスの代わりに、<xref:System.Collections.Generic?displayProperty=nameWithType> 名前空間または <xref:System.Collections.Concurrent> 名前空間のジェネリック コレクションを使用してください。

詳細については、「 <xref:Microsoft.VisualBasic.Collection> 」を参照してください。

<a name="BKMK_KeyValuePairs"></a>

## <a name="implementing-a-collection-of-keyvalue-pairs"></a>キーと値のペアのコレクションを実装する

<xref:System.Collections.Generic.Dictionary%602> ジェネリック コレクションでは、各要素のキーを使用してコレクションの要素にアクセスできます。 ディクショナリに追加される各エントリは、値とその値に関連付けられたキーで構成されます。 `Dictionary` クラスはハッシュ テーブルとして実装されているため、キーを使用した値の取得は非常に高速になります。

次の例では `Dictionary` のコレクションを作成し、`For Each` ステートメントを使用してディクショナリを反復処理します。

```vb
Private Sub IterateThroughDictionary()
    Dim elements As Dictionary(Of String, Element) = BuildDictionary()

    For Each kvp As KeyValuePair(Of String, Element) In elements
        Dim theElement As Element = kvp.Value

        Console.WriteLine("key: " & kvp.Key)
        With theElement
            Console.WriteLine("values: " & .Symbol & " " &
                .Name & " " & .AtomicNumber)
        End With
    Next
End Sub

Private Function BuildDictionary() As Dictionary(Of String, Element)
    Dim elements As New Dictionary(Of String, Element)

    AddToDictionary(elements, "K", "Potassium", 19)
    AddToDictionary(elements, "Ca", "Calcium", 20)
    AddToDictionary(elements, "Sc", "Scandium", 21)
    AddToDictionary(elements, "Ti", "Titanium", 22)

    Return elements
End Function

Private Sub AddToDictionary(ByVal elements As Dictionary(Of String, Element),
ByVal symbol As String, ByVal name As String, ByVal atomicNumber As Integer)
    Dim theElement As New Element

    theElement.Symbol = symbol
    theElement.Name = name
    theElement.AtomicNumber = atomicNumber

    elements.Add(Key:=theElement.Symbol, value:=theElement)
End Sub

Public Class Element
    Public Property Symbol As String
    Public Property Name As String
    Public Property AtomicNumber As Integer
End Class
```

コレクション初期化子を使用して `Dictionary` コレクションをビルドする代わりに、`BuildDictionary` および `AddToDictionary` メソッドを次のメソッドに置換できます。

```vb
Private Function BuildDictionary2() As Dictionary(Of String, Element)
    Return New Dictionary(Of String, Element) From
        {
            {"K", New Element With
                {.Symbol = "K", .Name = "Potassium", .AtomicNumber = 19}},
            {"Ca", New Element With
                {.Symbol = "Ca", .Name = "Calcium", .AtomicNumber = 20}},
            {"Sc", New Element With
                {.Symbol = "Sc", .Name = "Scandium", .AtomicNumber = 21}},
            {"Ti", New Element With
                {.Symbol = "Ti", .Name = "Titanium", .AtomicNumber = 22}}
        }
End Function
```

次の例では、キーによって項目をすばやく検索するために、<xref:System.Collections.Generic.Dictionary%602.ContainsKey%2A> の <xref:System.Collections.Generic.Dictionary%602.Item%2A> メソッドと `Dictionary` プロパティを使用します。 `Item` プロパティを使用すると、Visual Basic の `elements(symbol)` コードを使用して、`elements` コレクション内の項目にアクセスできます。

```vb
Private Sub FindInDictionary(ByVal symbol As String)
    Dim elements As Dictionary(Of String, Element) = BuildDictionary()

    If elements.ContainsKey(symbol) = False Then
        Console.WriteLine(symbol & " not found")
    Else
        Dim theElement = elements(symbol)
        Console.WriteLine("found: " & theElement.Name)
    End If
End Sub
```

次の例では、キーによって項目をすばやく検索するために、<xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A> メソッドを代わりに使用します。

```vb
Private Sub FindInDictionary2(ByVal symbol As String)
    Dim elements As Dictionary(Of String, Element) = BuildDictionary()

    Dim theElement As Element = Nothing
    If elements.TryGetValue(symbol, theElement) = False Then
        Console.WriteLine(symbol & " not found")
    Else
        Console.WriteLine("found: " & theElement.Name)
    End If
End Sub
```

<a name="BKMK_LINQ"></a>

## <a name="using-linq-to-access-a-collection"></a>LINQ を使用してコレクションにアクセスする

統合言語クエリ (LINQ) を使用してコレクションにアクセスできます。 LINQ クエリは、フィルター処理、並べ替え、およびグループ化の機能を提供します。 詳細については、「 [Visual Basic での LINQ のはじめに](../../../visual-basic/programming-guide/concepts/linq/getting-started-with-linq.md)」を参照してください。

次の例では、ジェネリック `List` に対して LINQ クエリを実行します。 LINQ クエリは、結果が格納されている別のコレクションを戻します。

```vb
Private Sub ShowLINQ()
    Dim elements As List(Of Element) = BuildList()

    ' LINQ Query.
    Dim subset = From theElement In elements
                  Where theElement.AtomicNumber < 22
                  Order By theElement.Name

    For Each theElement In subset
        Console.WriteLine(theElement.Name & " " & theElement.AtomicNumber)
    Next

    ' Output:
    '  Calcium 20
    '  Potassium 19
    '  Scandium 21
End Sub

Private Function BuildList() As List(Of Element)
    Return New List(Of Element) From
        {
            {New Element With
                {.Symbol = "K", .Name = "Potassium", .AtomicNumber = 19}},
            {New Element With
                {.Symbol = "Ca", .Name = "Calcium", .AtomicNumber = 20}},
            {New Element With
                {.Symbol = "Sc", .Name = "Scandium", .AtomicNumber = 21}},
            {New Element With
                {.Symbol = "Ti", .Name = "Titanium", .AtomicNumber = 22}}
        }
End Function

Public Class Element
    Public Property Symbol As String
    Public Property Name As String
    Public Property AtomicNumber As Integer
End Class
```

<a name="BKMK_Sorting"></a>

## <a name="sorting-a-collection"></a>コレクションを並べ替える

次の例では、コレクションを並べ替えるための手順を示しています。 この例は、`Car` に格納されている <xref:System.Collections.Generic.List%601> クラスのインスタンスの並べ替えを実行します。 `Car` クラスは、<xref:System.IComparable%601> のメソッドの実装を必要とする <xref:System.IComparable%601.CompareTo%2A> インターフェイスを実装します。

<xref:System.IComparable%601.CompareTo%2A> メソッドに対する各呼び出しは、並べ替えに使用される単一の比較を実行します。 `CompareTo` メソッドのユーザーが作成したコードは、現在のオブジェクトと別のオブジェクトとの各比較の値を戻します。 現在のオブジェクトが別のオブジェクトよりも小さい場合はゼロ未満の値を、大きい場合はゼロ以上の値を、等しい場合はゼロを戻します。 これによって、より大きい、より小さい、等しい、の条件をコードに定義することができます。

`ListCars` のメソッドでは、`cars.Sort()` ステートメントがリストを並べ替えます。 <xref:System.Collections.Generic.List%601.Sort%2A> の <xref:System.Collections.Generic.List%601> メソッドへの呼び出しによって、`CompareTo` メソッドは `Car` 内の `List` オブジェクトに自動的に呼び出されます。

```vb
Public Sub ListCars()

    ' Create some new cars.
    Dim cars As New List(Of Car) From
    {
        New Car With {.Name = "car1", .Color = "blue", .Speed = 20},
        New Car With {.Name = "car2", .Color = "red", .Speed = 50},
        New Car With {.Name = "car3", .Color = "green", .Speed = 10},
        New Car With {.Name = "car4", .Color = "blue", .Speed = 50},
        New Car With {.Name = "car5", .Color = "blue", .Speed = 30},
        New Car With {.Name = "car6", .Color = "red", .Speed = 60},
        New Car With {.Name = "car7", .Color = "green", .Speed = 50}
    }

    ' Sort the cars by color alphabetically, and then by speed
    ' in descending order.
    cars.Sort()

    ' View all of the cars.
    For Each thisCar As Car In cars
        Console.Write(thisCar.Color.PadRight(5) & " ")
        Console.Write(thisCar.Speed.ToString & " ")
        Console.Write(thisCar.Name)
        Console.WriteLine()
    Next

    ' Output:
    '  blue  50 car4
    '  blue  30 car5
    '  blue  20 car1
    '  green 50 car7
    '  green 10 car3
    '  red   60 car6
    '  red   50 car2
End Sub

Public Class Car
    Implements IComparable(Of Car)

    Public Property Name As String
    Public Property Speed As Integer
    Public Property Color As String

    Public Function CompareTo(ByVal other As Car) As Integer _
        Implements System.IComparable(Of Car).CompareTo
        ' A call to this method makes a single comparison that is
        ' used for sorting.

        ' Determine the relative order of the objects being compared.
        ' Sort by color alphabetically, and then by speed in
        ' descending order.

        ' Compare the colors.
        Dim compare As Integer
        compare = String.Compare(Me.Color, other.Color, True)

        ' If the colors are the same, compare the speeds.
        If compare = 0 Then
            compare = Me.Speed.CompareTo(other.Speed)

            ' Use descending order for speed.
            compare = -compare
        End If

        Return compare
    End Function
End Class
```

<a name="BKMK_CustomCollection"></a>

## <a name="defining-a-custom-collection"></a>カスタム コレクションを定義する

<xref:System.Collections.Generic.IEnumerable%601> または <xref:System.Collections.IEnumerable> のインターフェイスを実装してコレクションを定義できます。 詳細については、「[コレクションの列挙](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/hwyysy67(v=vs.100))」を参照してください。

カスタム コレクションを定義できますが、通常は、.NET Framework に含まれるコレクションを使用することが推奨されます。これについては、このトピックの[コレクションの種類](#kinds-of-collections)で既に説明されています。

次の例は、`AllColors` という名前のカスタム コレクション クラスを定義します。 このクラスは、<xref:System.Collections.IEnumerable> メソッドの実装を必要とする <xref:System.Collections.IEnumerable.GetEnumerator%2A> インターフェイスを実装します。

`GetEnumerator` メソッドは、`ColorEnumerator` クラスのインスタンスを戻します。 `ColorEnumerator` は、<xref:System.Collections.IEnumerator> プロパティ、<xref:System.Collections.IEnumerator.Current%2A> メソッド、および <xref:System.Collections.IEnumerator.MoveNext%2A> メソッドの実装を必要とする <xref:System.Collections.IEnumerator.Reset%2A> インターフェイスを実装します。

```vb
Public Sub ListColors()
    Dim colors As New AllColors()

    For Each theColor As Color In colors
        Console.Write(theColor.Name & " ")
    Next
    Console.WriteLine()
    ' Output: red blue green
End Sub

' Collection class.
Public Class AllColors
    Implements System.Collections.IEnumerable

    Private _colors() As Color =
    {
        New Color With {.Name = "red"},
        New Color With {.Name = "blue"},
        New Color With {.Name = "green"}
    }

    Public Function GetEnumerator() As System.Collections.IEnumerator _
        Implements System.Collections.IEnumerable.GetEnumerator

        Return New ColorEnumerator(_colors)

        ' Instead of creating a custom enumerator, you could
        ' use the GetEnumerator of the array.
        'Return _colors.GetEnumerator
    End Function

    ' Custom enumerator.
    Private Class ColorEnumerator
        Implements System.Collections.IEnumerator

        Private _colors() As Color
        Private _position As Integer = -1

        Public Sub New(ByVal colors() As Color)
            _colors = colors
        End Sub

        Public ReadOnly Property Current() As Object _
            Implements System.Collections.IEnumerator.Current
            Get
                Return _colors(_position)
            End Get
        End Property

        Public Function MoveNext() As Boolean _
            Implements System.Collections.IEnumerator.MoveNext
            _position += 1
            Return (_position < _colors.Length)
        End Function

        Public Sub Reset() Implements System.Collections.IEnumerator.Reset
            _position = -1
        End Sub
    End Class
End Class

' Element class.
Public Class Color
    Public Property Name As String
End Class
```

<a name="BKMK_Iterators"></a>

## <a name="iterators"></a>Iterators

*反復子*は、コレクションに対するカスタム イテレーションを実行するために使用されます。 反復子は、メソッドまたは `get` アクセサーのいずれかです。 反復子は[Yield](../../../visual-basic/language-reference/statements/yield-statement.md)ステートメントを使用して、コレクションの各要素を一度に 1 つずつ返します。

[For Each...Next](../../../visual-basic/language-reference/statements/for-each-next-statement.md)ステートメントを使用して、反復子を呼び出します。 `For Each` ループの繰り返しごとに、反復子を呼び出します。 反復子が`Yield` ステートメントに到達すると、式が返され、コードの現在の位置が保持されます。 次回、反復子が呼び出されると、この位置から実行が再開されます。

詳細については、[反復子 (Visual Basic)](../../../visual-basic/programming-guide/concepts/iterators.md)を参照してください。

次の例は、反復子メソッドを使用します。 反復子メソッドは、[For...Next](../../../visual-basic/language-reference/statements/for-next-statement.md)ループ内に`Yield`ステートメントを持ちます。 `ListEvenNumbers` メソッドでは、`For Each` ステートメント本体の繰り返しごとに、反復子メソッドの呼び出しを作成し、次の `Yield` ステートメントに進みます。

```vb
Public Sub ListEvenNumbers()
    For Each number As Integer In EvenSequence(5, 18)
        Console.Write(number & " ")
    Next
    Console.WriteLine()
    ' Output: 6 8 10 12 14 16 18
End Sub

Private Iterator Function EvenSequence(
ByVal firstNumber As Integer, ByVal lastNumber As Integer) _
As IEnumerable(Of Integer)

' Yield even numbers in the range.
    For number = firstNumber To lastNumber
        If number Mod 2 = 0 Then
            Yield number
        End If
    Next
End Function
```

## <a name="see-also"></a>関連項目

- [コレクション初期化子](../../../visual-basic/programming-guide/language-features/collection-initializers/index.md)
- [プログラミングの概念 (Visual Basic)](../../../visual-basic/programming-guide/concepts/index.md)
- [Option Strict ステートメント](../../../visual-basic/language-reference/statements/option-strict-statement.md)
- [LINQ to Objects (Visual Basic)](../../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)
- [Parallel LINQ (PLINQ)](../../../standard/parallel-programming/parallel-linq-plinq.md)
- [コレクションとデータ構造体](../../../standard/collections/index.md)
- [コレクション クラスの選択](../../../standard/collections/selecting-a-collection-class.md)
- [コレクション内での比較と並べ替え](../../../standard/collections/comparisons-and-sorts-within-collections.md)
- [ジェネリック コレクションを使用する状況](../../../standard/collections/when-to-use-generic-collections.md)
