---
title: 反復子
description: 組み込み C# の反復子を使用して、独自のカスタム反復子メソッドを作成する方法について説明します。
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: 5cf36f45-f91a-4fca-a0b7-87f233e108e9
ms.openlocfilehash: 1933ecf83e9fa234f9b88c815d8ab527997c97f2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398725"
---
# <a name="iterators"></a>反復子

プログラムを記述するうえで、ほとんどのプログラムに必要になるのがコレクションの反復処理です。 反復処理が必要な場合は、コレクション内のすべての項目を調べるコードを記述します。

また、クラスの要素に対して反復子を生成するメソッドである、反復子メソッドも作成することになります。 反復子メソッドは以下のような目的に使用できます。

+ コレクション内の各項目に対するアクションの実行。
+ カスタム コレクションの列挙。
+ [LINQ](linq/index.md) やその他のライブラリの拡張。
+ 反復子メソッドによってデータ フローを効率化するデータ パイプラインの作成。

C# 言語には、上記の両方のシナリオに対応するための機能が用意されています。 この記事では、それらの機能の概要について説明します。

このチュートリアルには、複数の手順があります。 各手順の後に、アプリケーションを実行して進行状況を確認できます。 このトピックの[完全なサンプルを表示またはダウンロードする](https://github.com/dotnet/samples/blob/master/csharp/iterators)こともできます。 ダウンロード方法については、「[サンプルおよびチュートリアル](../samples-and-tutorials/index.md#viewing-and-downloading-samples)」を参照してください。

## <a name="iterating-with-foreach"></a>foreach を使用した反復処理

コレクションの列挙方法は単純です。`foreach` キーワードによってコレクション内の要素ごとに埋め込みステートメントを 1 回実行し、コレクションを列挙します。

```csharp
foreach (var item in collection)
{
   Console.WriteLine(item.ToString());
}
```

これで完成です。 `foreach` ステートメントさえあれば、コレクションに含まれるすべての内容を反復処理できます。 ただし、`foreach` ステートメントは魔法ではありません。 コレクションの反復処理に必要なコードを生成するためには、.NET コア ライブラリに定義されている 2 つのジェネリック インターフェイス、`IEnumerable<T>` と `IEnumerator<T>` が不可欠です。 このメカニズムについては、後ほど詳しく説明します。

これら 2 つのインターフェイスに対応する非ジェネリック インターフェイスとして、`IEnumerable` と `IEnumerator` があります。 最新のコード向けには[ジェネリック](programming-guide/generics/index.md) バージョンが適しています。

## <a name="enumeration-sources-with-iterator-methods"></a>反復子メソッドを使用した列挙型のソース

C# 言語のもう 1 つの優れた機能を利用することで、列挙型用のソースを作成するメソッドを構築できます。 このようなメソッドを、"*反復子メソッド*" と呼びます。 反復子メソッドでは、要求があった場合にオブジェクトがどのようなシーケンスで生成されるかを定義します。 反復子メソッドを定義するには、`yield return` コンテキスト キーワードを使用します。

次のメソッドを記述することで、0 ～ 9 の整数からなるシーケンスを生成できます。

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    yield return 0;
    yield return 1;
    yield return 2;
    yield return 3;
    yield return 4;
    yield return 5;
    yield return 6;
    yield return 7;
    yield return 8;
    yield return 9;
}
```

上記のコードでは、複数の `yield return` ステートメントを反復子メソッド内で個別に使用できるという点を強調するために、さまざまな `yield return` ステートメントを示しています。
反復子メソッドのコードを簡略化するために、他の言語構成要素を使用することができます (実際、頻繁に使用します)。 次のメソッド定義では、まったく同じ数値のシーケンスが生成されます。

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    int index = 0;
    while (index < 10)
        yield return index++;
}
```

どちらか一方に決める必要はありません。 メソッドのニーズに合わせて必要な数だけ `yield return` ステートメントを使用できます。

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    int index = 0;
    while (index < 10)
        yield return index++;

    yield return 50;

    index = 100;
    while (index < 110)
        yield return index++;
}
```

これが基本的な構文です。 反復子メソッドを記述することになるであろう実際の例について考えてみましょう。 自分が IoT プロジェクトに参加しているとして、デバイス センサーから膨大な量のデータ ストリームが生成されている状況を想像してください。 データをおおまかに把握するためには、N 番目ごとにデータ要素をサンプリングするメソッドを記述することになります。 このような処理は、次の小さな反復子メソッドで実現できます。

```csharp
public static IEnumerable<T> Sample(this IEnumerable<T> sourceSequence, int interval)
{
    int index = 0;
    foreach (T item in sourceSequence)
    {
        if (index++ % interval == 0)
            yield return item;
    }
}
```

反復子メソッドには重要な制限事項が 1 つあり、`return` ステートメントと `yield return` ステートメントの両方を同じメソッド内で使用することはできません。 そのため、次のコードはコンパイルされません。

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    int index = 0;
    while (index < 10)
        yield return index++;

    yield return 50;

    // generates a compile time error:
    var items = new int[] {100, 101, 102, 103, 104, 105, 106, 107, 108, 109 };
    return items;
}
```

通常は、この制限が問題になることはありません。 メソッド全体で `yield return` を使用するか、元のメソッドを複数に分割して一部のメソッドでは `return`、一部のメソッドでは `yield return` を使用するか、いずれかの方法を選択できます。

前のメソッドを少し修正すると、メソッド全体で `yield return` のみを使用するように変更できます。

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    int index = 0;
    while (index < 10)
        yield return index++;

    yield return 50;

    var items = new int[] {100, 101, 102, 103, 104, 105, 106, 107, 108, 109 };
    foreach (var item in items)
        yield return item;
}
```

反復子メソッドを 2 つの異なるメソッドに分割することが正解となる場合もあります。 つまり、`return` を使用するメソッドと `yield return` を使用するメソッドの 2 つです。 ブール型の引数を基に、空のコレクションまたは最初の 5 つの奇数を返す必要があるような場合を考えてみてください。 この処理は、次の 2 つのメソッドとして記述できます。

```csharp
public IEnumerable<int> GetSingleDigitOddNumbers(bool getCollection)
{
    if (getCollection == false)
        return new int[0];
    else
        return IteratorMethod();
}

private IEnumerable<int> IteratorMethod()
{
    int index = 0;
    while (index < 10)
    {
        if (index % 2 == 1)
            yield return index;
        index++;
    }
}
```

上記のメソッドを見てみましょう。 1 つ目のメソッドでは、標準の `return` ステートメントを使用して空のコレクションまたは 2 つ目のメソッドで作成された反復子のいずれかを返します。 2 つ目のメソッドでは、`yield return` ステートメントを使用して要求されたシーケンスを作成します。

## <a name="deeper-dive-into-foreach"></a>`foreach` の詳細

`foreach` ステートメントは、`IEnumerable<T>` および `IEnumerator<T>` インターフェイスを使用してコレクションの全要素を反復処理する標準的な表現形式に展開されます。 また、開発者の不適切なリソース管理によって生じるエラーを最小化する効果もあります。

最初の例に登場する `foreach` ループは、コンパイラによって次のコンストラクトに似たコードに変換されます。

```csharp
IEnumerator<int> enumerator = collection.GetEnumerator();
while (enumerator.MoveNext())
{
    var item = enumerator.Current;
    Console.WriteLine(item.ToString());
}
```

上記のコンストラクトは、バージョン 5 以降の C# コンパイラによって生成されるコードを表しています。 バージョン 5 より前のバージョンでは、`item` 変数のスコープが異なります。

```csharp
// C# versions 1 through 4:
IEnumerator<int> enumerator = collection.GetEnumerator();
int item = default(int);
while (enumerator.MoveNext())
{
    item = enumerator.Current;
    Console.WriteLine(item.ToString());
}
```

この点が変更された理由は、以前の動作に、ラムダ式に関連する微妙なバグや診断の難しいバグを発生させる可能性があったためです。 ラムダ式について詳しくは、「[ラムダ式](./programming-guide/statements-expressions-operators/lambda-expressions.md)」をご覧ください。

コンパイラによって実際に生成されるコードはもう少し複雑であり、`GetEnumerator()` から返されるオブジェクトで `IDisposable` インターフェイスを実装する場合の処理も含まれています。 全展開によって生成されるコードは、次のようになります。

```csharp
{
    var enumerator = collection.GetEnumerator();
    try
    {
        while (enumerator.MoveNext())
        {
            var item = enumerator.Current;
            Console.WriteLine(item.ToString());
        }
    } finally
    {
        // dispose of enumerator.
    }
}
```

列挙子が破棄される場合、その方法は `enumerator` の型の特性によって異なります。 一般的なケースでは、`finally` 句は次のように展開されます。

```csharp
finally
{
   (enumerator as IDisposable)?.Dispose();
}
```

ただし、`enumerator` の型がシール型で、`enumerator` から `IDisposable` への暗黙的な型変換がない場合、`finally` 句は空のブロックに展開されます。

```csharp
finally
{
}
```

`enumerator` から `IDisposable` への暗黙的な型変換があり、`enumerator` が null 非許容の値型である場合、`finally` 句は次のように展開されます。

```csharp
finally
{
   ((IDisposable)enumerator).Dispose();
}
```

さいわいなことに、これらの詳細をすべて覚えておく必要はありません。 このような微妙な差異は、いずれも `foreach` ステートメントによって処理されます。 コンパイラでは、これらすべてのコンストラクトに対して正しいコードが生成されます。
