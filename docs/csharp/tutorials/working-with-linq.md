---
title: LINQ の使用
description: このチュートリアルでは、LINQ を使用してシーケンスを生成し、LINQ クエリで使用するためのメソッドを作成し、先行評価と遅延評価を区別する方法を説明します。
ms.date: 10/29/2018
ms.technology: csharp-linq
ms.assetid: 0db12548-82cb-4903-ac88-13103d70aa77
ms.openlocfilehash: ece001e82c0aa44a91999bea78d2fd695ff9362b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78240016"
---
# <a name="work-with-language-integrated-query-linq"></a>統合言語クエリ (LINQ) を使用する

## <a name="introduction"></a>はじめに

このチュートリアルでは、.NET Core と C# 言語の機能を説明します。 以下の方法について説明します。

- LINQ を使用してシーケンスを生成する。
- LINQ クエリで簡単に使用できるメソッドを記述する。
- 先行評価と遅延評価を区別する。

これらの方法を、マジシャンの基本的スキルの 1 つである[ファロ― シャッフル](https://en.wikipedia.org/wiki/Faro_shuffle)を再現するアプリケーションを作成しながら学習していきます。 ファロ― シャッフルとは、簡単に言うと、カード デッキを正確に 2 等分し、双方のデッキから 1 枚ずつ交互に並ぶようにシャッフルして、元のデッキを並べ替えることです。

マジシャンがこの手法を使用するのは、シャッフルした後もそれぞれのカードの位置がわかり、カードの順序が繰り返しのパターンになるからです。

ここでは、データ シーケンスの操作として気軽に見ていきましょう。 これから作成するアプリケーションでは、カード デッキを構築し、シャッフルのシーケンスを実行し、その都度シーケンスを書き込みます。 また、更新された順序を元の順序と比較します。

このチュートリアルには、複数の手順があります。 各手順の後に、アプリケーションを実行して進行状況を確認できます。 GitHub の dotnet/samples リポジトリでは、[完全版のサンプル](https://github.com/dotnet/samples/blob/master/csharp/getting-started/console-linq)を確認することもできます。 ダウンロード方法については、「[サンプルおよびチュートリアル](../../samples-and-tutorials/index.md#viewing-and-downloading-samples)」を参照してください。

## <a name="prerequisites"></a>必須コンポーネント

お使いのコンピューターを、.NET Core が実行されるように設定する必要があります。 インストールの手順については、[.NET Core のダウンロード](https://dotnet.microsoft.com/download) ページを参照してください。 このアプリケーションは、Windows、Ubuntu Linux、または OS X 上、あるいは Docker コンテナーで実行できます。 お好みのコード エディターをインストールしてください。 次の説明では、オープン ソースのクロス プラットフォーム エディターである [Visual Studio Code](https://code.visualstudio.com/) を使用しています。 しかし、他の使い慣れたツールを使用しても構いません。

## <a name="create-the-application"></a>アプリケーションを作成する

最初に新しいアプリケーションを作成します。 コマンド プロンプトを開き、アプリケーション用の新しいディレクトリを作成します。 それを、現在のディレクトリとしてください。 コマンド プロンプトで `dotnet new console` のコマンドを入力します。 これで、基本的な "Hello World" アプリケーションのスターター ファイルが作成されます。

C# を始めて使用する方向けに、[このチュートリアル](console-teleprompter.md)で C# プログラムの構造について説明しています。 そのチュートリアルを先に読んでから、ここに戻って LINQ の詳細について学ぶのも良いでしょう。

## <a name="create-the-data-set"></a>データ セットを作成する

開始する前に、`dotnet new console` によって生成された `Program.cs` ファイルの上部に次の行があることを確認してください。

```csharp
// Program.cs
using System;
using System.Collections.Generic;
using System.Linq;
```

これら 3 つの行 (`using` ステートメント) がファイルの上部にない場合、プログラムはコンパイルされません。

必要になる参照のすべてが用意できたので、カード デッキを構成するものは何かを考えます。 一般的に、1 組のトランプ カードには 4 種類の絵札 (スート) があり、スートごとに 13 個の値があります。 通常は、`Card` クラスをすぐに作成し、`Card` オブジェクトのコレクションを手動で設定することを検討するかもしれません。 LINQ では、カード デッキの作成を通常の方法よりも簡潔に処理することができます。 `Card` クラスを作成する代わりに、それぞれがスートとランクを表す 2 つのシーケンスを作成できます。 文字列の <xref:System.Collections.Generic.IEnumerable%601> としてランクとスートを生成する、非常に単純な "[*反復子メソッド*](../iterators.md#enumeration-sources-with-iterator-methods)" のペアを作成します。

```csharp
// Program.cs
// The Main() method

static IEnumerable<string> Suits()
{
    yield return "clubs";
    yield return "diamonds";
    yield return "hearts";
    yield return "spades";
}

static IEnumerable<string> Ranks()
{
    yield return "two";
    yield return "three";
    yield return "four";
    yield return "five";
    yield return "six";
    yield return "seven";
    yield return "eight";
    yield return "nine";
    yield return "ten";
    yield return "jack";
    yield return "queen";
    yield return "king";
    yield return "ace";
}
```

`Program.cs` ファイル内の `Main` メソッドの下にこれらを配置します。 これら 2 つのメソッドは両方とも、`yield return` 構文を使用して実行中にシーケンスを生成します。 コンパイラは <xref:System.Collections.Generic.IEnumerable%601> を実装するオブジェクトをビルドし、要求に応じて文字列のシーケンスを生成します。

次に、これらの反復子メソッドを使用して、カード デッキを作成します。 `Main` メソッドの中に LINQ クエリを配置します。 次のようになります。

```csharp
// Program.cs
static void Main(string[] args)
{
    var startingDeck = from s in Suits()
                       from r in Ranks()
                       select new { Suit = s, Rank = r };

    // Display each card that we've generated and placed in startingDeck in the console
    foreach (var card in startingDeck)
    {
        Console.WriteLine(card);
    }
}
```

複数の `from` 句は 1 つの <xref:System.Linq.Enumerable.SelectMany%2A> を生成し、これが、最初のシーケンス内の各要素と 2 番目のシーケンス内の各要素とを組み合わせた 1 つのシーケンスを作成します。 ここでは順序が重要です。 最初のソース シーケンス (Suits) 内の最初の要素が、2 番目のシーケンス (Ranks) のすべての要素と組み合わされます。 これで、最初のスートのカード 13 枚すべてが生成されます。 このプロセスを、最初のシーケンス (Suits) 内の各要素について繰り返します。 その結果、はじめにスート順に並んで、次に値の順に並んだカード デッキができます。

LINQ を記述するときに、上記に示したクエリ構文を使用することを選択した場合でも、メソッド構文を使用することを選択した場合でも、片方の形式の構文から他方の構文の形式に常に移動できることを覚えておくことが重要です。 クエリ構文で記述された上記のクエリは、次のようにメソッド構文で記述できます。

```csharp
var startingDeck = Suits().SelectMany(suit => Ranks().Select(rank => new { Suit = suit, Rank = rank }));
```

クエリ構文で記述された LINQ ステートメントは、コンパイラによって、同等のメソッド呼び出し構文に変換されます。 そのため、選択した構文に関係なく、クエリの 2 つのバージョンでは同じ結果が生成されます。 ご自分の状況にとって最善の構文を選択してください。たとえば、チームで作業している場合に、メソッド構文に慣れていないメンバーがいる場合は、クエリ構文の使用を勧めるようにしてください。

この時点で、作成したサンプルを実行してみてください。 デッキにある 52 枚のカードがすべて表示されます。 デバッガ―でこのサンプルを実行すると、`Suits()` メソッドと `Ranks()` メソッドがどのように実行されるか理解するのに役立ちます。 各シーケンス内の各文字列が必要な場合にのみ生成されることがよくわかります。

![アプリが 52 枚のカードを書き出しているコンソール ウィンドウ](./media/working-with-linq/console-52-card-application.png)

## <a name="manipulate-the-order"></a>順序を操作する

次に、カード デッキをどのようにシャッフルするかに注目します。 適切なシャッフルの最初の手順は、カード デッキを 2 つの山に分けることです。 LINQ API の一部である <xref:System.Linq.Enumerable.Take%2A> メソッドと <xref:System.Linq.Enumerable.Skip%2A> メソッドの機能を利用できます。 それらを `foreach` ループの下に配置します。

```csharp
// Program.cs
public static void Main(string[] args)
{
    var startingDeck = from s in Suits()
                       from r in Ranks()
                       select new { Suit = s, Rank = r };

    foreach (var c in startingDeck)
    {
        Console.WriteLine(c);
    }

    // 52 cards in a deck, so 52 / 2 = 26
    var top = startingDeck.Take(26);
    var bottom = startingDeck.Skip(26);
}
```

ただし、標準ライブラリの中には利用できるシャッフル メソッドがないので、自分で作成する必要があります。 作成するシャッフル メソッドには、LINQ ベースのプログラムで使用できるさまざまなテクニックが例示されているので、このプロセスの各部分を手順を追って説明します。

LINQ クエリから返される <xref:System.Collections.Generic.IEnumerable%601> を操作する方法に対していくつかの機能を追加するには、[拡張メソッド](../programming-guide/classes-and-structs/extension-methods.md)と呼ばれる特別な種類のメソッドをいくつか記述する必要があります。 手短に言うと、拡張メソッドとは、既に存在する型に機能を追加する際に、元の型を変更せずに新しい機能を追加するという特別な目的を持つ*静的メソッド*です。

`Extensions.cs` という名前の新しい *static* クラス ファイルをプログラムに追加した後、最初の拡張メソッドをビルドすることで、自分の拡張メソッドに新しいホームを与えます。

```csharp
// Extensions.cs
using System;
using System.Collections.Generic;
using System.Linq;

namespace LinqFaroShuffle
{
    public static class Extensions
    {
        public static IEnumerable<T> InterleaveSequenceWith<T>(this IEnumerable<T> first, IEnumerable<T> second)
        {
            // Your implementation will go here soon enough
        }
    }
}
```

少しの間、メソッド シグネチャ、特にパラメーターを調べてみてください。

```csharp
public static IEnumerable<T> InterleaveSequenceWith<T> (this IEnumerable<T> first, IEnumerable<T> second)
```

メソッドの最初の引数に `this` 修飾子が追加されているのがわかります。 つまり、最初の引数の型のメンバー メソッドと同じように、メソッドを呼び出すということです。 また、このメソッドの宣言は標準的な表現形式に従い、入力と出力の型が `IEnumerable<T>` となります。 これによって LINQ メソッドが連結され、より複雑なクエリを実行できるようになります。

当然ながら、デッキを二等分したため、これらを結合させる必要があります。 コードでは、これは、<xref:System.Linq.Enumerable.Take%2A> と <xref:System.Linq.Enumerable.Skip%2A> を同時に実行し、要素を *`interleaving`* して取得したシーケンスの両方を列挙し、1 つのシーケンス (今シャッフルが行なわれたカード デッキ) を作成することを意味します。 2 つのシーケンスで動作する LINQ メソッドを作成するには、<xref:System.Collections.Generic.IEnumerable%601> がどのように動作するかを理解する必要があります。

<xref:System.Collections.Generic.IEnumerable%601> インターフェイスには <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> のメソッドが 1 つあります。 <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> によって返されるオブジェクトには、次の要素に移動するメソッドと、シーケンス内の現在の要素を取得するプロパティがあります。 これら 2 つのメンバーを使用して、コレクションを列挙し、要素を返します。 このインターリーブ メソッドは反復子メソッドになるので、コレクションを作成してそのコレクションを返す代わりに、上記の `yield return` 構文を使用します。

そのメソッドの実装を以下に示します。

[!CODE-csharp[InterleaveSequenceWith](../../../samples/snippets/csharp/getting-started/console-linq/extensions.cs?name=snippet1)]

このメソッドが作成できたので、`Main` メソッドに戻り、デッキを 1 回シャッフルします。

```csharp
// Program.cs
public static void Main(string[] args)
{
    var startingDeck = from s in Suits()
                       from r in Ranks()
                       select new { Suit = s, Rank = r };

    foreach (var c in startingDeck)
    {
        Console.WriteLine(c);
    }

    var top = startingDeck.Take(26);
    var bottom = startingDeck.Skip(26);
    var shuffle = top.InterleaveSequenceWith(bottom);

    foreach (var c in shuffle)
    {
        Console.WriteLine(c);
    }
}
```

## <a name="comparisons"></a>比較

デッキを元の順序に戻すまでに何回シャッフルが必要でしょうか。 これを見つけるには、2 つのシーケンスが等しいかどうかを判断するメソッドを記述する必要があります。 そのメソッドを記述したら、デッキをループでシャッフルするコードを配置し、どの時点で元の順序に戻るかを確認する必要があります。

2 つのシーケンスが等しいかどうかを判断するメソッドの記述は簡単です。 デッキのシャッフル用に記述したメソッドと似た構造です。 今回に限り、各要素を `yield return` するのではなく、各シーケンスの一致する要素を比較します。 シーケンス全体が列挙されている場合、各要素が一致すれば、シーケンスは同じです。

[!CODE-csharp[SequenceEquals](../../../samples/snippets/csharp/getting-started/console-linq/extensions.cs?name=snippet2)]

これは 2 つ目の LINQ の表現形式であるターミナル メソッドを示しています。 これらは、シーケンスを入力として受け取り (この場合 2 つのシーケンス)、単一のスカラー値を返します。 ターミナル メソッドを使用した場合、それらは常に LINQ クエリ用のメソッド チェーンの最後のメソッドになります。そのため、名前が "ターミナル" (終点) になっています。

これは、デッキが元の順序に戻るタイミングの判定に使用すると、どのように動作するかを確認できます。 ループ内にシャッフルのコードを配置し、`SequenceEquals()` メソッドを適用して、シーケンスが元の順序に戻った時点で停止します。 シーケンスではなく単一の値を返すため、常にクエリ内の最後のメソッドになることがわかります。

```csharp
// Program.cs
static void Main(string[] args)
{
    // Query for building the deck

    // Shuffling using InterleaveSequenceWith<T>();

    var times = 0;
    // We can re-use the shuffle variable from earlier, or you can make a new one
    shuffle = startingDeck;
    do
    {
        shuffle = shuffle.Take(26).InterleaveSequenceWith(shuffle.Skip(26));

        foreach (var card in shuffle)
        {
            Console.WriteLine(card);
        }
        Console.WriteLine();
        times++;

    } while (!startingDeck.SequenceEquals(shuffle));

    Console.WriteLine(times);
}
```

ここでコードを実行して、シャッフルごとにデッキがどのように整列するかを書き留めておいてください。 8 回のシャッフル (do-while loop の繰り返し) 後に、デッキは、開始時の LINQ クエリから作成された初回の構成に戻ります。

## <a name="optimizations"></a>最適化

ここまでで構築したサンプルは、*アウト シャッフル* (一番上と一番下のカードが毎回同じになること) を実行するものです。 ここで 1 つ変更を加えましょう。"*イン シャッフル* " を代わりに使用します。ここでは、52 枚のカードすべての位置が変わります。 イン シャッフルでは、下半分の一番上のカードが、デッキの最上部に来るように、デッキをインターウィーブします。 つまり、上半分の一番下のカードがデッキの最下部のカードになります。 これは、1 行のコードを変更する単純な変更です。 <xref:System.Linq.Enumerable.Take%2A> と <xref:System.Linq.Enumerable.Skip%2A> の位置を入れ替えることで、現在のシャッフル クエリを更新します。 これにより、デッキの上半分と下半分の順序が変更されます。

```csharp
shuffle = shuffle.Skip(26).InterleaveSequenceWith(shuffle.Take(26));
```

プログラムを再実行すると、デッキが元の順序に戻るのに反復が 52 回行われることがわかります。 プログラムを実行し続けると、著しくパフォーマンスが低下することがわかります。

これには多くの理由があります。 このパフォーマンスの低下の主な原因の 1 つを説明できます。それは、"[*遅延評価*](../programming-guide/concepts/linq/deferred-execution-and-lazy-evaluation-in-linq-to-xml.md)" の非効率的な使用です。

手短に言えば、遅延評価とは、ステートメントの値が必要になるまで、その評価が実行されないことです。 遅延して評価されているステートメントは LINQ クエリです。 シーケンスは、要素が要求された場合にのみ生成されます。 通常、これは LINQ の利点です。 しかし、このプログラムのような使い方をする場合、実行時間が急激に増加する要因となります。

元のデッキを LINQ クエリを使用して生成したことを思い出してください。 毎回のシャッフルは、直前のデッキに LINQ クエリを 3 回実行することによって生成されます。 これらはすべて遅延実行されます。 つまり、シーケンスが要求されるたびに再度実行されるということです。 52 回反復されるまでに、元のデッキを何度も何度も再生成しているのです。 ログを記述してこの動作を示しましょう。 その次に修正します。

`Extensions.cs` ファイルに、次のメソッドを入力するかコピーします。 この拡張メソッドによって、プロジェクト ディレクトリ内に `debug.log` と呼ばれる新しいファイルが作成され、このログ ファイルに現在どのようなクエリが実行されているかが記録されます。 この拡張メソッドはどんなクエリにも追加でき、そのクエリが実行されたことをマークできます。

[!CODE-csharp[LogQuery](../../../samples/snippets/csharp/getting-started/console-linq/extensions.cs?name=snippet3)]

`File` の下に、存在しないことを意味する赤い波線が表示されます。 コンパイラがどのような `File` かを把握できないため、これはコンパイルされません。 この問題を解決するには、`Extensions.cs` の最初の行の下に次のコード行を追加してください。

```csharp
using System.IO;
```

これでイシューが解決し、赤のエラーは表示されなくなるはずです。

次に、各クエリの定義をログ メッセージでインストルメントします。

```csharp
// Program.cs
public static void Main(string[] args)
{
    var startingDeck = (from s in Suits().LogQuery("Suit Generation")
                        from r in Ranks().LogQuery("Rank Generation")
                        select new { Suit = s, Rank = r }).LogQuery("Starting Deck");

    foreach (var c in startingDeck)
    {
        Console.WriteLine(c);
    }

    Console.WriteLine();
    var times = 0;
    var shuffle = startingDeck;

    do
    {
        // Out shuffle
        /*
        shuffle = shuffle.Take(26)
            .LogQuery("Top Half")
            .InterleaveSequenceWith(shuffle.Skip(26)
            .LogQuery("Bottom Half"))
            .LogQuery("Shuffle");
        */

        // In shuffle
        shuffle = shuffle.Skip(26).LogQuery("Bottom Half")
                .InterleaveSequenceWith(shuffle.Take(26).LogQuery("Top Half"))
                .LogQuery("Shuffle");

        foreach (var c in shuffle)
        {
            Console.WriteLine(c);
        }

        times++;
        Console.WriteLine(times);
    } while (!startingDeck.SequenceEquals(shuffle));

    Console.WriteLine(times);
}
```

クエリにアクセスするたびにログをとるわけではないことに注意してください。 元のクエリを作成する場合にのみログをとります。 プログラムの実行にはまだ時間がかかりますが、これで理由がわかるようになりました。 ログを有効にしたままイン シャッフルを実行すると時間がかかりすぎると感じる場合は、アウト シャッフルに戻してください。 それでも遅延評価の影響はわかります。 1 回の実行で、すべての値とスートの生成を含めてクエリを 2592 回実行します。

ここでコードのパフォーマンスを向上させて、実行回数を減らすことができます。 実行できる単純な修正は、カード デッキを構築する元の LINQ クエリの結果を "*キャッシュ*" することです。 現時点では、do-while loop が繰り返されるたびに、何度もクエリが実行され、カード デッキが再構築され、シャッフルが毎回実行されています。 カード デッキをキャッシュするには、LINQ メソッドの <xref:System.Linq.Enumerable.ToArray%2A> と <xref:System.Linq.Enumerable.ToList%2A> を活用できます。それらをクエリに追加すると、実行するように指示したのと同じアクションが実行されますが、今回は、呼び出すように選択したメソッドに応じて、結果が配列または一覧内に格納されます。 LINQ メソッド <xref:System.Linq.Enumerable.ToArray%2A> を両方のクエリに追加し、もう一度プログラムを実行します。

[!CODE-csharp[Main](../../../samples/snippets/csharp/getting-started/console-linq/Program.cs?name=snippet1)]

これで、アウト シャッフルのクエリが 30 回に減少します。 イン シャッフルで再実行しても、同様の改善がみられます。今回は 162 回のクエリが実行されます。

この例は、遅延評価がパフォーマンス問題を引き起こす可能性があるユース ケースを強調することを**意図**していることに注意してください。 遅延評価がどこでコードのパフォーマンスに影響を与える可能性があるかを確認することは重要ですが、すべてのクエリを先行評価で実行する必要があるわけではないことを理解することも同じように重要です。 <xref:System.Linq.Enumerable.ToArray%2A> の無使用によってパフォーマンス ヒットが発生するのは、カード デッキの新しい並びが、毎回、直前の並びから作成されるためです。 遅延評価を使用すると、`startingDeck` を作成したコードを実行するときでさえも、新しいデッキ構成が毎回最初のデッキから作成されることになります。 これでは、余分な作業が多く発生してしまいます。

実際には、先行評価を使用すると効率的に動作するアルゴリズムもあれば、遅延評価を使用したほうがよいアルゴリズムもあります。 日常の使用では、データ ソースがデータベース エンジンのように個別のプロセスである場合は、遅延評価のほうが通常は適しています。 データベースでは、遅延評価を使用すると、より複雑なクエリがデータベース プロセスに対して 1 往復だけ実行された後、残りのコードに戻ることができます。 LINQ には遅延評価と先行評価のどちらを利用するかを選択できる柔軟性があるため、プロセスを測定して、最善のパフォーマンスをもたらすほうの種類の評価を選択してください。

## <a name="conclusion"></a>まとめ

このプロジェクトでは、以下を扱いました。

- LINQ クエリを使用してデータを集計して、意味のあるシーケンスにする
- 拡張メソッドを記述して、LINQ クエリに独自のカスタム機能を追加する
- LINQ クエリによって速度の低下のようなパフォーマンスの問題が発生する可能性があるコード内の領域を見つける
- LINQ クエリに関する遅延評価と選考評価と、クエリのパフォーマンスにおけるそれらの意味

LINQ の他に、マジシャンがカードのトリックで使用するテクニックについて少し学びました。 マジシャンは、すべてのカードをデッキのどこに移動させるかを制御できるため、ファロー シャッフルを使用しています。 これがわかったからといって、種明かしをしてマジックを台無しにしないでください。

LINQ の詳細については、以下を参照してください。

- [統合言語クエリ (LINQ)](../programming-guide/concepts/linq/index.md)
- [LINQ の概要](../programming-guide/concepts/linq/index.md)
- [LINQ クエリの基本操作 (C#)](../programming-guide/concepts/linq/basic-linq-query-operations.md)
- [LINQ によるデータ変換 (C#)](../programming-guide/concepts/linq/data-transformations-with-linq.md)
- [LINQ でのクエリ構文とメソッド構文 (C#)](../programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq.md)
- [LINQ をサポートする C# の機能](../programming-guide/concepts/linq/features-that-support-linq.md)
