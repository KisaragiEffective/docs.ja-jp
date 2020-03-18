---
title: コレクション (C#)
ms.date: 07/20/2015
ms.assetid: 317d7dc3-8587-4873-8b3e-556f86497939
ms.openlocfilehash: 23d73a26bbe0e711bb3a081994826e06634c4bac
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73418707"
---
# <a name="collections-c"></a>コレクション (C#)

多くのアプリケーションで、関連するオブジェクトのグループの作成および管理が必要になります。 オブジェクトをグループ化するには、オブジェクトの配列を作成する方法と、オブジェクトのコレクションを作成する方法があります。

配列は、数が固定されている厳密に型指定されたオブジェクトの作成および処理に最も適しています。 配列の詳細については、「[配列](../arrays/index.md)」を参照してください。

コレクションは、オブジェクトのグループをより柔軟に処理できます。 配列の場合とは違って、コレクションで扱うオブジェクトのグループは、アプリケーションの変更に伴う必要に応じて動的に拡大および縮小できます。 コレクションによっては、コレクションに含まれるオブジェクトのキーを割り当てると、そのキーを使用してオブジェクトを迅速に取り出すことができます。

コレクションはクラスであるため、そのコレクションに要素を追加するには、事前にそのクラスのインスタンスを宣言する必要があります。

含まれる要素が 1 つのデータ型だけのコレクションの場合は、<xref:System.Collections.Generic?displayProperty=nameWithType> 名前空間のクラスのいずれかを使用できます。 ジェネリック コレクションでは、タイプ セーフが強制されるため、他のデータ型を追加することはできません。 ジェネリック コレクションから要素を取得する場合は、データ型を判断したり、変換したりする必要はありません。

> [!NOTE]
> このトピックの例には、`System.Collections.Generic` 名前空間および `System.Linq` 名前空間の [using](../../language-reference/keywords/using-directive.md) ディレクティブがあります。

 **このトピックの内容**

- [単純なコレクションを使用する](#BKMK_SimpleCollection)

- [コレクションの種類](#BKMK_KindsOfCollections)

  - [System.Collections.Generic のクラス](#BKMK_Generic)

  - [System.Collections.Concurrent のクラス](#BKMK_Concurrent)

  - [System.Collections のクラス](#BKMK_Collections)

- [キーと値のペアのコレクションを実装する](#BKMK_KeyValuePairs)

- [LINQ を使用してコレクションにアクセスする](#BKMK_LINQ)

- [コレクションを並べ替える](#BKMK_Sorting)

- [カスタム コレクションを定義する](#BKMK_CustomCollection)

- [反復子](#BKMK_Iterators)

<a name="BKMK_SimpleCollection"></a>

## <a name="using-a-simple-collection"></a>単純なコレクションを使用する

このセクションの例は、厳密に型指定されたオブジェクトの一覧を使用できる、ジェネリックの <xref:System.Collections.Generic.List%601> クラスを使用します。

次の例は、文字列の一覧を作成した後、[foreach](../../language-reference/keywords/foreach-in.md) ステートメントを使用して文字列を反復処理します。

```csharp
// Create a list of strings.
var salmons = new List<string>();
salmons.Add("chinook");
salmons.Add("coho");
salmons.Add("pink");
salmons.Add("sockeye");

// Iterate through the list.
foreach (var salmon in salmons)
{
    Console.Write(salmon + " ");
}
// Output: chinook coho pink sockeye
```

コレクションのコンテンツが既知の場合、コレクションの初期化に*コレクション初期化子*を使用できます。 詳細については、「[オブジェクト初期化子とコレクション初期化子](../classes-and-structs/object-and-collection-initializers.md)」を参照してください。

次の例は、コレクションへの要素の追加にコレクション初期化子を使用する以外、前の例と同じです。

```csharp
// Create a list of strings by using a
// collection initializer.
var salmons = new List<string> { "chinook", "coho", "pink", "sockeye" };

// Iterate through the list.
foreach (var salmon in salmons)
{
    Console.Write(salmon + " ");
}
// Output: chinook coho pink sockeye
```

コレクションを反復処理するには、`foreach` ステートメントの代わりに、[for](../../language-reference/keywords/for.md) ステートメントを使用できます。 インデックス位置によってコレクションの要素にアクセスすることで、これを実現します。 要素のインデックスは、0 から開始し、要素の数から 1 少ない値で終了します。

次の例は、`for` の代わりに `foreach` を使用して、コレクションの要素を反復処理します。

```csharp
// Create a list of strings by using a
// collection initializer.
var salmons = new List<string> { "chinook", "coho", "pink", "sockeye" };

for (var index = 0; index < salmons.Count; index++)
{
    Console.Write(salmons[index] + " ");
}
// Output: chinook coho pink sockeye
```

次の例は、削除するオブジェクトを指定して、コレクションから要素を削除します。

```csharp
// Create a list of strings by using a
// collection initializer.
var salmons = new List<string> { "chinook", "coho", "pink", "sockeye" };

// Remove an element from the list by specifying
// the object.
salmons.Remove("coho");

// Iterate through the list.
foreach (var salmon in salmons)
{
    Console.Write(salmon + " ");
}
// Output: chinook pink sockeye
```

次の例では、ジェネリック リストからすべての要素を削除します。 `foreach` ステートメントの代わりに、降順に反復する [for](../../language-reference/keywords/for.md) ステートメントを使用します。 これは、<xref:System.Collections.Generic.List%601.RemoveAt%2A> メソッドを実行すると、削除された要素の後にある各要素のインデックス値が小さくなるためです。

```csharp
var numbers = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };

// Remove odd numbers.
for (var index = numbers.Count - 1; index >= 0; index--)
{
    if (numbers[index] % 2 == 1)
    {
        // Remove the element by specifying
        // the zero-based index in the list.
        numbers.RemoveAt(index);
    }
}

// Iterate through the list.
// A lambda expression is placed in the ForEach method
// of the List(T) object.
numbers.ForEach(
    number => Console.Write(number + " "));
// Output: 0 2 4 6 8
```

<xref:System.Collections.Generic.List%601> の要素の型は、独自のクラスでも定義できます。 次の例では、`Galaxy` が使用する <xref:System.Collections.Generic.List%601> クラスがコードに定義されます。

```csharp
private static void IterateThroughList()
{
    var theGalaxies = new List<Galaxy>
        {
            new Galaxy() { Name="Tadpole", MegaLightYears=400},
            new Galaxy() { Name="Pinwheel", MegaLightYears=25},
            new Galaxy() { Name="Milky Way", MegaLightYears=0},
            new Galaxy() { Name="Andromeda", MegaLightYears=3}
        };

    foreach (Galaxy theGalaxy in theGalaxies)
    {
        Console.WriteLine(theGalaxy.Name + "  " + theGalaxy.MegaLightYears);
    }

    // Output:
    //  Tadpole  400
    //  Pinwheel  25
    //  Milky Way  0
    //  Andromeda  3
}

public class Galaxy
{
    public string Name { get; set; }
    public int MegaLightYears { get; set; }
}
```

<a name="BKMK_KindsOfCollections"></a>

## <a name="kinds-of-collections"></a>コレクションの種類

.NET Framework は、多くの共通のコレクションを提供します。 各コレクションの型は、固有の目的に合わせてデザインされています。

このセクションでは、次の共通のコレクション クラスをいつくか説明します:

- <xref:System.Collections.Generic> クラス

- <xref:System.Collections.Concurrent> クラス

- <xref:System.Collections> クラス

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

詳細については、「[一般的に使用されるコレクション型](../../../standard/collections/commonly-used-collection-types.md)」、「[コレクション クラスの選択](../../../standard/collections/selecting-a-collection-class.md)」、「<xref:System.Collections.Generic>」を参照してください。

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

<a name="BKMK_KeyValuePairs"></a>

## <a name="implementing-a-collection-of-keyvalue-pairs"></a>キーと値のペアのコレクションを実装する

<xref:System.Collections.Generic.Dictionary%602> ジェネリック コレクションでは、各要素のキーを使用してコレクションの要素にアクセスできます。 ディクショナリに追加される各エントリは、値とその値に関連付けられたキーで構成されます。 `Dictionary` クラスはハッシュ テーブルとして実装されているため、キーを使用した値の取得は非常に高速になります。

次の例では `Dictionary` のコレクションを作成し、`foreach` ステートメントを使用してディクショナリを反復処理します。

```csharp
private static void IterateThruDictionary()
{
    Dictionary<string, Element> elements = BuildDictionary();

    foreach (KeyValuePair<string, Element> kvp in elements)
    {
        Element theElement = kvp.Value;

        Console.WriteLine("key: " + kvp.Key);
        Console.WriteLine("values: " + theElement.Symbol + " " +
            theElement.Name + " " + theElement.AtomicNumber);
    }
}

private static Dictionary<string, Element> BuildDictionary()
{
    var elements = new Dictionary<string, Element>();

    AddToDictionary(elements, "K", "Potassium", 19);
    AddToDictionary(elements, "Ca", "Calcium", 20);
    AddToDictionary(elements, "Sc", "Scandium", 21);
    AddToDictionary(elements, "Ti", "Titanium", 22);

    return elements;
}

private static void AddToDictionary(Dictionary<string, Element> elements,
    string symbol, string name, int atomicNumber)
{
    Element theElement = new Element();

    theElement.Symbol = symbol;
    theElement.Name = name;
    theElement.AtomicNumber = atomicNumber;

    elements.Add(key: theElement.Symbol, value: theElement);
}

public class Element
{
    public string Symbol { get; set; }
    public string Name { get; set; }
    public int AtomicNumber { get; set; }
}
```

コレクション初期化子を使用して `Dictionary` コレクションをビルドする代わりに、`BuildDictionary` および `AddToDictionary` メソッドを次のメソッドに置換できます。

```csharp
private static Dictionary<string, Element> BuildDictionary2()
{
    return new Dictionary<string, Element>
    {
        {"K",
            new Element() { Symbol="K", Name="Potassium", AtomicNumber=19}},
        {"Ca",
            new Element() { Symbol="Ca", Name="Calcium", AtomicNumber=20}},
        {"Sc",
            new Element() { Symbol="Sc", Name="Scandium", AtomicNumber=21}},
        {"Ti",
            new Element() { Symbol="Ti", Name="Titanium", AtomicNumber=22}}
    };
}
```

次の例では、キーによって項目をすばやく検索するために、<xref:System.Collections.Generic.Dictionary%602.ContainsKey%2A> の <xref:System.Collections.Generic.Dictionary%602.Item%2A> メソッドと `Dictionary` プロパティを使用します。 `Item` プロパティでは、C# の `elements[symbol]` を使用して `elements` コレクションの項目にアクセスできます。

```csharp
private static void FindInDictionary(string symbol)
{
    Dictionary<string, Element> elements = BuildDictionary();

    if (elements.ContainsKey(symbol) == false)
    {
        Console.WriteLine(symbol + " not found");
    }
    else
    {
        Element theElement = elements[symbol];
        Console.WriteLine("found: " + theElement.Name);
    }
}
```

次の例では、キーによって項目をすばやく検索するために、<xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A> メソッドを代わりに使用します。

```csharp
private static void FindInDictionary2(string symbol)
{
    Dictionary<string, Element> elements = BuildDictionary();

    Element theElement = null;
    if (elements.TryGetValue(symbol, out theElement) == false)
        Console.WriteLine(symbol + " not found");
    else
        Console.WriteLine("found: " + theElement.Name);
}
```

<a name="BKMK_LINQ"></a>

## <a name="using-linq-to-access-a-collection"></a>LINQ を使用してコレクションにアクセスする

統合言語クエリ (LINQ) を使用してコレクションにアクセスできます。 LINQ クエリは、フィルター処理、並べ替え、およびグループ化の機能を提供します。 詳細については、「[Getting Started with LINQ in C#](/dotnet/csharp/programming-guide/concepts/linq/)」 (C# での LINQ の概要) を参照してください。

次の例では、ジェネリック `List` に対して LINQ クエリを実行します。 LINQ クエリは、結果が格納されている別のコレクションを戻します。

```csharp
private static void ShowLINQ()
{
    List<Element> elements = BuildList();

    // LINQ Query.
    var subset = from theElement in elements
                 where theElement.AtomicNumber < 22
                 orderby theElement.Name
                 select theElement;

    foreach (Element theElement in subset)
    {
        Console.WriteLine(theElement.Name + " " + theElement.AtomicNumber);
    }

    // Output:
    //  Calcium 20
    //  Potassium 19
    //  Scandium 21
}

private static List<Element> BuildList()
{
    return new List<Element>
    {
        { new Element() { Symbol="K", Name="Potassium", AtomicNumber=19}},
        { new Element() { Symbol="Ca", Name="Calcium", AtomicNumber=20}},
        { new Element() { Symbol="Sc", Name="Scandium", AtomicNumber=21}},
        { new Element() { Symbol="Ti", Name="Titanium", AtomicNumber=22}}
    };
}

public class Element
{
    public string Symbol { get; set; }
    public string Name { get; set; }
    public int AtomicNumber { get; set; }
}
```

<a name="BKMK_Sorting"></a>

## <a name="sorting-a-collection"></a>コレクションを並べ替える

次の例では、コレクションを並べ替えるための手順を示しています。 この例は、`Car` に格納されている <xref:System.Collections.Generic.List%601> クラスのインスタンスの並べ替えを実行します。 `Car` クラスは、<xref:System.IComparable%601> のメソッドの実装を必要とする <xref:System.IComparable%601.CompareTo%2A> インターフェイスを実装します。

<xref:System.IComparable%601.CompareTo%2A> メソッドに対する各呼び出しは、並べ替えに使用される単一の比較を実行します。 `CompareTo` メソッドのユーザーが作成したコードは、現在のオブジェクトと別のオブジェクトとの各比較の値を戻します。 現在のオブジェクトが別のオブジェクトよりも小さい場合はゼロ未満の値を、大きい場合はゼロ以上の値を、等しい場合はゼロを戻します。 これによって、より大きい、より小さい、等しい、の条件をコードに定義することができます。

`ListCars` のメソッドでは、`cars.Sort()` ステートメントがリストを並べ替えます。 <xref:System.Collections.Generic.List%601.Sort%2A> の <xref:System.Collections.Generic.List%601> メソッドへの呼び出しによって、`CompareTo` メソッドは `Car` 内の `List` オブジェクトに自動的に呼び出されます。

```csharp
private static void ListCars()
{
    var cars = new List<Car>
    {
        { new Car() { Name = "car1", Color = "blue", Speed = 20}},
        { new Car() { Name = "car2", Color = "red", Speed = 50}},
        { new Car() { Name = "car3", Color = "green", Speed = 10}},
        { new Car() { Name = "car4", Color = "blue", Speed = 50}},
        { new Car() { Name = "car5", Color = "blue", Speed = 30}},
        { new Car() { Name = "car6", Color = "red", Speed = 60}},
        { new Car() { Name = "car7", Color = "green", Speed = 50}}
    };

    // Sort the cars by color alphabetically, and then by speed
    // in descending order.
    cars.Sort();

    // View all of the cars.
    foreach (Car thisCar in cars)
    {
        Console.Write(thisCar.Color.PadRight(5) + " ");
        Console.Write(thisCar.Speed.ToString() + " ");
        Console.Write(thisCar.Name);
        Console.WriteLine();
    }

    // Output:
    //  blue  50 car4
    //  blue  30 car5
    //  blue  20 car1
    //  green 50 car7
    //  green 10 car3
    //  red   60 car6
    //  red   50 car2
}

public class Car : IComparable<Car>
{
    public string Name { get; set; }
    public int Speed { get; set; }
    public string Color { get; set; }

    public int CompareTo(Car other)
    {
        // A call to this method makes a single comparison that is
        // used for sorting.

        // Determine the relative order of the objects being compared.
        // Sort by color alphabetically, and then by speed in
        // descending order.

        // Compare the colors.
        int compare;
        compare = String.Compare(this.Color, other.Color, true);

        // If the colors are the same, compare the speeds.
        if (compare == 0)
        {
            compare = this.Speed.CompareTo(other.Speed);

            // Use descending order for speed.
            compare = -compare;
        }

        return compare;
    }
}
```

<a name="BKMK_CustomCollection"></a>

## <a name="defining-a-custom-collection"></a>カスタム コレクションを定義する

<xref:System.Collections.Generic.IEnumerable%601> または <xref:System.Collections.IEnumerable> のインターフェイスを実装してコレクションを定義できます。

カスタム コレクションを定義できますが、通常は、.NET Framework に含まれるコレクションを使用することが推奨されます。これについては、このトピックの[コレクションの種類](#BKMK_KindsOfCollections)で既に説明されています。

次の例は、`AllColors` という名前のカスタム コレクション クラスを定義します。 このクラスは、<xref:System.Collections.IEnumerable> メソッドの実装を必要とする <xref:System.Collections.IEnumerable.GetEnumerator%2A> インターフェイスを実装します。

`GetEnumerator` メソッドは、`ColorEnumerator` クラスのインスタンスを戻します。 `ColorEnumerator` は、<xref:System.Collections.IEnumerator> プロパティ、<xref:System.Collections.IEnumerator.Current%2A> メソッド、および <xref:System.Collections.IEnumerator.MoveNext%2A> メソッドの実装を必要とする <xref:System.Collections.IEnumerator.Reset%2A> インターフェイスを実装します。

```csharp
private static void ListColors()
{
    var colors = new AllColors();

    foreach (Color theColor in colors)
    {
        Console.Write(theColor.Name + " ");
    }
    Console.WriteLine();
    // Output: red blue green
}

// Collection class.
public class AllColors : System.Collections.IEnumerable
{
    Color[] _colors =
    {
        new Color() { Name = "red" },
        new Color() { Name = "blue" },
        new Color() { Name = "green" }
    };

    public System.Collections.IEnumerator GetEnumerator()
    {
        return new ColorEnumerator(_colors);

        // Instead of creating a custom enumerator, you could
        // use the GetEnumerator of the array.
        //return _colors.GetEnumerator();
    }

    // Custom enumerator.
    private class ColorEnumerator : System.Collections.IEnumerator
    {
        private Color[] _colors;
        private int _position = -1;

        public ColorEnumerator(Color[] colors)
        {
            _colors = colors;
        }

        object System.Collections.IEnumerator.Current
        {
            get
            {
                return _colors[_position];
            }
        }

        bool System.Collections.IEnumerator.MoveNext()
        {
            _position++;
            return (_position < _colors.Length);
        }

        void System.Collections.IEnumerator.Reset()
        {
            _position = -1;
        }
    }
}

// Element class.
public class Color
{
    public string Name { get; set; }
}
```

<a name="BKMK_Iterators"></a>

## <a name="iterators"></a>Iterators

*反復子*は、コレクションに対するカスタム イテレーションを実行するために使用されます。 反復子は、メソッドまたは `get` アクセサーのいずれかです。 反復子は、[yield return](../../language-reference/keywords/yield.md) ステートメントを使用して、コレクションの各要素を 1 回に 1 つ返します。

[foreach](../../language-reference/keywords/foreach-in.md) ステートメントを使用して、反復子を呼び出します。 `foreach` ループの各イテレーションは、反復子を呼び出します。 `yield return` ステートメントが反復子に到達すると、式が戻され、コードの現在の位置が保持されます。 次回、反復子が呼び出されると、この位置から実行が再開されます。

詳細については、「[反復子 (C#)](./iterators.md)」を参照してください。

次の例は、反復子メソッドを使用します。 反復子メソッドには、[for](../../language-reference/keywords/for.md) ループ内に `yield return` ステートメントがあります。 `ListEvenNumbers` メソッドでは、`foreach` ステートメント本文の各イテレーションが、反復子メソッドの呼び出しを作成し、これが次の `yield return` ステートメントに続行されます。

```csharp
private static void ListEvenNumbers()
{
    foreach (int number in EvenSequence(5, 18))
    {
        Console.Write(number.ToString() + " ");
    }
    Console.WriteLine();
    // Output: 6 8 10 12 14 16 18
}

private static IEnumerable<int> EvenSequence(
    int firstNumber, int lastNumber)
{
    // Yield even numbers in the range.
    for (var number = firstNumber; number <= lastNumber; number++)
    {
        if (number % 2 == 0)
        {
            yield return number;
        }
    }
}
```

## <a name="see-also"></a>関連項目

- [オブジェクト初期化子とコレクション初期化子](../classes-and-structs/object-and-collection-initializers.md)
- [プログラミングの概念 (C#)](./index.md)
- [Option Strict ステートメント](../../../visual-basic/language-reference/statements/option-strict-statement.md)
- [LINQ to Objects (C#)](./linq/linq-to-objects.md)
- [Parallel LINQ (PLINQ)](../../../standard/parallel-programming/parallel-linq-plinq.md)
- [コレクションとデータ構造体](../../../standard/collections/index.md)
- [コレクション クラスの選択](../../../standard/collections/selecting-a-collection-class.md)
- [コレクション内での比較と並べ替え](../../../standard/collections/comparisons-and-sorts-within-collections.md)
- [ジェネリック コレクションを使用する状況](../../../standard/collections/when-to-use-generic-collections.md)
