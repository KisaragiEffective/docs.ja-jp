---
title: C# 8.0 の新機能 - C# ガイド
description: C# 8.0 で使用できる新しい機能の概要を説明します。
ms.date: 09/20/2019
ms.openlocfilehash: 540b95beaf00c17812a3b602602504278be69b0e
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74429391"
---
# <a name="whats-new-in-c-80"></a>C# 8.0 の新機能

C# 8.0 では、C# 言語に次の機能と機能強化が追加されています。

- [読み取り専用メンバー](#readonly-members)
- [既定のインターフェイス メソッド](#default-interface-methods)
- [パターン マッチングの拡張機能](#more-patterns-in-more-places):
  - [switch 式](#switch-expressions)
  - [プロパティのパターン](#property-patterns)
  - [タプル パターン](#tuple-patterns)
  - [位置指定パターン](#positional-patterns)
- [using 宣言](#using-declarations)
- [静的ローカル関数](#static-local-functions)
- [破棄可能な ref 構造体](#disposable-ref-structs)
- [Null 許容参照型](#nullable-reference-types)
- [非同期ストリーム](#asynchronous-streams)
- [インデックスと範囲](#indices-and-ranges)
- [null 合体割り当て](#null-coalescing-assignment)
- [構築されたアンマネージド型](#unmanaged-constructed-types)
- [入れ子になった式の stackalloc](#stackalloc-in-nested-expressions)
- [verbatim 補間文字列の拡張](#enhancement-of-interpolated-verbatim-strings)

C# 8.0 は **.NET Core 3.x** と **.NET Standard 2.1** でサポートされています。 詳細については、「[C# 言語のバージョン管理](../language-reference/configure-language-version.md)」を参照してください。

この記事の以降では、これらの機能について簡単に説明します。 詳細な記事がある場合は、それらのチュートリアルと概要へのリンクが提供されています。 `dotnet try` グローバル ツールを使って、これらの機能をご自身の環境で調べることができます。

1. [dotnet try](https://github.com/dotnet/try/blob/master/README.md#setup) グローバル ツールをインストールします。
1. [dotnet/try-samples](https://github.com/dotnet/try-samples) リポジトリを複製します。
1. 現在のディレクトリを、*try-samples* リポジトリの *csharp8* サブディレクトリに設定します。
1. `dotnet try` を実行します。

## <a name="readonly-members"></a>読み取り専用メンバー

構造体のメンバーに `readonly` 修飾子を適用できます。 これは、メンバーが状態を変更しないことを示します。 `readonly` 修飾子を `struct` 宣言に適用するよりも詳細になります。  次の変更可能な構造体を検討します。

```csharp
public struct Point
{
    public double X { get; set; }
    public double Y { get; set; }
    public double Distance => Math.Sqrt(X * X + Y * Y);

    public override string ToString() =>
        $"({X}, {Y}) is {Distance} from the origin";
}
```

`ToString()` メソッドでは、ほとんどの構造体と同様に状態を変更しません。 それを示すには、`ToString()` の宣言に修飾子 `readonly` を追加します。

```csharp
public readonly override string ToString() =>
    $"({X}, {Y}) is {Distance} from the origin";
```

`ToString` が `readonly` とマークされていない `Distance` プロパティにアクセスするため、上記の変更により、コンパイラの警告が生成されます。

```console
warning CS8656: Call to non-readonly member 'Point.Distance.get' from a 'readonly' member results in an implicit copy of 'this'
```

コンパイラからは、防御用のコピーを作成する必要があるときに警告されます。  `Distance` プロパティでは状態を変更しないため、次のように宣言に `readonly` 修飾子を追加することで、この警告を修正できます。

```csharp
public readonly double Distance => Math.Sqrt(X * X + Y * Y);
```

`readonly` 修飾子は読み取り専用プロパティに必要であることに注意してください。 コンパイラでは、`get` アクセサーが状態を変更しないことを想定していないため、`readonly` を明示的に宣言する必要があります。 自動実装プロパティは例外です。このコンパイラは、自動実装されたすべてのゲッターを readonly として処理します。したがって、ここでは、`X` および `Y` プロパティに `readonly` 修飾子を追加する必要はありません。

コンパイラによって、`readonly` メンバーによって状態が変更されないというルールが適用されます。 次のメソッドは、`readonly` 修飾子を削除しない限りコンパイルされません。

```csharp
public readonly void Translate(int xOffset, int yOffset)
{
    X += xOffset;
    Y += yOffset;
}
```

この機能により、設計の意図を指定し、コンパイラによってそれが適用され、その意図に基づいて最適化が行われるようにすることができます。 読み取り専用メンバーの詳細については、[`readonly`](../language-reference/keywords/readonly.md#readonly-member-examples) の言語リファレンスに関する記事を参照してください。

## <a name="default-interface-methods"></a>既定のインターフェイス メソッド

ここでインターフェイスにメンバーを追加し、それらのメンバーの実装を提供できます。 この言語機能を使用することで、API 作成者は、インターフェイスの既存の実装とのソースやバイナリの互換性を損なうことなく、新しいバージョンのそのインターフェイスにメソッドを追加できます。 既存の実装では既定の実装が*継承*されます。 さらに、この機能により、同様の機能をサポートする Android や Swift を対象とする API を、C# と連携させることができます。 既定のインターフェイス メソッドでは、"traits" 言語機能のようなシナリオも可能になります。

既定のインターフェイス メソッドにより、多くのシナリオと言語要素が影響を受けます。 最初のチュートリアルでは、[既定の実装でのインターフェイスの更新](../tutorials/default-interface-methods-versions.md)について取り上げています。 その他のチュートリアルとリファレンスの更新は、一般公開に間に合うように提供されます。

## <a name="more-patterns-in-more-places"></a>より多くの場所でより多くのパターン

**パターン マッチング**では、関連はあっても種類が異なるデータをまたがってシェイプに依存する機能を提供するツールが用意されています。 C# 7.0 では、[`is`](../language-reference/keywords/is.md) 式と [`switch`](../language-reference/keywords/switch.md) ステートメントを使用することで、型パターンと定数パターンの構文が導入されました。 これらの機能では、データと機能が分かれて存在するプログラミング パラダイムのサポートに向けた最初の試験的なステップが示されました。 業界はマイクロサービスと他のクラウド ベース アーキテクチャに向けて移動しており、他の言語ツールが必要になっています。

C# 8.0 では、このボキャブラリが展開されて、コードのより多くの場所で、より多くのパターン式を使用できます。 データと機能が分かれているときは、これらの機能を検討してください。 アルゴリズムがオブジェクトのランタイム型以外の事実に依存している場合は、パターン マッチングを検討してください。 これらの手法では、設計を表現する別の方法が提供されます。

新しい場所での新しいパターンだけでなく、C# 8.0 では**再帰パターン**が追加されています。 パターン式の結果は式です。 再帰パターンは、単に、別のパターン式の出力に適用されるパターン式です。

### <a name="switch-expressions"></a>switch 式

多くの場合、[`switch`](../language-reference/keywords/switch.md) ステートメントでは、その各 `case` ブロックで値が生成されます。 **switch 式**を使用すると、より簡潔な式の構文を使用できます。 反復的な `case` や `break` キーワード、および中かっこの数が少なくなります。  たとえば、虹の色を示す次のような列挙型について考えます。

```csharp
public enum Rainbow
{
    Red,
    Orange,
    Yellow,
    Green,
    Blue,
    Indigo,
    Violet
}
```

アプリケーションで `R`、`G`、および `B` コンポーネントから構成される `RGBColor` 型が定義されている場合は、switch 式を含む次のメソッドを使用して、`Rainbow` の値をその RGB 値に変換できます。

```csharp
public static RGBColor FromRainbow(Rainbow colorBand) =>
    colorBand switch
    {
        Rainbow.Red    => new RGBColor(0xFF, 0x00, 0x00),
        Rainbow.Orange => new RGBColor(0xFF, 0x7F, 0x00),
        Rainbow.Yellow => new RGBColor(0xFF, 0xFF, 0x00),
        Rainbow.Green  => new RGBColor(0x00, 0xFF, 0x00),
        Rainbow.Blue   => new RGBColor(0x00, 0x00, 0xFF),
        Rainbow.Indigo => new RGBColor(0x4B, 0x00, 0x82),
        Rainbow.Violet => new RGBColor(0x94, 0x00, 0xD3),
        _              => throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand)),
    };
```

この構文ではいくつかの点が改良されています。

- 変数は `switch` キーワードの前にあります。 順序を変えることで、switch ステートメントからの switch 式の視覚的な区別が容易になります。
- `case` 要素と `:` 要素は、`=>` に置き換えられます。 より簡潔でわかりやすくなります。
- `default` ケースは、`_` 破棄に置き換えられます。
- 本体は式であり、ステートメントではありません。

従来の `switch` ステートメントを使用した同等のコードと比較してください。

```csharp
public static RGBColor FromRainbowClassic(Rainbow colorBand)
{
    switch (colorBand)
    {
        case Rainbow.Red:
            return new RGBColor(0xFF, 0x00, 0x00);
        case Rainbow.Orange:
            return new RGBColor(0xFF, 0x7F, 0x00);
        case Rainbow.Yellow:
            return new RGBColor(0xFF, 0xFF, 0x00);
        case Rainbow.Green:
            return new RGBColor(0x00, 0xFF, 0x00);
        case Rainbow.Blue:
            return new RGBColor(0x00, 0x00, 0xFF);
        case Rainbow.Indigo:
            return new RGBColor(0x4B, 0x00, 0x82);
        case Rainbow.Violet:
            return new RGBColor(0x94, 0x00, 0xD3);
        default:
            throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand));
    };
}
```

### <a name="property-patterns"></a>プロパティ パターン

**プロパティ パターン**を使用すると、調査対象のオブジェクトのプロパティと照合することができます。 購入者の住所に基づいて消費税を計算する必要がある eコマース サイトについて考えます。 そのような計算は、`Address` クラスの主な役割ではありません。 時間とともに、おそらくは住所の形式の変更より頻繁に、変更されます。 消費税の金額は、住所の `State` プロパティに依存します。 次のメソッドでは、プロパティ パターンを使用して、住所と価格から消費税を計算しています。

```csharp
public static decimal ComputeSalesTax(Address location, decimal salePrice) =>
    location switch
    {
        { State: "WA" } => salePrice * 0.06M,
        { State: "MN" } => salePrice * 0.75M,
        { State: "MI" } => salePrice * 0.05M,
        // other cases removed for brevity...
        _ => 0M
    };
```

パターン マッチングにより、このアルゴリズムを表現するための簡潔な構文が作成されます。

### <a name="tuple-patterns"></a>タプル パターン

いくつかのアルゴリズムは複数の入力に依存しています。 **タプル パターン**を使うと、[タプル](../tuples.md)として表現された複数の値に基づいて切り替えを行うことができます。  "*rock、paper、scissors (じゃんけん)* " ゲーム用の switch 式を示すコードを以下に示します。

```csharp
public static string RockPaperScissors(string first, string second)
    => (first, second) switch
    {
        ("rock", "paper") => "rock is covered by paper. Paper wins.",
        ("rock", "scissors") => "rock breaks scissors. Rock wins.",
        ("paper", "rock") => "paper covers rock. Paper wins.",
        ("paper", "scissors") => "paper is cut by scissors. Scissors wins.",
        ("scissors", "rock") => "scissors is broken by rock. Rock wins.",
        ("scissors", "paper") => "scissors cuts paper. Scissors wins.",
        (_, _) => "tie"
    };
```

メッセージは勝者を示しています。 破棄のケースは、引き分けとなる 3 つの組み合わせ、またはその他のテキスト入力を表します。

### <a name="positional-patterns"></a>位置指定パターン

一部の型には、そのプロパティを個別の変数に分解する `Deconstruct` メソッドが含まれています。 `Deconstruct` メソッドにアクセスできる場合、**位置指定パターン**を使ってオブジェクトのプロパティを検査し、パターン用にそれらのプロパティを使うことができます。  `X` と `Y` の個別の変数を作成する `Deconstruct` メソッドを含む `Point` クラスの例を次に示します。

```csharp
public class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y) => (X, Y) = (x, y);

    public void Deconstruct(out int x, out int y) =>
        (x, y) = (X, Y);
}
```

さらに、クアドラントのさまざまな位置を表す次の列挙を検討してください。

```csharp
public enum Quadrant
{
    Unknown,
    Origin,
    One,
    Two,
    Three,
    Four,
    OnBorder
}
```

次のメソッドでは、**位置指定パターン**を使用して、`x` と `y` の値を抽出しています。 その後、`when` 句を使用して、点の `Quadrant` を決定します。

```csharp
static Quadrant GetQuadrant(Point point) => point switch
{
    (0, 0) => Quadrant.Origin,
    var (x, y) when x > 0 && y > 0 => Quadrant.One,
    var (x, y) when x < 0 && y > 0 => Quadrant.Two,
    var (x, y) when x < 0 && y < 0 => Quadrant.Three,
    var (x, y) when x > 0 && y < 0 => Quadrant.Four,
    var (_, _) => Quadrant.OnBorder,
    _ => Quadrant.Unknown
};
```

前の switch での破棄パターンは、`x` または `y` のどちらか一方が 0 のときに一致しますが、両方とも 0 のときには一致しません。 switch 式は、値を生成するか、または例外をスローする必要があります。 どのケースとも一致しない場合、switch 式は例外をスローします。 可能性のあるすべてのケースが switch 式で網羅されていない場合、コンパイラで警告が生成されます。

この[パターン マッチングの高度なチュートリアル](../tutorials/pattern-matching.md)で、パターン マッチング手法を確認できます。

## <a name="using-declarations"></a>using 宣言

**using 宣言**は、`using` キーワードが前に付いている変数宣言です。 宣言されている変数を外側のスコープの最後に破棄する必要があることを、コンパイラに伝えます。 たとえば、テキスト ファイルを書き込む次のようなコードについて考えます。

```csharp
static int WriteLinesToFile(IEnumerable<string> lines)
{
    using var file = new System.IO.StreamWriter("WriteLines2.txt");
    // Notice how we declare skippedLines after the using statement.
    int skippedLines = 0;
    foreach (string line in lines)
    {
        if (!line.Contains("Second"))
        {
            file.WriteLine(line);
        }
        else
        {
            skippedLines++;
        }
    }
    // Notice how skippedLines is in scope here.
    return skippedLines;
    // file is disposed here
}
```

上の例では、メソッドの右中かっこに達した時点で、ファイルは破棄されます。 そこは、`file` が宣言されているスコープの末端です。 上記のコードは、従来の [using ステートメント](../language-reference/keywords/using-statement.md)を使用する次のコードと同等です。

```csharp
static int WriteLinesToFile(IEnumerable<string> lines)
{
    // We must declare the variable outside of the using block
    // so that it is in scope to be returned.
    int skippedLines = 0;
    using (var file = new System.IO.StreamWriter("WriteLines2.txt"))
    {
        foreach (string line in lines)
        {
            if (!line.Contains("Second"))
            {
                file.WriteLine(line);
            }
            else
            {
                skippedLines++;
            }
        }
    } // file is disposed here
    return skippedLines;
}
```

上の例では、`using` ステートメントに関連付けられている右中かっこに達すると、ファイルは破棄されます。

どちらの場合も、コンパイラでは `Dispose()` の呼び出しが生成されます。 `using` ステートメント内の式を破棄できない場合、コンパイラによってエラーが生成されます。

## <a name="static-local-functions"></a>静的ローカル関数

`static` 修飾子をローカル関数に追加することにより、ローカル関数で外側のスコープの変数がキャプチャ (参照) されないようにすることができます。 それを行うと、`CS8421` "静的ローカル関数は \<variable> への参照を含むことができない" が生成されます。 

次のコードについて考えてみましょう。 ローカル関数 `LocalFunction` は、外側のスコープ (`M` メソッド) で宣言されている変数 `y` にアクセスしています。 そのため、`LocalFunction` では `static` 修飾子を宣言することはできません。

```csharp
int M()
{
    int y;
    LocalFunction();
    return y;

    void LocalFunction() => y = 0;
}
```

次のコードには、静的ローカル関数が含まれています。 外側のスコープ内のどの変数にもアクセスしていないため、静的にすることができます。

```csharp
int M()
{
    int y = 5;
    int x = 7;
    return Add(x, y);

    static int Add(int left, int right) => left + right;
}
```

## <a name="disposable-ref-structs"></a>破棄可能な ref 構造体

`ref` 修飾子付きで宣言されている `struct` ではインターフェイスを実装できないので、<xref:System.IDisposable> を実装できません。 したがって、`ref struct` を破棄できるようにするには、アクセス可能な `void Dispose()` メソッドを持っている必要があります。 この機能は、`readonly ref struct` 宣言にも当てはまります。

## <a name="nullable-reference-types"></a>null 許容参照型

null 許容注釈コンテキスト内では、参照型のすべての変数は、**null 非許容参照型**と見なされます。 変数が null 許容であることを示したい場合は、型名に `?` を追加し、**null 許容参照型**として変数を宣言する必要があります。

null 非許容参照型の場合は、コンパイラでフロー分析を使用して、ローカル変数が宣言時に null 以外の値に初期化されることが確認されます。 フィールドは、構築時に初期化される必要があります。 変数が使用可能ないずれかのコンストラクターの呼び出しまたは初期化子によって設定されていない場合、コンパイラで警告が生成されます。 さらに、null 非許容参照型に、null になる可能性がある値を割り当てることはできません。

null 許容参照型の場合は、null に割り当てられたり初期化されたりしないことは確認されません。 ただし、null 許容参照型の変数が null 非許容参照型にアクセスしたり割り当てられたりするときは、その前に、コンパイラでフロー分析を使用して、null 値のチェックが行われます。

詳しくは、「[null 許容参照型](../nullable-references.md)」の概要をご覧ください。 この [null 許容参照型チュートリアル](../tutorials/nullable-reference-types.md)の新しいアプリケーションを使って、自分で試してみてください。 既存のコードベースを null 許容参照型を使用するように移行する手順について詳しくは、[null 許容参照型を使用するようにアプリケーションを移行する方法についてのチュートリアル](../tutorials/upgrade-to-nullable-references.md)に関する記事をご覧ください。

## <a name="asynchronous-streams"></a>非同期ストリーム

C# 8.0 以降では、ストリームを非同期的に作成して使用することができます。 非同期ストリームを返すメソッドには、次の 3 つの特徴があります。

1. `async` 修飾子を使用して宣言されています。
1. <xref:System.Collections.Generic.IAsyncEnumerable%601> を返します。
1. メソッドに、連続する要素を非同期ストリームで返すための `yield return` ステートメントが含まれています。

非同期ストリームを使用するには、ストリームの要素を列挙するときに、`foreach` キーワードの前に `await` キーワードを追加する必要があります。 `await` キーワードを追加するには、非同期ストリームを列挙するメソッドが、`async` 修飾子を使用して宣言されていて、`async` メソッドに対して許可される型を返すようになっている必要があります。 通常は、<xref:System.Threading.Tasks.Task> または <xref:System.Threading.Tasks.Task%601> を返すことを意味します。 <xref:System.Threading.Tasks.ValueTask> または <xref:System.Threading.Tasks.ValueTask%601> にすることもできます。 同じメソッドで非同期ストリームの使用と生成の両方を行うことができます。これは、そのメソッドが <xref:System.Collections.Generic.IAsyncEnumerable%601> を返すことを意味します。 次のコードでは、0 から 19 の値のシーケンスが生成され、各値の間に 100 ミリ秒の待機が設けられています。

```csharp
public static async System.Collections.Generic.IAsyncEnumerable<int> GenerateSequence()
{
    for (int i = 0; i < 20; i++)
    {
        await Task.Delay(100);
        yield return i;
    }
}
```

シーケンスの列挙は、`await foreach` ステートメントを使用して行います。

```csharp
await foreach (var number in GenerateSequence())
{
    Console.WriteLine(number);
}
```

[非同期ストリームの作成と使用](../tutorials/generate-consume-asynchronous-stream.md)に関するチュートリアルを使用して、自分で非同期ストリームを試すことができます。

## <a name="indices-and-ranges"></a>インデックスと範囲

インデックスと範囲には、シーケンス内の 1 つの要素または範囲にアクセスできる簡潔な構文が用意されています。

この言語のサポートでは、次の 2 つの新しい型と 2 つの新しい演算子を使用しています。

- <xref:System.Index?displayProperty=nameWithType> はシーケンスとしてインデックスを表します。
- index from end 演算子の `^`。シーケンスの末尾から相対的なインデックスを指定します。
- <xref:System.Range?displayProperty=nameWithType> はシーケンスのサブ範囲を表します。
- 範囲演算子の `..`。範囲の先頭と末尾をそのオペランドとして指定します。

インデックスのルールから始めましょう。 配列 `sequence` を考えます。 `0` インデックスは `sequence[0]` と同じです。 `^0` インデックスは `sequence[sequence.Length]` と同じです。 `sequence[sequence.Length]` と同様に、`sequence[^0]` は例外をスローすることに注意してください。 任意の数値 `n` の場合、インデックス `^n` は `sequence.Length - n` と同じです。

範囲は、範囲の*先頭*と*末尾*を指定します。 範囲の先頭は包含ですが、範囲の末尾は排他です。つまり、"*先頭*" は範囲に含まれますが、"*末尾*" は範囲に含まれません。 範囲 `[0..^0]` は、`[0..sequence.Length]` が範囲全体を表すのと同じように、範囲全体を表します。

いくつか例を見てみましょう。 先頭および末尾からのインデックスの注釈が付けられた、次のような配列について考えます。

```csharp
var words = new string[]
{
                // index from start    index from end
    "The",      // 0                   ^9
    "quick",    // 1                   ^8
    "brown",    // 2                   ^7
    "fox",      // 3                   ^6
    "jumped",   // 4                   ^5
    "over",     // 5                   ^4
    "the",      // 6                   ^3
    "lazy",     // 7                   ^2
    "dog"       // 8                   ^1
};              // 9 (or words.Length) ^0
```

最後の単語は、`^1` というインデックスで取得することができます。

```csharp
Console.WriteLine($"The last word is {words[^1]}");
// writes "dog"
```

次のコードでは、単語 "quick"、"brown"、"fox" から成る部分範囲が作成されます。 それには、`words[1]` から `words[3]` までが含まれます。 要素 `words[4]` が範囲内にありません。

```csharp
var quickBrownFox = words[1..4];
```

次のコードでは、"lazy" と "dog" の部分範囲が作成されます。 それには、`words[^2]` と `words[^1]` が含まれます。 末尾インデックス `words[^0]` は含まれません。

```csharp
var lazyDog = words[^2..^0];
```

次の例では、先頭と末尾の一方または両方が開いている範囲が作成されます。

```csharp
var allWords = words[..]; // contains "The" through "dog".
var firstPhrase = words[..4]; // contains "The" through "fox"
var lastPhrase = words[6..]; // contains "the", "lazy" and "dog"
```

変数として範囲を宣言することもできます。

```csharp
Range phrase = 1..4;
```

その場合、範囲は文字 `[` と `]` の内側で使用できます。

```csharp
var text = words[phrase];
```

配列でインデックスと範囲がサポートされるだけではありません。 [string](../language-reference/builtin-types/reference-types.md#the-string-type)、<xref:System.Span%601>、または <xref:System.ReadOnlySpan%601> と共にインデックスと範囲を使用することもできます。 詳細については、「[インデックスと範囲の型のサポート](../tutorials/ranges-indexes.md#type-support-for-indices-and-ranges)」を参照してください。

チュートリアルでのインデックスと範囲について詳しくは、「[Indices and ranges (インデックスと範囲)](../tutorials/ranges-indexes.md)」で調べることができます。

## <a name="null-coalescing-assignment"></a>null 合体割り当て

C# 8.0 では、null 合体割り当て演算子 `??=` が導入されています。 左側のオペランドが `null` に評価された場合にのみ、`??=` 演算子を使用して右側のオペランドの値を左側のオペランドに割り当てることができます。

```csharp
List<int> numbers = null;
int? i = null;

numbers ??= new List<int>();
numbers.Add(i ??= 17);
numbers.Add(i ??= 20);

Console.WriteLine(string.Join(" ", numbers));  // output: 17 17
Console.WriteLine(i);  // output: 17
```

詳細については、「[?? and ??= 演算子](../language-reference/operators/null-coalescing-operator.md)」の記事を参照してください。

## <a name="unmanaged-constructed-types"></a>構築されたアンマネージド型

C# 7.3 以前では、構築された型 (少なくとも 1 つの型引数を含む型) は[アンマネージド型](../language-reference/builtin-types/unmanaged-types.md)にできません。 C# 8.0 以降、アンマネージド型のフィールドのみが含まれている場合、構築された値型はアンマネージドになります。

たとえば、次の `Coords<T>` ジェネリック型の定義があるとします。

```csharp
public struct Coords<T>
{
    public T X;
    public T Y;
}
```

この `Coords<int>` 型は、C# 8.0 以降ではアンマネージド型です。 あらゆるアンマネージド型の場合と同様に、この型の変数へのポインターを作成したり、この型のインスタンスの[スタックにメモリ ブロックを割り当て](../language-reference/operators/stackalloc.md)たりすることができます。

```csharp
Span<Coords<int>> coordinates = stackalloc[]
{
    new Coords<int> { X = 0, Y = 0 },
    new Coords<int> { X = 0, Y = 3 },
    new Coords<int> { X = 4, Y = 0 }
};
```

詳細については、「[アンマネージド型](../language-reference/builtin-types/unmanaged-types.md)」を参照してください。

## <a name="stackalloc-in-nested-expressions"></a>入れ子になった式の stackalloc

C# 8.0 以降、[stackalloc](../language-reference/operators/stackalloc.md) 式の結果が <xref:System.Span%601?displayProperty=nameWithType> または <xref:System.ReadOnlySpan%601?displayProperty=nameWithType> 型になる場合、他の式で `stackalloc` 式を使用できます。

```csharp
Span<int> numbers = stackalloc[] { 1, 2, 3, 4, 5, 6 };
var ind = numbers.IndexOfAny(stackalloc[] { 2, 4, 6 ,8 });
Console.WriteLine(ind);  // output: 1
```

## <a name="enhancement-of-interpolated-verbatim-strings"></a>verbatim 補間文字列の拡張

verbatim [補間](../language-reference/tokens/interpolated.md)文字列において、`$` および `@` のトークンの順序は任意です。`$@"..."` と `@$"..."` は両方とも有効な verbatim 補間文字列です。 以前のバージョンの C# では、`$` トークンは `@` トークンの前に記述する必要があります。
