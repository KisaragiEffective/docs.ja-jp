---
title: F# コードのフォーマットに関するガイドライン
description: コードを書式設定F#するためのガイドラインについて説明します。
ms.date: 11/04/2019
ms.openlocfilehash: 895c8211731b47bd4c59d762d5806cfc1bfe232d
ms.sourcegitcommit: 7f8eeef060ddeb2cabfa52843776faf652c5a1f5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74089312"
---
# <a name="f-code-formatting-guidelines"></a>F# コードのフォーマットに関するガイドライン

この記事では、コードを次のようF#に書式設定する方法に関するガイドラインを提供します。

* 一般に読みやすく表示される
* は、Visual Studio や他のエディターの書式設定ツールによって適用された規則に従っています
* 他のコードと同様

これらのガイドラインは、 [: Ahn-dung Phan](https://github.com/dungpa)による[ F#書式設定規則の包括的なガイド](https://github.com/dungpa/fantomas/blob/master/docs/FormattingConventions.md)に基づいています。

## <a name="general-rules-for-indentation"></a>インデントの一般的な規則

F#既定では有意の空白文字が使用されます。 次のガイドラインは、このような問題の対処方法に関するガイダンスを提供することを目的としています。

### <a name="using-spaces"></a>スペースの使用

インデントが必要な場合は、タブではなく、スペースを使用する必要があります。 少なくとも1つのスペースが必要です。 組織では、インデントに使用するスペースの数を指定するためのコーディング標準を作成できます。インデントが発生する各レベルで、2つ、3つ、または4個のインデントが行われます。

**インデントごとに4つのスペースを推奨します。**

ただし、プログラムのインデントは主観的な意味を持ちます。 バリエーションは問題ありませんが、最初に従う必要がある規則は、*インデントの一貫性*です。 一般的に受け入れられているインデントのスタイルを選択し、コードベース全体で使用します。

## <a name="formatting-white-space"></a>空白文字の書式設定

F#は空白を区別します。 空白のほとんどのセマンティクスは適切なインデントによってカバーされますが、他にも考慮すべき点があります。

### <a name="formatting-operators-in-arithmetic-expressions"></a>算術式での演算子の書式設定

バイナリ算術式の前後には常に空白文字を使用します。

```fsharp
let subtractThenAdd x = x - 1 + 3
```

単項 `-` 演算子には、常に次のように否定される値が含まれている必要があります。

```fsharp
// OK
let negate x = -x

// Bad
let negateBad x = - x
```

`-` 演算子の後に空白文字を追加すると、他のユーザーの混乱を招く可能性があります。

要約すると、常に次のことが重要になります。

* バイナリ演算子を空白で囲む
* 単項演算子の後に末尾の空白を含めることはできません

二項算術演算子のガイドラインは特に重要です。 バイナリ `-` 演算子を囲まないと、特定の書式設定の選択肢と組み合わせると、単項 `-`として解釈される可能性があります。

### <a name="surround-a-custom-operator-definition-with-white-space"></a>カスタム演算子の定義を空白で囲む

演算子の定義を囲むには、常に空白文字を使用します。

```fsharp
// OK
let ( !> ) x f = f x

// Bad
let (!>) x f = f x
```

`*` で始まり、複数の文字を持つカスタム演算子については、コンパイラのあいまいさを避けるために、定義の先頭に空白を追加する必要があります。 このため、すべての演算子の定義を1つの空白文字で囲むことをお勧めします。

### <a name="surround-function-parameter-arrows-with-white-space"></a>関数パラメーターの矢印を空白で囲む

関数の署名を定義するときは、`->` 記号の周囲に空白文字を使用します。

```fsharp
// OK
type MyFun = int -> int -> string

// Bad
type MyFunBad = int->int->string
```

### <a name="surround-function-arguments-with-white-space"></a>関数の引数を空白で囲む

関数を定義する場合は、各引数の前後に空白文字を使用します。

```fsharp
// OK
let myFun (a: decimal) b c = a + b + c

// Bad
let myFunBad (a:decimal)(b)c = a + b + c
```

### <a name="place-parameters-on-a-new-line-for-very-long-member-definitions"></a>非常に長いメンバー定義のために新しい行にパラメーターを配置する

非常に長いメンバー定義がある場合は、パラメーターを新しい行に配置し、1つのスコープにインデントを設定します。

```fsharp
type C() =
    member _.LongMethodWithLotsOfParameters(
        aVeryLongType: AVeryLongTypeThatYouNeedToUse
        aSecondVeryLongType: AVeryLongTypeThatYouNeedToUse
        aThirdVeryLongType: AVeryLongTypeThatYouNeedToUse) =
        // ... the body of the method follows
```

これは、コンストラクターにも適用されます。

```fsharp
type C(
    aVeryLongType: AVeryLongTypeThatYouNeedToUse
    aSecondVeryLongType: AVeryLongTypeThatYouNeedToUse
    aThirdVeryLongType: AVeryLongTypeThatYouNeedToUse) =
    // ... the body of the class follows
```

### <a name="type-annotations"></a>型の注釈

#### <a name="right-pad-function-argument-type-annotations"></a>右埋め込み関数の引数の型の注釈

型の注釈を持つ引数を定義する場合は、`:` の記号の後に空白文字を使用します。

```fsharp
// OK
let complexFunction (a: int) (b: int) c = a + b + c

// Bad
let complexFunctionBad (a :int) (b :int) (c:int) = a + b + c
```

#### <a name="surround-return-type-annotations-with-white-space"></a>戻り値の型の注釈を空白で囲む

Let バインド関数または値型の注釈 (関数の場合は戻り値の型) では、`:` 記号の前後に空白文字を使用します。

```fsharp
// OK
let expensiveToCompute : int = 0 // Type annotation for let-bound value
let myFun (a: decimal) b c : decimal = a + b + c // Type annotation for the return type of a function
// Bad
let expensiveToComputeBad1:int = 1
let expensiveToComputeBad2 :int = 2
let myFunBad (a: decimal) b c:decimal = a + b + c
```

## <a name="formatting-blank-lines"></a>空白行の書式設定

* 最上位の関数とクラスの定義を、2つの空白行で区切ります。
* クラス内のメソッド定義は、1つの空白行で区切られます。
* 関連する関数のグループを分離するために、余分な空白行を使用する (控えめに) ことがあります。 関連する1つの liners (たとえば、一連のダミーの実装) 間で空白行を省略することができます。
* 論理セクションを示すには、関数の空白行を控えめに使用します。

## <a name="formatting-comments"></a>コメントの書式設定

一般に、ML スタイルのブロックコメントに対しては、複数のダブルスラッシュコメントを優先します。

```fsharp
// Prefer this style of comments when you want
// to express written ideas on multiple lines.

(*
    ML-style comments are fine, but not a .NET-ism.
    They are useful when needing to modify multi-line comments, though.
*)
```

インラインコメントは、最初の文字を大文字にする必要があります。

```fsharp
let f x = x + 1 // Increment by one.
```

## <a name="naming-conventions"></a>名前付け規則

### <a name="use-camelcase-for-class-bound-expression-bound-and-pattern-bound-values-and-functions"></a>クラスバインド、式バインド、およびパターンバインドの値と関数にキャメルケースを使用する

これは、ローカル変数F#としてバインドされたすべての名前、またはパターン一致と関数定義でキャメルケースを使用するための一般的なスタイルです。

```fsharp
// OK
let addIAndJ i j = i + j

// Bad
let addIAndJ I J = I+J

// Bad
let AddIAndJ i j = i + j
```

クラスのローカルバインド関数は、キャメルケースも使用する必要があります。

```fsharp
type MyClass() =

    let doSomething () =

    let firstResult = ...

    let secondResult = ...

    member x.Result = doSomething()
```

### <a name="use-camelcase-for-module-bound-public-functions"></a>モジュールバインドパブリック関数にキャメルケースを使用する

モジュールバインド関数がパブリック API の一部である場合、キャメルケースを使用する必要があります。

```fsharp
module MyAPI =
    let publicFunctionOne param1 param2 param2 = ...

    let publicFunctionTwo param1 param2 param3 = ...
```

### <a name="use-camelcase-for-internal-and-private-module-bound-values-and-functions"></a>内部およびプライベートのモジュールバインド値および関数にキャメルケースを使用する

次のように、プライベートのモジュールバインド値にはキャメルケースを使用します。

* スクリプト内のアドホック関数

* モジュールまたは型の内部実装を構成する値

```fsharp
let emailMyBossTheLatestResults =
    ...
```

### <a name="use-camelcase-for-parameters"></a>パラメーターにキャメルケースを使用する

すべてのパラメーターは、.NET の名前付け規則に従ってキャメルケースを使用する必要があります。

```fsharp
module MyModule =
    let myFunction paramOne paramTwo = ...

type MyClass() =
    member this.MyMethod(paramOne, paramTwo) = ...
```

### <a name="use-pascalcase-for-modules"></a>モジュールに対してパスワードを使用する

すべてのモジュール (最上位レベル、内部、プライベート、入れ子になっている) では、文字を使用する必要があります。

```fsharp
module MyTopLevelModule

module Helpers =
    module private SuperHelpers =
        ...

    ...
```

### <a name="use-pascalcase-for-type-declarations-members-and-labels"></a>型宣言、メンバー、およびラベルに対しては、文字を使用します。

クラス、インターフェイス、構造体、列挙型、デリゲート、レコード、および判別共用体の名前は、すべて、文字を使用した名前にする必要があります。 レコードと判別共用体の型およびラベル内のメンバーも、文字を使用する必要があります。

```fsharp
type IMyInterface =
    abstract Something: int

type MyClass() =
    member this.MyMethod(x, y) = x + y

type MyRecord = { IntVal: int; StringVal: string }

type SchoolPerson =
    | Professor
    | Student
    | Advisor
    | Administrator
```

### <a name="use-pascalcase-for-constructs-intrinsic-to-net"></a>.NET に固有の構造体には、パスワードを使用する

名前空間、例外、イベント、およびプロジェクト/`.dll` 名でも、使用する必要があります。 他の .NET 言語からの消費が、コンシューマーにとって自然なものであるだけでなく、発生する可能性のある .NET の名前付け規則とも一貫しています。

### <a name="avoid-underscores-in-names"></a>名前にアンダースコアを使わない

これまで、 F#一部のライブラリでは、名前にアンダースコアが使用されていました。 ただし、これは .NET の名前付け規則と競合するため、広く受け入れられなくなりました。 ただし、プログラマにF#よっては、アンダースコアが頻繁に使用され、履歴上の理由により、許容範囲と尊重が重要になります。 ただし、スタイルは多くの場合、使用するかどうかを選択できる他のユーザーによって無効になっていることに注意してください。

一部の例外には、ネイティブコンポーネントとの相互運用が含まれており、アンダースコアは非常に一般的です。

### <a name="use-standard-f-operators"></a>標準F#の演算子を使用する

次の演算子はF#標準ライブラリで定義されており、同等のを定義する代わりに使用する必要があります。 コードを読みやすくし、慣用的なする傾向があるため、これらの演算子を使用することをお勧めします。 OCaml または他の関数型プログラミング言語の背景を持つ開発者は、さまざまな表現方法に慣れている場合があります。 次の一覧は、推奨F#される演算子をまとめたものです。

```fsharp
x |> f // Forward pipeline
f >> g // Forward composition
x |> ignore // Discard away a value
x + y // Overloaded addition (including string concatenation)
x - y // Overloaded subtraction
x * y // Overloaded multiplication
x / y // Overloaded division
x % y // Overloaded modulus
x && y // Lazy/short-cut "and"
x || y // Lazy/short-cut "or"
x <<< y // Bitwise left shift
x >>> y // Bitwise right shift
x ||| y // Bitwise or, also for working with “flags” enumeration
x &&& y // Bitwise and, also for working with “flags” enumeration
x ^^^ y // Bitwise xor, also for working with “flags” enumeration
```

### <a name="use-prefix-syntax-for-generics-foot-in-preference-to-postfix-syntax-t-foo"></a>後置構文 (`T Foo`) の場合は、ジェネリック (`Foo<T>`) のプレフィックス構文を使用します

F#は、ジェネリック型の名前付けの後置形式の ML スタイル (`int list`など) と、プレフィックス .NET スタイル (たとえば、`list<int>`) の両方を継承します。 次の5つの特定の種類を除き、.NET スタイルを優先します。

1. リストF#の場合は、`list<int>`ではなく、後置形式の `int list` を使用します。
2. オプションF#には、`option<int>`ではなく、後置形式の `int option` を使用します。
3. 値F#オプションの場合は、`voption<int>`ではなく、後置形式の `int voption` を使用します。
4. 配列F#の場合は、`int array` または `array<int>`ではなく、構文名 `int[]` を使用します。
5. 参照セルの場合は、`ref<int>` または `Ref<int>`ではなく `int ref` を使用します。

それ以外のすべての型については、プレフィックスフォームを使用します。

## <a name="formatting-tuples"></a>タプルの書式設定

組のインスタンス化は、かっこで囲む必要があります。また、内の区切り記号の後には、1つのスペースを指定する必要があります (例: `(1, 2)`、`(x, y, z)`)。

通常は、組のパターンマッチングでかっこを省略することをお勧めします。

```fsharp
let (x, y) = z // Destructuring
let x, y = z // OK

// OK
match x, y with
| 1, _ -> 0
| x, 1 -> 0
| x, y -> 1
```

タプルが関数の戻り値の場合は、通常、かっこを省略することもできます。

```fsharp
// OK
let update model msg =
    match msg with
    | 1 -> model + 1, []
    | _ -> model, [ msg ]
```

まとめとして、かっこで囲まれたタプルのインスタンス化を使用しますが、パターンマッチングまたは戻り値に組を使用する場合は、かっこを使用しないことをお勧めします。

## <a name="formatting-discriminated-union-declarations"></a>判別共用体宣言の書式設定

4つのスペースによる型定義内のインデント `|`:

```fsharp
// OK
type Volume =
    | Liter of float
    | FluidOunce of float
    | ImperialPint of float

// Not OK
type Volume =
| Liter of float
| USPint of float
| ImperialPint of float
```

## <a name="formatting-discriminated-unions"></a>判別共用体の書式設定

複数の行に分割されるインスタンス化された判別共用体は、含まれるデータにインデントを含む新しいスコープを指定する必要があります。

```fsharp
let tree1 =
    BinaryNode
        (BinaryNode(BinaryValue 1, BinaryValue 2),
         BinaryNode(BinaryValue 3, BinaryValue 4))
```

右かっこは、新しい行にも指定できます。

```fsharp
let tree1 =
    BinaryNode(
        BinaryNode(BinaryValue 1, BinaryValue 2),
        BinaryNode(BinaryValue 3, BinaryValue 4)
    )
```

## <a name="formatting-record-declarations"></a>レコード宣言の書式設定

4つのスペースで型定義内の `{` をインデントし、同じ行でフィールドリストを開始します。

```fsharp
// OK
type PostalAddress =
    { Address: string
      City: string
      Zip: string }
    member x.ZipAndCity = sprintf "%s %s" x.Zip x.City

// Not OK
type PostalAddress =
  { Address: string
    City: string
    Zip: string }
    member x.ZipAndCity = sprintf "%s %s" x.Zip x.City

// Unusual in F#
type PostalAddress =
    {
        Address: string
        City: string
        Zip: string
    }
```

インターフェイスの実装やレコードのメンバーを宣言する場合は、開始トークンを新しい行に配置し、終了トークンを新しい行に配置することをお勧めします。

```fsharp
// Declaring additional members on PostalAddress
type PostalAddress =
    {
        Address: string
        City: string
        Zip: string
    } with
    member x.ZipAndCity = sprintf "%s %s" x.Zip x.City

type MyRecord =
    {
        SomeField: int
    }
    interface IMyInterface
```

## <a name="formatting-records"></a>レコードの書式設定

短いレコードは、1行に記述できます。

```fsharp
let point = { X = 1.0; Y = 0.0 }
```

ラベルに新しい行を使用する必要があるレコードは次のとおりです。

```fsharp
let rainbow =
    { Boss = "Jeffrey"
      Lackeys = ["Zippy"; "George"; "Bungle"] }
```

次の場合は、開始トークンを新しい行に配置し、1つのスコープの上にある [コンテンツ] タブと、新しい行の終了トークンを使用することをお勧めします。

* 異なるインデントスコープを持つコード内でのレコードの移動
* 関数にパイプする

```fsharp
let rainbow =
    {
        Boss1 = "Jeffrey"
        Boss2 = "Jeffrey"
        Boss3 = "Jeffrey"
        Boss4 = "Jeffrey"
        Boss5 = "Jeffrey"
        Boss6 = "Jeffrey"
        Boss7 = "Jeffrey"
        Boss8 = "Jeffrey"
        Lackeys = ["Zippy"; "George"; "Bungle"]
    }

type MyRecord =
    {
        SomeField: int
    }
    interface IMyInterface

let foo a =
    a
    |> Option.map (fun x ->
        {
            MyField = x
        })
```

リストおよび配列要素にも同じ規則が適用されます。

## <a name="formatting-copy-and-update-record-expressions"></a>コピーおよび更新レコード式の書式設定

コピーと更新のレコード式は依然としてレコードであるため、同様のガイドラインが適用されます。

短い式は、1行に収めることができます。

```fsharp
let point2 = { point with X = 1; Y = 2 }
```

長い式では、新しい行を使用する必要があります。

```fsharp
let rainbow2 =
    { rainbow with
        Boss = "Jeffrey"
        Lackeys = ["Zippy"; "George"; "Bungle"] }
```

レコードガイダンスと同様に、中かっこに個別の行を追加し、式を使用して1つのスコープを右側にインデントすることもできます。 一部の特殊なケースでは、かっこを使用せずに省略可能な値をラップする場合があることに注意してください。

```fsharp
type S = { F1: int; F2: string }
type State = { F:  S option }

let state = { F = Some { F1 = 1; F2 = "Hello" } }
let newState =
    {
        state with
            F = Some {
                    F1 = 0
                    F2 = ""
                }
    }
```

## <a name="formatting-lists-and-arrays"></a>リストと配列の書式設定

`::` 演算子の前後にスペースを使用して `x :: l` を記述します (`::` は挿入演算子であり、そのためスペースで囲まれています)。

1つの行で宣言されたリストと配列には、左角かっこの前と右角かっこの前にスペースが必要です。

```fsharp
let xs = [ 1; 2; 3 ]
let ys = [| 1; 2; 3; |]
```

2つの異なるかっこで囲まれた演算子の間には、常に少なくとも1つの空白文字を使用します。 たとえば、`[` と `{`の間にはスペースを入れます。

```fsharp
// OK
[ { IngredientName = "Green beans"; Quantity = 250 }
  { IngredientName = "Pine nuts"; Quantity = 250 }
  { IngredientName = "Feta cheese"; Quantity = 250 }
  { IngredientName = "Olive oil"; Quantity = 10 }
  { IngredientName = "Lemon"; Quantity = 1 } ]

// Not OK
[{ IngredientName = "Green beans"; Quantity = 250 }
 { IngredientName = "Pine nuts"; Quantity = 250 }
 { IngredientName = "Feta cheese"; Quantity = 250 }
 { IngredientName = "Olive oil"; Quantity = 10 }
 { IngredientName = "Lemon"; Quantity = 1 }]
```

組のリストまたは配列にも同じガイドラインが適用されます。

複数の行にわたって分割されるリストと配列は、レコードと同様のルールに従います。

```fsharp
let pascalsTriangle =
    [|
        [| 1 |]
        [| 1; 1 |]
        [| 1; 2; 1 |]
        [| 1; 3; 3; 1 |]
        [| 1; 4; 6; 4; 1 |]
        [| 1; 5; 10; 10; 5; 1 |]
        [| 1; 6; 15; 20; 15; 6; 1 |]
        [| 1; 7; 21; 35; 35; 21; 7; 1 |]
        [| 1; 8; 28; 56; 70; 56; 28; 8; 1 |]
    |]
```

レコードの場合と同様に、開始角かっこと右角かっこを独自の行で宣言すると、コードの移動や関数へのパイプ処理が容易になります。

プログラムによって配列とリストを生成する場合は、常に値が生成されるときに `do ... yield` より `->` を優先します。

```fsharp
// Preferred
let squares = [ for x in 1..10 -> x*x ]

// Not preferred
let squares' = [ for x in 1..10 do yield x*x ]
```

以前のバージョンのF#言語では、データが条件付きで生成される場合や、連続して評価される式がある場合に `yield` を指定する必要があります。 古いF#言語バージョンでコンパイルする必要がある場合を除き、これらの `yield` キーワードは省略してください。

```fsharp
// Preferred
let daysOfWeek includeWeekend =
    [
        "Monday"
        "Tuesday"
        "Wednesday"
        "Thursday"
        "Friday"
        if includeWeekend then
            "Saturday"
            "Sunday"
    ]

// Not preferred
let daysOfWeek' includeWeekend =
    [
        yield "Monday"
        yield "Tuesday"
        yield "Wednesday"
        yield "Thursday"
        yield "Friday"
        if includeWeekend then
            yield "Saturday"
            yield "Sunday"
    ]
```

場合によっては、`do...yield` が読みやすくなることがあります。 ただし、このような場合は主観的に考慮する必要があります。

## <a name="formatting-if-expressions"></a>If 式の書式設定

条件のインデントは、それらを作成する式のサイズによって異なります。 `cond`、`e1` と `e2` が短い場合は、単に1行に記述するだけです。

```fsharp
if cond then e1 else e2
```

`cond`のいずれかの場合、`e1` または `e2` は長くなりますが、複数行にはなりません。

```fsharp
if cond
then e1
else e2
```

式のいずれかが複数行の場合は、次のようになります。

```fsharp
if cond then
    e1
else
    e2
```

`elif` と `else` を持つ複数の条件は、`if`と同じスコープでインデントされます。

```fsharp
if cond1 then e1
elif cond2 then e2
elif cond3 then e3
else e4
```

### <a name="pattern-matching-constructs"></a>パターン一致のコンストラクト

照合の各句に対して、インデントなしの `|` を使用します。 式が短い場合は、各部分式も単純である場合は単一行を使用することを検討してください。

```fsharp
// OK
match l with
| { him = x; her = "Posh" } :: tail -> x
| _ :: tail -> findDavid tail
| [] -> failwith "Couldn't find David"

// Not OK
match l with
    | { him = x; her = "Posh" } :: tail -> x
    | _ :: tail -> findDavid tail
    | [] -> failwith "Couldn't find David"
```

[パターン一致] の矢印の右側にある式が大きすぎる場合は、次の行に移動して、`match`/`|`の1つの手順をインデントします。

```fsharp
match lam with
| Var v -> 1
| Abs(x, body) ->
    1 + sizeLambda body
| App(lam1, lam2) ->
    sizeLambda lam1 + sizeLambda lam2

```

`function`から始まる匿名関数のパターンマッチングは、通常はあまりインデントされません。 たとえば、次のように1つのスコープをインデントすることは問題ありません。

```fsharp
lambdaList
|> List.map (function
    | Abs(x, body) -> 1 + sizeLambda 0 body
    | App(lam1, lam2) -> sizeLambda (sizeLambda 0 lam1) lam2
    | Var v -> 1)
```

`let` または `let rec` で定義された関数でのパターンマッチングは、`function` キーワードが使用されている場合でも、`let`の開始後に4つのスペースをインデントする必要があります。

```fsharp
let rec sizeLambda acc = function
    | Abs(x, body) -> sizeLambda (succ acc) body
    | App(lam1, lam2) -> sizeLambda (sizeLambda acc lam1) lam2
    | Var v -> succ acc
```

矢印は配置しないことをお勧めします。

## <a name="formatting-trywith-expressions"></a>Try/with 式の書式設定

例外の種類のパターンマッチングは、`with`と同じレベルでインデントする必要があります。

```fsharp
try
    if System.DateTime.Now.Second % 3 = 0 then
        raise (new System.Exception())
    else
        raise (new System.ApplicationException())
with
| :? System.ApplicationException ->
    printfn "A second that was not a multiple of 3"
| _ ->
    printfn "A second that was a multiple of 3"
```

## <a name="formatting-function-parameter-application"></a>関数パラメーターアプリケーションの書式設定

一般に、ほとんどの関数パラメーターアプリケーションは同じ行で実行されます。

新しい行の関数にパラメーターを適用する場合は、1つのスコープでインデントします。

```fsharp
// OK
sprintf "\t%s - %i\n\r"
     x.IngredientName x.Quantity

// OK
sprintf
     "\t%s - %i\n\r"
     x.IngredientName x.Quantity

// OK
let printVolumes x =
    printf "Volume in liters = %f, in us pints = %f, in imperial = %f"
        (convertVolumeToLiter x)
        (convertVolumeUSPint x)
        (convertVolumeImperialPint x)
```

ラムダ式には、関数の引数と同じガイドラインが適用されます。 ラムダ式の本体の場合、本文は別の行を持つことができ、1つのスコープでインデントされます。

```fsharp
let printListWithOffset a list1 =
    List.iter
        (fun elem -> printfn "%d" (a + elem))
        list1

// OK if lambda body is long enough
let printListWithOffset a list1 =
    List.iter
        (fun elem ->
            printfn "%d" (a + elem))
        list1
```

ただし、ラムダ式の本体が複数行である場合は、複数行の構成要素を関数に1つの引数として適用するのではなく、別の関数にファクタリングすることを検討してください。

### <a name="formatting-infix-operators"></a>挿入演算子の書式設定

演算子はスペースで区切ります。 このルールの明確な例外は、`!` 演算子と `.` 演算子です。

挿入式は、同じ列に編成することができます。

```fsharp
acc +
(sprintf "\t%s - %i\n\r"
     x.IngredientName x.Quantity)

let function1 arg1 arg2 arg3 arg4 =
    arg1 + arg2 +
    arg3 + arg4
```

### <a name="formatting-pipeline-operators"></a>パイプライン演算子の書式設定

パイプライン `|>` 演算子は、操作対象の式の下にある必要があります。

```fsharp
// Preferred approach
let methods2 =
    System.AppDomain.CurrentDomain.GetAssemblies()
    |> List.ofArray
    |> List.map (fun assm -> assm.GetTypes())
    |> Array.concat
    |> List.ofArray
    |> List.map (fun t -> t.GetMethods())
    |> Array.concat

// Not OK
let methods2 = System.AppDomain.CurrentDomain.GetAssemblies()
            |> List.ofArray
            |> List.map (fun assm -> assm.GetTypes())
            |> Array.concat
            |> List.ofArray
            |> List.map (fun t -> t.GetMethods())
            |> Array.concat
```

### <a name="formatting-modules"></a>モジュールの書式設定

ローカルモジュール内のコードは、モジュールに対して相対的にインデントする必要がありますが、最上位レベルのモジュール内のコードをインデントすることはできません。 名前空間要素をインデントする必要はありません。

```fsharp
// A is a top-level module.
module A

let function1 a b = a - b * b
```

```fsharp
// A1 and A2 are local modules.
module A1 =
    let function1 a b = a*a + b*b

module A2 =
    let function2 a b = a*a - b*b
```

### <a name="formatting-object-expressions-and-interfaces"></a>オブジェクト式およびインターフェイスの書式設定

オブジェクトの式とインターフェイスは、4つのスペースの後にインデントを設定する `member` のと同じ方法で配置する必要があります。

```fsharp
let comparer =
    { new IComparer<string> with
          member x.Compare(s1, s2) =
              let rev (s: String) =
                  new String (Array.rev (s.ToCharArray()))
              let reversed = rev s1
              reversed.CompareTo (rev s2) }
```

### <a name="formatting-white-space-in-expressions"></a>式の空白の書式設定

式に余分なスペースF#を含めないでください。

```fsharp
// OK
spam (ham.[1])

// Not OK
spam ( ham.[ 1 ] )
```

名前付き引数には、`=`を囲む領域を指定することもできません。

```fsharp
// OK
let makeStreamReader x = new System.IO.StreamReader(path=x)

// Not OK
let makeStreamReader x = new System.IO.StreamReader(path = x)
```

## <a name="formatting-attributes"></a>属性の書式設定

[属性](../language-reference/attributes.md)はコンストラクトの上に配置されます。

```fsharp
[<SomeAttribute>]
type MyClass() = ...

[<RequireQualifiedAccess>]
module M =
    let f x = x

[<Struct>]
type MyRecord =
    { Label1: int
      Label2: string }
```

### <a name="formatting-attributes-on-parameters"></a>パラメーターの属性の書式設定

属性をパラメーターに配置することもできます。 この場合は、パラメーターと名前の前に次の行を配置します。

```fsharp
// Defines a class that takes an optional value as input defaulting to false.
type C() =
    member _.M([<Optional; DefaultParameterValue(false)>] doSomething: bool)
```

### <a name="formatting-multiple-attributes"></a>複数の属性の書式設定

パラメーターではないコンストラクトに複数の属性が適用されている場合は、1行につき1つの属性が存在するように配置する必要があります。

```fsharp
[<Struct>]
[<IsByRefLike>]
type MyRecord =
    { Label1: int
      Label2: string }
```

パラメーターに適用する場合は、同じ行に配置し、`;` の区切り記号で区切る必要があります。

## <a name="formatting-literals"></a>リテラルの書式設定

`Literal` 属性を使用するリテラルでは、属性をそれ自体の行に配置し、文字の名前付けを使用します。 [ F# ](../language-reference/literals.md)

```fsharp
[<Literal>]
let Path = __SOURCE_DIRECTORY__ + "/" + __SOURCE_FILE__

[<Literal>]
let MyUrl = "www.mywebsitethatiamworkingwith.com"
```

属性を値と同じ行に配置することは避けてください。
