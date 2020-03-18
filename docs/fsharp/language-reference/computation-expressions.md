---
title: コンピュテーション式
description: 制御フローの構造とバインディングを使用してF#シーケンス処理および結合できる、で計算を記述するための便利な構文を作成する方法について説明します。
ms.date: 11/04/2019
ms.openlocfilehash: 55406cc12d9e6e890fe69d712f79486d23b84452
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794547"
---
# <a name="computation-expressions"></a>コンピュテーション式

のF#コンピュテーション式は、制御フローの構造とバインディングを使用して、シーケンス処理および結合できる計算を作成するための便利な構文を提供します。 計算式の種類によっては、monads、モノ id、monads トランスフォーマー、およびアプリケーションを表現する方法と考えることができます。 ただし、Haskell*のような*他の言語とは異なり、それらは1つの抽象化に関連付けられておらず、マクロやその他の形式のメタプログラミングに依存せず、便利で状況依存の構文を実現できません。

## <a name="overview"></a>の概要

計算には多くの形式が必要です。 最も一般的な計算形式は、簡単に理解して変更できるシングルスレッド実行です。 ただし、すべての形式の計算は、シングルスレッド実行と単純なものではありません。 たとえば、次のものがあります。

- 非決定的な計算
- 非同期計算
- Effectful の計算
- 注釈の計算

一般に、アプリケーションの特定の部分で実行する必要がある*状況依存*の計算があります。 コンテキストに依存するコードを記述することは困難な場合があります。これは、特定のコンテキストの外では、抽象化を行わずに "リーク" の計算を簡単に行うことができるためです。 多くの場合、これらの抽象化は自分で記述することF#が困難です。そのため、**コンピュテーション式**と呼ばれる汎用的な方法があります。

コンピュテーション式は、文脈に依存した計算をエンコードするための統一された構文と抽象化モデルを提供します。

すべての計算式は、*ビルダー*型によってサポートされます。 ビルダーの型は、コンピュテーション式で使用できる操作を定義します。 「[コンピュテーション式の新しい型の作成](computation-expressions.md#creating-a-new-type-of-computation-expression)」を参照してください。これは、カスタム計算式の作成方法を示しています。

### <a name="syntax-overview"></a>構文の概要

すべての計算式の形式は次のとおりです。

```fsharp
builder-expr { cexper }
```

ここで `builder-expr` はコンピュテーション式を定義するビルダー型の名前、`cexper` はコンピュテーション式の式の本体です。 たとえば、`async` コンピュテーション式のコードは次のようになります。

```fsharp
let fetchAndDownload url =
    async {
        let! data = downloadData url

        let processedData = processData data

        return processedData
    }
```

前の例で示したように、コンピュテーション式内で使用できる特殊な追加の構文があります。 コンピュテーション式では、次の式の形式を使用できます。

```fsharp
expr { let! ... }
expr { do! ... }
expr { yield ... }
expr { yield! ... }
expr { return ... }
expr { return! ... }
expr { match! ... }
```

これらのキーワードおよびその他の標準F#のキーワードは、バッキングビルダーの型で定義されている場合にのみ、コンピュテーション式で使用できます。 唯一の例外は `match!`です。これは、`let!` を使用するための構文砂糖であり、その後に結果のパターンマッチが続きます。

ビルダーの型は、コンピュテーション式のフラグメントの結合方法を制御する特殊なメソッドを定義するオブジェクトです。つまり、そのメソッドは、コンピュテーション式の動作を制御します。 ビルダークラスを記述するもう1つの方法は、ループやバインドなど、さまざまF#な構造の操作をカスタマイズできるということです。

### `let!`

`let!` キーワードは、別のコンピュテーション式への呼び出しの結果を名前にバインドします。

```fsharp
let doThingsAsync url =
    async {
        let! data = getDataAsync url
        ...
    }
```

`let`でコンピュテーション式への呼び出しをバインドすると、コンピュテーション式の結果は得られません。 代わりに、そのコンピュテーション式に対して、*実現*されていない呼び出しの値をバインドします。 `let!` を使用して、結果にバインドします。

`let!` は、ビルダー型の `Bind(x, f)` メンバーによって定義されます。

### `do!`

`do!` キーワードは、`unit`に似た型 (ビルダーの `Zero` メンバーによって定義) を返すコンピュテーション式を呼び出すためのものです。

```fsharp
let doThingsAsync data url =
    async {
        do! submitData data url
        ...
    }
```

[非同期ワークフロー](asynchronous-workflows.md)の場合、この型は `Async<unit>`です。 その他の計算式では、型が `CExpType<unit>`される可能性があります。

`do!` は、`f` が `unit`を生成するビルダー型の `Bind(x, f)` メンバーによって定義されます。

### `yield`

`yield` キーワードは、<xref:System.Collections.Generic.IEnumerable%601>として使用できるように、コンピュテーション式から値を返すためのものです。

```fsharp
let squares =
    seq {
        for i in 1..10 do
            yield i * i
    }

for sq in squares do
    printfn "%d" sq
```

ほとんどの場合、呼び出し元は省略できます。 `yield` を省略する最も一般的な方法は、`->` 演算子を使用することです。

```fsharp
let squares =
    seq {
        for i in 1..10 -> i * i
    }

for sq in squares do
    printfn "%d" sq
```

多くの異なる値を生成する可能性がある複雑な式の場合、場合によってはキーワードを省略するだけで、次のことが可能です。

```fsharp
let weekdays includeWeekend =
    seq {
        "Monday"
        "Tuesday"
        "Wednesday"
        "Thursday"
        "Friday"
        if includeWeekend then
            "Saturday"
            "Sunday"
    }
```

[ C#で yield キーワード](../../csharp/language-reference/keywords/yield.md)を使用する場合と同様に、コンピュテーション式の各要素は反復処理されるときに返されます。

`yield` は、ビルダー型の `Yield(x)` メンバーによって定義されます。 `x` は、返される項目です。

### `yield!`

`yield!` キーワードは、コンピュテーション式から値のコレクションをフラット化するためのものです。

```fsharp
let squares =
    seq {
        for i in 1..3 -> i * i
    }

let cubes =
    seq {
        for i in 1..3 -> i * i * i
    }

let squaresAndCubes =
    seq {
        yield! squares
        yield! cubes
    }

printfn "%A" squaresAndCubes // Prints - 1; 4; 9; 1; 8; 27
```

評価された場合、`yield!` によって呼び出されるコンピュテーション式の項目は1つずつ返され、結果がフラット化されます。

`yield!` は、ビルダー型の `YieldFrom(x)` メンバーによって定義されます。 `x` は値のコレクションです。

`yield`とは異なり、`yield!` は明示的に指定する必要があります。 計算式では、その動作は暗黙的ではありません。

### `return`

`return` キーワードは、コンピュテーション式に対応する型の値をラップします。 `yield`を使用した計算式とは別に、計算式を "完了" するために使用されます。

```fsharp
let req = // 'req' is of type is 'Async<data>'
    async {
        let! data = fetch url
        return data
    }

// 'result' is of type 'data'
let result = Async.RunSynchronously req
```

`return` は、ビルダーの型の `Return(x)` メンバーによって定義されます。 `x` は、ラップする項目です。

### `return!`

`return!` キーワードは、コンピュテーション式の値を認識し、その結果をコンピュテーション式に対応する型にラップします。

```fsharp
let req = // 'req' is of type is 'Async<data>'
    async {
        return! fetch url
    }

// 'result' is of type 'data'
let result = Async.RunSynchronously req
```

`return!` は、ビルダーの型の `ReturnFrom(x)` メンバーによって定義されます。 `x` は別のコンピュテーション式です。

### `match!`

`match!` キーワードを使用すると、別のコンピュテーション式への呼び出しをインライン化し、結果にパターン一致を行うことができます。

```fsharp
let doThingsAsync url =
    async {
        match! callService url with
        | Some data -> ...
        | None -> ...
    }
```

`match!`でコンピュテーション式を呼び出すと、`let!`のような呼び出しの結果が認識されます。 これは、通常、結果が[省略可能](options.md)であるコンピュテーション式を呼び出すときに使用されます。

## <a name="built-in-computation-expressions"></a>組み込みのコンピュテーション式

コアF#ライブラリでは、[シーケンス式](sequences.md)、[非同期ワークフロー](asynchronous-workflows.md)、[クエリ式](query-expressions.md)という3つの組み込みのコンピュテーション式が定義されています。

## <a name="creating-a-new-type-of-computation-expression"></a>新しい種類のコンピュテーション式の作成

ビルダークラスを作成し、クラスに特定の特別なメソッドを定義することで、独自のコンピュテーション式の特性を定義できます。 ビルダークラスでは、次の表に示すように、必要に応じてメソッドを定義できます。

次の表では、ワークフロービルダークラスで使用できるメソッドについて説明します。

|**メソッド**|**一般的な署名**|**説明**|
|----|----|----|
|`Bind`|`M<'T> * ('T -> M<'U>) -> M<'U>`|コンピュテーション式で `let!` および `do!` に対して呼び出されます。|
|`Delay`|`(unit -> M<'T>) -> M<'T>`|計算式を関数としてラップします。|
|`Return`|`'T -> M<'T>`|コンピュテーション式の `return` に対して呼び出されます。|
|`ReturnFrom`|`M<'T> -> M<'T>`|コンピュテーション式の `return!` に対して呼び出されます。|
|`Run`|`M<'T> -> M<'T>`<br /><br />`M<'T> -> 'T`|コンピュテーション式を実行します。|
|`Combine`|`M<'T> * M<'T> -> M<'T>`<br /><br />`M<unit> * M<'T> -> M<'T>`|コンピュテーション式でシーケンス処理を行うために呼び出されます。|
|`For`|`seq<'T> * ('T -> M<'U>) -> M<'U>`<br /><br />`seq<'T> * ('T -> M<'U>) -> seq<M<'U>>`|コンピュテーション式の `for...do` 式に対して呼び出されます。|
|`TryFinally`|`M<'T> * (unit -> unit) -> M<'T>`|コンピュテーション式の `try...finally` 式に対して呼び出されます。|
|`TryWith`|`M<'T> * (exn -> M<'T>) -> M<'T>`|コンピュテーション式の `try...with` 式に対して呼び出されます。|
|`Using`|`'T * ('T -> M<'U>) -> M<'U> when 'T :> IDisposable`|コンピュテーション式の `use` バインドに対して呼び出されます。|
|`While`|`(unit -> bool) * M<'T> -> M<'T>`|コンピュテーション式の `while...do` 式に対して呼び出されます。|
|`Yield`|`'T -> M<'T>`|コンピュテーション式の `yield` 式に対して呼び出されます。|
|`YieldFrom`|`M<'T> -> M<'T>`|コンピュテーション式の `yield!` 式に対して呼び出されます。|
|`Zero`|`unit -> M<'T>`|コンピュテーション式の `if...then` 式の空の `else` 分岐に対して呼び出されます。|
|`Quote`|`Quotations.Expr<'T> -> Quotations.Expr<'T>`|コンピュテーション式が `Run` メンバーに引用符として渡されることを示します。 計算のすべてのインスタンスを引用符に変換します。|

ビルダークラスのメソッドの多くは、`M<'T>` コンストラクトを使用して返します。通常、これは、結合する計算の種類を特徴とする個別に定義された型です。たとえば、非同期ワークフローの場合は `Async<'T>`、シーケンスワークフローの場合は `Seq<'T>` です。 これらのメソッドのシグネチャを使用すると、1つのコンストラクトから返されるワークフローオブジェクトを次の構造体に渡すことができるように、それらを組み合わせて入れ子にすることができます。 コンパイラは、コンピュテーション式を解析するときに、前の表のメソッドとコンピュテーション式のコードを使用して、一連の入れ子になった関数呼び出しに式を変換します。

入れ子になった式の形式は次のとおりです。

```fsharp
builder.Run(builder.Delay(fun () -> {| cexpr |}))
```

上記のコードでは、コンピュテーション式ビルダークラスで定義されていない場合、`Run` と `Delay` への呼び出しは省略されます。 ここでは `{| cexpr |}`として示されているコンピュテーション式の本体は、次の表に示す翻訳によって、ビルダークラスのメソッドを含む呼び出しに変換されます。 コンピュテーション式 `{| cexpr |}` は、これらの変換に従って、`expr` がF#式で、`cexpr` がコンピュテーション式である場合に、再帰的に定義されます。

|[式]|変換|
|----------|-----------|
|<code>{ let binding in cexpr }</code>|<code>let binding in {&#124; cexpr &#124;}</code>|
|<code>{ let! pattern = expr in cexpr }</code>|<code>builder.Bind(expr, (fun pattern -> {&#124; cexpr &#124;}))</code>|
|<code>{ do! expr in cexpr }</code>|<code>builder.Bind(expr, (fun () -> {&#124; cexpr &#124;}))</code>|
|<code>{ yield expr }</code>|`builder.Yield(expr)`|
|<code>{ yield! expr }</code>|`builder.YieldFrom(expr)`|
|<code>{ return expr }</code>|`builder.Return(expr)`|
|<code>{ return! expr }</code>|`builder.ReturnFrom(expr)`|
|<code>{ use pattern = expr in cexpr }</code>|<code>builder.Using(expr, (fun pattern -> {&#124; cexpr &#124;}))</code>|
|<code>{ use! value = expr in cexpr }</code>|<code>builder.Bind(expr, (fun value -> builder.Using(value, (fun value -> { cexpr }))))</code>|
|<code>{ if expr then cexpr0 &#124;}</code>|<code>if expr then { cexpr0 } else builder.Zero()</code>|
|<code>{ if expr then cexpr0 else cexpr1 &#124;}</code>|<code>if expr then { cexpr0 } else { cexpr1 }</code>|
|<code>{ match expr with &#124; pattern_i -> cexpr_i }</code>|<code>match expr with &#124; pattern_i -> { cexpr_i }</code>|
|<code>{ for pattern in expr do cexpr }</code>|<code>builder.For(enumeration, (fun pattern -> { cexpr }))</code>|
|<code>{ for identifier = expr1 to expr2 do cexpr }</code>|<code>builder.For(enumeration, (fun identifier -> { cexpr }))</code>|
|<code>{ while expr do cexpr }</code>|<code>builder.While(fun () -> expr, builder.Delay({ cexpr }))</code>|
|<code>{ try cexpr with &#124; pattern_i -> expr_i }</code>|<code>builder.TryWith(builder.Delay({ cexpr }), (fun value -> match value with &#124; pattern_i -> expr_i &#124; exn -> reraise exn)))</code>|
|<code>{ try cexpr finally expr }</code>|<code>builder.TryFinally(builder.Delay( { cexpr }), (fun () -> expr))</code>|
|<code>{ cexpr1; cexpr2 }</code>|<code>builder.Combine({ cexpr1 }, { cexpr2 })</code>|
|<code>{ other-expr; cexpr }</code>|<code>expr; { cexpr }</code>|
|<code>{ other-expr }</code>|`expr; builder.Zero()`|

前の表では、`other-expr` テーブルに記載されていない式について説明しています。 ビルダークラスは、すべてのメソッドを実装する必要はなく、前の表に一覧表示されているすべての変換をサポートします。 実装されていない構造体は、その型のコンピュテーション式では使用できません。 たとえば、コンピュテーション式で `use` キーワードをサポートしない場合は、ビルダークラスの `Use` の定義を省略できます。

次のコード例は、一度に1ステップずつ評価できる一連のステップとして計算をカプセル化するコンピュテーション式を示しています。 判別共用体型 `OkOrException`は、これまでに評価された式のエラー状態をエンコードします。 このコードは、いくつかのビルダーメソッドの定型的な実装など、計算式で使用できる一般的なパターンを示しています。

```fsharp
// Computations that can be run step by step
type Eventually<'T> =
    | Done of 'T
    | NotYetDone of (unit -> Eventually<'T>)

module Eventually =
    // The bind for the computations. Append 'func' to the
    // computation.
    let rec bind func expr =
        match expr with
        | Done value -> func value
        | NotYetDone work -> NotYetDone (fun () -> bind func (work()))

    // Return the final value wrapped in the Eventually type.
    let result value = Done value

    type OkOrException<'T> =
        | Ok of 'T
        | Exception of System.Exception

    // The catch for the computations. Stitch try/with throughout
    // the computation, and return the overall result as an OkOrException.
    let rec catch expr =
        match expr with
        | Done value -> result (Ok value)
        | NotYetDone work ->
            NotYetDone (fun () ->
                let res = try Ok(work()) with | exn -> Exception exn
                match res with
                | Ok cont -> catch cont // note, a tailcall
                | Exception exn -> result (Exception exn))

    // The delay operator.
    let delay func = NotYetDone (fun () -> func())

    // The stepping action for the computations.
    let step expr =
        match expr with
        | Done _ -> expr
        | NotYetDone func -> func ()

    // The rest of the operations are boilerplate.
    // The tryFinally operator.
    // This is boilerplate in terms of "result", "catch", and "bind".
    let tryFinally expr compensation =
        catch (expr)
        |> bind (fun res ->
            compensation();
            match res with
            | Ok value -> result value
            | Exception exn -> raise exn)

    // The tryWith operator.
    // This is boilerplate in terms of "result", "catch", and "bind".
    let tryWith exn handler =
        catch exn
        |> bind (function Ok value -> result value | Exception exn -> handler exn)

    // The whileLoop operator.
    // This is boilerplate in terms of "result" and "bind".
    let rec whileLoop pred body =
        if pred() then body |> bind (fun _ -> whileLoop pred body)
        else result ()

    // The sequential composition operator.
    // This is boilerplate in terms of "result" and "bind".
    let combine expr1 expr2 =
        expr1 |> bind (fun () -> expr2)

    // The using operator.
    let using (resource: #System.IDisposable) func =
        tryFinally (func resource) (fun () -> resource.Dispose())

    // The forLoop operator.
    // This is boilerplate in terms of "catch", "result", and "bind".
    let forLoop (collection:seq<_>) func =
        let ie = collection.GetEnumerator()
        tryFinally
            (whileLoop
                (fun () -> ie.MoveNext())
                (delay (fun () -> let value = ie.Current in func value)))
            (fun () -> ie.Dispose())

// The builder class.
type EventuallyBuilder() =
    member x.Bind(comp, func) = Eventually.bind func comp
    member x.Return(value) = Eventually.result value
    member x.ReturnFrom(value) = value
    member x.Combine(expr1, expr2) = Eventually.combine expr1 expr2
    member x.Delay(func) = Eventually.delay func
    member x.Zero() = Eventually.result ()
    member x.TryWith(expr, handler) = Eventually.tryWith expr handler
    member x.TryFinally(expr, compensation) = Eventually.tryFinally expr compensation
    member x.For(coll:seq<_>, func) = Eventually.forLoop coll func
    member x.Using(resource, expr) = Eventually.using resource expr

let eventually = new EventuallyBuilder()

let comp = eventually {
    for x in 1..2 do
        printfn " x = %d" x
    return 3 + 4 }

// Try the remaining lines in F# interactive to see how this
// computation expression works in practice.
let step x = Eventually.step x

// returns "NotYetDone <closure>"
comp |> step

// prints "x = 1"
// returns "NotYetDone <closure>"
comp |> step |> step

// prints "x = 1"
// prints "x = 2"
// returns "Done 7"
comp |> step |> step |> step |> step
```

コンピュテーション式には基になる型があり、この式はを返します。 基になる型は、計算された結果または実行可能な遅延計算を表すことができます。また、一部の型のコレクションを反復処理する方法を提供する場合もあります。 前の例では、基になる型は**最終的**にでした。 シーケンス式の場合、基になる型は <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType>です。 クエリ式の場合、基になる型は <xref:System.Linq.IQueryable?displayProperty=nameWithType>です。 非同期ワークフローの場合、基になる型は[`Async`](https://msdn.microsoft.com/library/03eb4d12-a01a-4565-a077-5e83f17cf6f7)です。 `Async` オブジェクトは、結果を計算するために実行する作業を表します。 たとえば、 [`Async.RunSynchronously`](https://msdn.microsoft.com/library/0a6663a9-50f2-4d38-8bf3-cefd1a51fd6b)を呼び出して計算を実行し、その結果を返します。

## <a name="custom-operations"></a>カスタム操作

コンピュテーション式でカスタム操作を定義し、コンピュテーション式で演算子としてカスタム操作を使用することができます。 たとえば、クエリ式にクエリ演算子を含めることができます。 カスタム操作を定義する場合は、コンピュテーション式の Yield メソッドと For メソッドを定義する必要があります。 カスタム操作を定義するには、それをコンピュテーション式のビルダークラスに配置し、 [`CustomOperationAttribute`](https://msdn.microsoft.com/library/199f3927-79df-484b-ba66-85f58cc49b19)を適用します。 この属性は、引数として文字列を受け取ります。これはカスタム操作で使用される名前です。 この名前は、コンピュテーション式の左中かっこの先頭にあるスコープに含まれます。 したがって、このブロック内のカスタム操作と同じ名前を持つ識別子は使用しないでください。 たとえば、クエリ式で `all` や `last` などの識別子を使用しないようにします。

### <a name="extending-existing-builders-with-new-custom-operations"></a>新しいカスタム操作を使用した既存のビルダーの拡張

既にビルダークラスがある場合は、そのカスタム操作をこのビルダークラスの外部から拡張できます。 拡張機能はモジュール内で宣言する必要があります。 名前空間には、同じファイルと、型が定義されている同じ名前空間宣言グループ以外の拡張メンバーを含めることはできません。

次の例は、既存の `Microsoft.FSharp.Linq.QueryBuilder` クラスの拡張を示しています。

```fsharp
type Microsoft.FSharp.Linq.QueryBuilder with

    [<CustomOperation("existsNot")>]
    member _.ExistsNot (source: QuerySource<'T, 'Q>, predicate) =
        Enumerable.Any (source.Source, Func<_,_>(predicate)) |> not
```

## <a name="see-also"></a>関連項目

- [F# 言語リファレンス](index.md)
- [非同期ワークフロー](asynchronous-workflows.md)
- [シーケンス](https://msdn.microsoft.com/library/6b773b6b-9c9a-4af8-bd9e-d96585c166db)
- [クエリ式](query-expressions.md)
