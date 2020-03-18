---
title: C# でのコレクションの反復処理
ms.date: 08/14/2018
ms.assetid: c93f6dd4-e72a-4a06-be1c-a98b3255b734
ms.openlocfilehash: aceedd11466c75cedad3c67224c3a5595b4cabfa
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77626271"
---
# <a name="iterators-c"></a>反復子 (C#)

*反復子*を使用して、リストや配列などのコレクションをステップ実行することができます。

iterator メソッドまたは `get` アクセサーは、コレクションに対するカスタム イテレーションを実行します。 反復子メソッドは、[yield return](../../language-reference/keywords/yield.md) ステートメントを使用して、各要素を 1 回に 1 つ返します。 `yield return` ステートメントに達すると、コードの現在の場所が記憶されます。 次回、iterator 関数が呼び出されると、この位置から実行が再開されます。

[foreach](../../language-reference/keywords/foreach-in.md) ステートメントまたは LINQ クエリを使用して、クライアント コードから反復子を呼び出します。

次の例では、`foreach` ループの最初の反復子により、最初の `yield return` ステートメントに達するまで `SomeNumbers` iterator メソッドで実行が続行されます。 このイテレーションは 3 の値を返し、iterator メソッドの現在の場所が保持されます。 ループの次のイテレーションでは、iterator メソッドの実行が中断した場所から続行し、`yield return` ステートメントに達したときに再度停止します。 このイテレーションは 5 の値を返し、ここでも iterator メソッドの現在の場所が保持されます。 iterator メソッドの最後に達すると、ループが完了します。

```csharp
static void Main()
{
    foreach (int number in SomeNumbers())
    {
        Console.Write(number.ToString() + " ");
    }
    // Output: 3 5 8
    Console.ReadKey();
}

public static System.Collections.IEnumerable SomeNumbers()
{
    yield return 3;
    yield return 5;
    yield return 8;
}
```

Iterator メソッドまたは `get` アクセサーの戻り値の型は、<xref:System.Collections.IEnumerable>、<xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Collections.IEnumerator>、または <xref:System.Collections.Generic.IEnumerator%601> となります。

`yield break` ステートメントを使用すると、反復を終了できます。

> [!NOTE]
> このトピックの単純な反復子の例を除くすべての例には、`System.Collections` および `System.Collections.Generic` 名前空間の [using](../../language-reference/keywords/using-directive.md) ディレクティブが含まれています。

## <a name="simple-iterator"></a>単純な反復子

次の例では、[for](../../language-reference/keywords/for.md) ループ内に 1 つの `yield return` ステートメントが含まれます。 `Main` では、`foreach` ステートメント本文の各イテレーションで iterator 関数が呼び出され、これが次の `yield return` ステートメントに続行されます。

```csharp
static void Main()
{
    foreach (int number in EvenSequence(5, 18))
    {
        Console.Write(number.ToString() + " ");
    }
    // Output: 6 8 10 12 14 16 18
    Console.ReadKey();
}

public static System.Collections.Generic.IEnumerable<int>
    EvenSequence(int firstNumber, int lastNumber)
{
    // Yield even numbers in the range.
    for (int number = firstNumber; number <= lastNumber; number++)
    {
        if (number % 2 == 0)
        {
            yield return number;
        }
    }
}
```

## <a name="creating-a-collection-class"></a>コレクション クラスを作成する

次の例の `DaysOfTheWeek` クラスは、<xref:System.Collections.IEnumerable.GetEnumerator%2A> メソッドを必要とする <xref:System.Collections.IEnumerable> インターフェイスを実装します。 コンパイラは、<xref:System.Collections.IEnumerator> を返す `GetEnumerator` メソッドを暗黙的に呼び出します。

`GetEnumerator` メソッドは、`yield return` ステートメントを使用して、各文字列を一度に 1 つ返します。

```csharp
static void Main()
{
    DaysOfTheWeek days = new DaysOfTheWeek();

    foreach (string day in days)
    {
        Console.Write(day + " ");
    }
    // Output: Sun Mon Tue Wed Thu Fri Sat
    Console.ReadKey();
}

public class DaysOfTheWeek : IEnumerable
{
    private string[] days = { "Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };

    public IEnumerator GetEnumerator()
    {
        for (int index = 0; index < days.Length; index++)
        {
            // Yield each day of the week.
            yield return days[index];
        }
    }
}
```

次の例では、動物のコレクションを含む `Zoo` クラスを作成します。

クラス インスタンス (`theZoo`) を参照する `foreach` ステートメントでは、`GetEnumerator` メソッドが暗黙的に呼び出されます。 `Birds` および `Mammals` プロパティを参照する `foreach` ステートメントでは、`AnimalsForType` という名前の iterator メソッドが使用されます。

```csharp
static void Main()
{
    Zoo theZoo = new Zoo();

    theZoo.AddMammal("Whale");
    theZoo.AddMammal("Rhinoceros");
    theZoo.AddBird("Penguin");
    theZoo.AddBird("Warbler");

    foreach (string name in theZoo)
    {
        Console.Write(name + " ");
    }
    Console.WriteLine();
    // Output: Whale Rhinoceros Penguin Warbler

    foreach (string name in theZoo.Birds)
    {
        Console.Write(name + " ");
    }
    Console.WriteLine();
    // Output: Penguin Warbler

    foreach (string name in theZoo.Mammals)
    {
        Console.Write(name + " ");
    }
    Console.WriteLine();
    // Output: Whale Rhinoceros

    Console.ReadKey();
}

public class Zoo : IEnumerable
{
    // Private members.
    private List<Animal> animals = new List<Animal>();

    // Public methods.
    public void AddMammal(string name)
    {
        animals.Add(new Animal { Name = name, Type = Animal.TypeEnum.Mammal });
    }

    public void AddBird(string name)
    {
        animals.Add(new Animal { Name = name, Type = Animal.TypeEnum.Bird });
    }

    public IEnumerator GetEnumerator()
    {
        foreach (Animal theAnimal in animals)
        {
            yield return theAnimal.Name;
        }
    }

    // Public members.
    public IEnumerable Mammals
    {
        get { return AnimalsForType(Animal.TypeEnum.Mammal); }
    }

    public IEnumerable Birds
    {
        get { return AnimalsForType(Animal.TypeEnum.Bird); }
    }

    // Private methods.
    private IEnumerable AnimalsForType(Animal.TypeEnum type)
    {
        foreach (Animal theAnimal in animals)
        {
            if (theAnimal.Type == type)
            {
                yield return theAnimal.Name;
            }
        }
    }

    // Private class.
    private class Animal
    {
        public enum TypeEnum { Bird, Mammal }

        public string Name { get; set; }
        public TypeEnum Type { get; set; }
    }
}
```

## <a name="using-iterators-with-a-generic-list"></a>ジェネリック リストと共に反復子を使用する

次の例の <xref:System.Collections.Generic.Stack%601> ジェネリック クラスは、<xref:System.Collections.Generic.IEnumerable%601> ジェネリック インターフェイスを実装しています。 <xref:System.Collections.Generic.Stack%601.Push%2A> メソッドでは、`T` 型の配列に値を割り当てます。 <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> メソッドは、`yield return` ステートメントを使って配列値を返します。

ジェネリック メソッド <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> だけでなく、非ジェネリック メソッド <xref:System.Collections.IEnumerable.GetEnumerator%2A> も実装する必要があります。 これは、<xref:System.Collections.Generic.IEnumerable%601> が <xref:System.Collections.IEnumerable> から継承するためです。 非ジェネリック実装は、ジェネリック実装に従います。

例では名前付き反復子を使用して、同じデータ コレクションでのさまざまな反復処理をサポートします。 この場合の名前付き反復子は、`TopToBottom` プロパティと `BottomToTop` プロパティ、および `TopN` メソッドです。

`BottomToTop` プロパティは `get` アクセサーで反復子を使用します。

```csharp
static void Main()
{
    Stack<int> theStack = new Stack<int>();

    //  Add items to the stack.
    for (int number = 0; number <= 9; number++)
    {
        theStack.Push(number);
    }

    // Retrieve items from the stack.
    // foreach is allowed because theStack implements IEnumerable<int>.
    foreach (int number in theStack)
    {
        Console.Write("{0} ", number);
    }
    Console.WriteLine();
    // Output: 9 8 7 6 5 4 3 2 1 0

    // foreach is allowed, because theStack.TopToBottom returns IEnumerable(Of Integer).
    foreach (int number in theStack.TopToBottom)
    {
        Console.Write("{0} ", number);
    }
    Console.WriteLine();
    // Output: 9 8 7 6 5 4 3 2 1 0

    foreach (int number in theStack.BottomToTop)
    {
        Console.Write("{0} ", number);
    }
    Console.WriteLine();
    // Output: 0 1 2 3 4 5 6 7 8 9

    foreach (int number in theStack.TopN(7))
    {
        Console.Write("{0} ", number);
    }
    Console.WriteLine();
    // Output: 9 8 7 6 5 4 3

    Console.ReadKey();
}

public class Stack<T> : IEnumerable<T>
{
    private T[] values = new T[100];
    private int top = 0;

    public void Push(T t)
    {
        values[top] = t;
        top++;
    }
    public T Pop()
    {
        top--;
        return values[top];
    }

    // This method implements the GetEnumerator method. It allows
    // an instance of the class to be used in a foreach statement.
    public IEnumerator<T> GetEnumerator()
    {
        for (int index = top - 1; index >= 0; index--)
        {
            yield return values[index];
        }
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }

    public IEnumerable<T> TopToBottom
    {
        get { return this; }
    }

    public IEnumerable<T> BottomToTop
    {
        get
        {
            for (int index = 0; index <= top - 1; index++)
            {
                yield return values[index];
            }
        }
    }

    public IEnumerable<T> TopN(int itemsFromTop)
    {
        // Return less than itemsFromTop if necessary.
        int startIndex = itemsFromTop >= top ? 0 : top - itemsFromTop;

        for (int index = top - 1; index >= startIndex; index--)
        {
            yield return values[index];
        }
    }

}
```

## <a name="syntax-information"></a>構文情報

反復子は、メソッドまたは `get` アクセサーとして指定できます。 反復子を、イベント、インスタンス コンストラクター、静的コンストラクター、静的ファイナライザーで指定することはできません。

`yield return` ステートメントの式の型を、反復子によって返される `IEnumerable<T>` の型引数に暗黙的に変換する必要があります。

C# の場合、反復子メソッドで `in`、`ref`、`out` パラメーターを指定することはできません。

C# の場合、`yield` は予約語ではなく、`return` または `break` キーワードの前に使用される場合にのみ、特別な意味を持ちます。

## <a name="technical-implementation"></a>技術的な実装

メソッドとして反復子を記述しても、コンパイラが入れ子のクラス (つまり、事実上、ステート マシン) に変換します。 このクラスは、クライアント コードで `foreach` ループが続く限り、反復子の位置を追跡します。

コンパイラの動作を確認するには、Ildasm.exe ツールを使用して、iterator メソッドに対して生成される Microsoft 中間言語コードを表示します。

[クラス](../../language-reference/keywords/class.md)または[構造体](../../language-reference/builtin-types/struct.md)用の反復子を作成する場合、<xref:System.Collections.IEnumerator> インターフェイス全体を実装する必要はありません。 コンパイラは、反復子を検出すると、<xref:System.Collections.IEnumerator> または <xref:System.Collections.Generic.IEnumerator%601> インターフェイスの `Current`、`MoveNext`、および `Dispose` メソッドを自動的に生成します。

`foreach` ループの連続する反復ごとに (または `IEnumerator.MoveNext` を直接呼び出すと)、前の `yield return` ステートメントの後で次の反復子コード本体が再開されます。 その後、反復子本体の最後に到達するか、`yield break` ステートメントが検出されるまで、次の `yield return` ステートメントに続行されます。

反復子は、<xref:System.Collections.IEnumerator.Reset%2A?displayProperty=nameWithType> メソッドをサポートしません。 反復処理を最初から再度行う場合は、新しい反復子を取得する必要があります。 反復子メソッドによって返される反復子に対して <xref:System.Collections.IEnumerator.Reset%2A> を呼び出すと、<xref:System.NotSupportedException> がスローされます。

詳細については、「[C# 言語の仕様](~/_csharplang/spec/classes.md#iterators)」を参照してください。

## <a name="use-of-iterators"></a>反復子の使用

反復子を使用すると、複雑なコードを使用して一覧シーケンスを設定する必要がある場合に、`foreach` ループの単純さを維持することができます。 これは次のような場合に役立ちます。

- 最初の `foreach` ループ イテレーションの後に一覧シーケンスを変更する。

- 最初の `foreach` ループ イテレーションの前に大きい一覧が完全に読み込まれないようにする。 例として、ページ フェッチでのテーブル行のバッチの読み込みなどがあります。 また、別の例として、<xref:System.IO.DirectoryInfo.EnumerateFiles%2A> メソッドでの .NET Framework 内の反復子の実装があります。

- 反復子に一覧の作成をカプセル化する。 iterator メソッドでは、一覧を作成してから、ループで各結果を生成することができます。

## <a name="see-also"></a>関連項目

- <xref:System.Collections.Generic>
- <xref:System.Collections.Generic.IEnumerable%601>
- [foreach、in](../../language-reference/keywords/foreach-in.md)
- [yield](../../language-reference/keywords/yield.md)
- [配列での foreach の使用](../arrays/using-foreach-with-arrays.md)
- [ジェネリック](../generics/index.md)
