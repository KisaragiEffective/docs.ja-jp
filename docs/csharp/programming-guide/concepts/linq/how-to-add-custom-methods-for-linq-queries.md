---
title: LINQ クエリのカスタム メソッドを追加する方法 (C#)
ms.date: 07/20/2015
ms.assetid: 1a500f60-2e10-49fb-8b2a-d8d08e4817cb
ms.openlocfilehash: e16175d3332b6ce36458eaa78af093e4f8772723
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74141474"
---
# <a name="how-to-add-custom-methods-for-linq-queries-c"></a>LINQ クエリのカスタム メソッドを追加する方法 (C#)

<xref:System.Collections.Generic.IEnumerable%601> インターフェイスに拡張メソッドを追加することで、LINQ クエリに使用できるメソッド セットを拡張できます。 たとえば一連の値から単一の値を求めるために、平均値や最大値を求める標準的な演算に加えて、独自の集計メソッドを作成することができます。 また、一連の値を受け取って別の一連の値を返す特定のデータ変換やカスタム フィルターの働きを持ったメソッドを作成することもできます。 このようなメソッドには、<xref:System.Linq.Enumerable.Distinct%2A>、<xref:System.Linq.Enumerable.Skip%2A>、<xref:System.Linq.Enumerable.Reverse%2A> があります。

<xref:System.Collections.Generic.IEnumerable%601> インターフェイスを拡張すると、列挙可能なコレクションにカスタム メソッドを適用できます。 詳細については、「[拡張メソッド](../../classes-and-structs/extension-methods.md)」を参照してください。

## <a name="adding-an-aggregate-method"></a>集計メソッドの追加

集計メソッドは、一連の値から単一の値を計算するメソッドです。 LINQ は、<xref:System.Linq.Enumerable.Average%2A>、<xref:System.Linq.Enumerable.Min%2A>、<xref:System.Linq.Enumerable.Max%2A> などの集計メソッドを提供します。 <xref:System.Collections.Generic.IEnumerable%601> インターフェイスに拡張メソッドを追加することで、独自の集計メソッドを作成できます。

次のコード例は、`Median` 型の一連の数値から中央値を求める `double` という拡張メソッドの作成方法を示しています。

```csharp
public static class LINQExtension
{
    public static double Median(this IEnumerable<double> source)
    {
        if (source.Count() == 0)
        {
            throw new InvalidOperationException("Cannot compute median for an empty set.");
        }

        var sortedList = from number in source
                         orderby number
                         select number;

        int itemIndex = (int)sortedList.Count() / 2;

        if (sortedList.Count() % 2 == 0)
        {
            // Even number of items.
            return (sortedList.ElementAt(itemIndex) + sortedList.ElementAt(itemIndex - 1)) / 2;
        }
        else
        {
            // Odd number of items.
            return sortedList.ElementAt(itemIndex);
        }
    }
}
```

この拡張メソッドは、<xref:System.Collections.Generic.IEnumerable%601> インターフェイスにある他の集計メソッドを呼び出すときと同じように、列挙可能な任意のコレクションに対して呼び出すことができます。

`Median` 型の配列に対して `double` メソッドを使用する方法を次のコード例に示します。

```csharp
double[] numbers1 = { 1.9, 2, 8, 4, 5.7, 6, 7.2, 0 };

var query1 = numbers1.Median();

Console.WriteLine("double: Median = " + query1);
```

```csharp
/*
 This code produces the following output:

 Double: Median = 4.85
*/
```

### <a name="overloading-an-aggregate-method-to-accept-various-types"></a>さまざまな型を受け取るために集計メソッドをオーバーロードする

集計メソッドでさまざまな型を受け取るように、集計メソッドをオーバーロードすることができます。 その標準的な方法として、型ごとにオーバーロードを作成します。 または、ジェネリック型を受け取り、デリゲートを使って特定の型に変換するオーバーロードを作成する方法もあります。 その両方の方法を組み合わせることもできます。

#### <a name="to-create-an-overload-for-each-type"></a>型ごとにオーバーロードを作成するには

サポート予定の型ごとに固有のオーバーロードを作成できます。 次のコード例では、`Median` 型の `integer` メソッドのオーバーロードを示します。

```csharp
//int overload

public static double Median(this IEnumerable<int> source)
{
    return (from num in source select (double)num).Median();
}
```

これで、次のコードに示すように、`Median` 型と `integer` 型の両方に対して、`double` のオーバーロードを呼び出すことができるようになりました。

```csharp
double[] numbers1 = { 1.9, 2, 8, 4, 5.7, 6, 7.2, 0 };

var query1 = numbers1.Median();

Console.WriteLine("double: Median = " + query1);
```

```csharp
int[] numbers2 = { 1, 2, 3, 4, 5 };

var query2 = numbers2.Median();

Console.WriteLine("int: Median = " + query2);
```

```csharp
/*
 This code produces the following output:

 Double: Median = 4.85
 Integer: Median = 3
*/
```

#### <a name="to-create-a-generic-overload"></a>ジェネリック オーバーロードを作成するには

一連のジェネリック オブジェクトを受け取るオーバーロードを作成することもできます。 このオーバーロードは、デリゲートをパラメーターとして受け取り、ジェネリック型の一連のオブジェクトを特定の型に変換します。

次のコードは、`Median` デリゲートをパラメーターとして受け取る <xref:System.Func%602> メソッドのオーバーロードを示しています。 このデリゲートは、ジェネリック型 T のオブジェクトを受け取り、`double` 型のオブジェクトを返します。

```csharp
// Generic overload.

public static double Median<T>(this IEnumerable<T> numbers,
                       Func<T, double> selector)
{
    return (from num in numbers select selector(num)).Median();
}
```

これで、任意の型の一連のオブジェクトに対して `Median` メソッドを呼び出すことができます。 型に固有のメソッド オーバーロードがない場合は、デリゲート パラメーターを渡す必要があります。 この目的で、C# ではラムダ式を使用できます。 また、Visual Basic に限り、メソッド呼び出しの代わりに `Aggregate` 句または `Group By` 句を使用した場合、その句のスコープにある任意の値または式を渡すことができます。

次のコード例では、整数の配列と文字列の配列に対して `Median` メソッドを呼び出す方法を示します。 文字列の場合は、配列に格納されている文字列の長さの中央値が計算されます。 この例は、それぞれのケースについて、<xref:System.Func%602> メソッドに `Median` デリゲート パラメーターを渡す方法を示しています。

```csharp
int[] numbers3 = { 1, 2, 3, 4, 5 };

/*
  You can use the num=>num lambda expression as a parameter for the Median method
  so that the compiler will implicitly convert its value to double.
  If there is no implicit conversion, the compiler will display an error message.
*/

var query3 = numbers3.Median(num => num);

Console.WriteLine("int: Median = " + query3);

string[] numbers4 = { "one", "two", "three", "four", "five" };

// With the generic overload, you can also use numeric properties of objects.

var query4 = numbers4.Median(str => str.Length);

Console.WriteLine("String: Median = " + query4);

/*
 This code produces the following output:

 Integer: Median = 3
 String: Median = 4
*/
```

## <a name="adding-a-method-that-returns-a-collection"></a>コレクションを返すメソッドを追加する

<xref:System.Collections.Generic.IEnumerable%601> インターフェイスは、一連の値を返すカスタム クエリ メソッドを追加することで拡張できます。 その場合、メソッドで型 <xref:System.Collections.Generic.IEnumerable%601> のコレクションを返す必要があります。 このようなメソッドを使用すると、一連の値にフィルターまたはデータ変換を適用することができます。

次の例では、コレクション内の最初の要素から 1 つおきに要素を返す `AlternateElements` という名前の拡張メソッドを作成する方法を示しています。

```csharp
// Extension method for the IEnumerable<T> interface.
// The method returns every other element of a sequence.

public static IEnumerable<T> AlternateElements<T>(this IEnumerable<T> source)
{
    List<T> list = new List<T>();

    int i = 0;

    foreach (var element in source)
    {
        if (i % 2 == 0)
        {
            list.Add(element);
        }

        i++;
    }

    return list;
}
```

この拡張メソッドは、次のコードに示すとおり、<xref:System.Collections.Generic.IEnumerable%601> インターフェイスにある他のメソッドを呼び出すときと同じように、列挙可能な任意のコレクションに対して呼び出すことができます。

```csharp
string[] strings = { "a", "b", "c", "d", "e" };

var query = strings.AlternateElements();

foreach (var element in query)
{
    Console.WriteLine(element);
}
/*
 This code produces the following output:

 a
 c
 e
*/
```

## <a name="see-also"></a>参照

- <xref:System.Collections.Generic.IEnumerable%601>
- [拡張メソッド](../../classes-and-structs/extension-methods.md)
