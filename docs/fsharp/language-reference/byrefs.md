---
title: Byrefs
description: 下位レベルのプログラミングに使用される、 F#の byref および byref に似た型について説明します。
ms.date: 11/04/2019
ms.openlocfilehash: 2d98d325dc4ad26548fb2cc6aa5b872e152ee0a8
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77092789"
---
# <a name="byrefs"></a>Byrefs

F#には、低レベルのプログラミングの領域を処理する2つの主要な機能領域があります。

* `byref`は、マネージポインターである `outref` 型 /`inref`/ます。 実行時に無効なプログラムをコンパイルできないように、使用方法に制限があります。
* `byref`に似た構造体。これは、`byref<'T>`と同じセマンティクスとコンパイル時の制限を持つ[構造体](structures.md)です。 一例として、<xref:System.Span%601>があります。

## <a name="syntax"></a>構文

```fsharp
// Byref types as parameters
let f (x: byref<'T>) = ()
let g (x: inref<'T>) = ()
let h (x: outref<'T>) = ()

// Calling a function with a byref parameter
let mutable x = 3
f &x

// Declaring a byref-like struct
open System.Runtime.CompilerServices

[<Struct; IsByRefLike>]
type S(count1: int, count2: int) =
    member x.Count1 = count1
    member x.Count2 = count2
```

## <a name="byref-inref-and-outref"></a>Byref、inref、および outref

`byref`には、次の3つの形式があります。

* `inref<'T>`、基になる値を読み取るためのマネージポインターです。
* `outref<'T>`、基になる値に書き込むためのマネージポインターです。
* `byref<'T>`、基になる値の読み取りと書き込みを行うためのマネージポインターです。

`inref<'T>` が必要な場所で `byref<'T>` を渡すことができます。 同様に、`outref<'T>` が想定されている場所で `byref<'T>` を渡すことができます。

## <a name="using-byrefs"></a>Byref の使用

`inref<'T>`を使用するには、`&`でポインター値を取得する必要があります。

```fsharp
open System

let f (dt: inref<DateTime>) =
    printfn "Now: %s" (dt.ToString())

let usage =
    let dt = DateTime.Now
    f &dt // Pass a pointer to 'dt'
```

`outref<'T>` または `byref<'T>`を使用してポインターに書き込むには、ポインターを取得する値も `mutable`に設定する必要があります。

```fsharp
open System

let f (dt: byref<DateTime>) =
    printfn "Now: %s" (dt.ToString())
    dt <- DateTime.Now

// Make 'dt' mutable
let mutable dt = DateTime.Now

// Now you can pass the pointer to 'dt'
f &dt
```

ポインターを読み取る代わりに作成するだけの場合は、`byref<'T>`ではなく `outref<'T>` を使用することを検討してください。

### <a name="inref-semantics"></a>Inref セマンティクス

次のコードについて考えてみましょう。

```fsharp
let f (x: inref<SomeStruct>) = x.SomeField
```

意味的には、これは次のことを意味します。

* `x` ポインターの所有者は、それを使用して値を読み取ることしかできません。
* `SomeStruct` 内で入れ子になって `struct` フィールドに対して取得されたポインターには、型 `inref<_>`が指定されます。

次のような場合もあります。

* 他のスレッドまたは別名には、`x`への書き込みアクセス権がないという意味はありません。
* `inref`で `x` ことによって `SomeStruct` が不変であるという意味はありません。

ただし、変更F#できない値型の**場合は、** `this` ポインターが `inref`と推測されます。

これらのルールはすべて、`inref` ポインターの所有者が、ポイントされているメモリの直接の内容を変更しない可能性があることを意味します。

### <a name="outref-semantics"></a>Outref セマンティクス

`outref<'T>` の目的は、ポインターを書き込むだけであることを示すことです。 予期しない `outref<'T>` は、名前に関係なく、基になる値の読み取りを許可します。 これは、互換性のためのものです。 意味的には、`outref<'T>` は `byref<'T>`とは異なります。

### <a name="interop-with-c"></a>C\# との相互運用

C#では、`ref` が返すだけでなく、`in ref` キーワードと `out ref` キーワードもサポートされています。 次の表は、 F#がC#出力を解釈する方法を示しています。

|C#構築|F#推論|
|------------|---------|
|戻り値の `ref`|`outref<'T>`|
|戻り値の `ref readonly`|`inref<'T>`|
|`in ref` パラメーター|`inref<'T>`|
|`out ref` パラメーター|`outref<'T>`|

次の表は、 F#が出力する内容を示しています。

|F#構築|出力されたコンストラクト|
|------------|-----------------|
|`inref<'T>` 引数|引数の `[In]` 属性|
|`inref<'T>` 返す|値の `modreq` 属性|
|抽象スロットまたは実装内の `inref<'T>`|引数または戻り値の `modreq`|
|`outref<'T>` 引数|引数の `[Out]` 属性|

### <a name="type-inference-and-overloading-rules"></a>型の推定とオーバーロードの規則

`inref<'T>` 型は、次の場合F#にコンパイラによって推論されます。

1. `IsReadOnly` 属性を持つ .NET パラメーターまたは戻り値の型。
2. 変更可能なフィールドを持たない構造体型の `this` ポインター。
3. 別の `inref<_>` ポインターから派生したメモリ位置のアドレス。

`inref` の暗黙的なアドレスを取得するときに、型 `SomeType` の引数を持つオーバーロードは `inref<SomeType>`型の引数を持つオーバーロードに優先されます。 次に例を示します。

```fsharp
type C() =
    static member M(x: System.DateTime) = x.AddDays(1.0)
    static member M(x: inref<System.DateTime>) = x.AddDays(2.0)
    static member M2(x: System.DateTime, y: int) = x.AddDays(1.0)
    static member M2(x: inref<System.DateTime>, y: int) = x.AddDays(2.0)

let res = System.DateTime.Now
let v =  C.M(res)
let v2 =  C.M2(res, 4)
```

どちらの場合も、`inref<System.DateTime>`を取るオーバーロードではなく、`System.DateTime` を受け取るオーバーロードが解決されます。

## <a name="byref-like-structs"></a>Byref に似た構造体

`byref`/`inref`/`outref` trio に加えて、`byref`に似たセマンティクスに従うことができる独自の構造体を定義することもできます。 これは、<xref:System.Runtime.CompilerServices.IsByRefLikeAttribute> 属性を使用して行います。

```fsharp
open System
open System.Runtime.CompilerServices

[<IsByRefLike; Struct>]
type S(count1: Span<int>, count2: Span<int>) =
    member x.Count1 = count1
    member x.Count2 = count2
```

`IsByRefLike` は `Struct`を意味しません。 両方とも型に存在する必要があります。

のF# "`byref`に似た" 構造体は、スタックバインド値型です。 マネージヒープに割り当てられることはありません。 `byref`のような構造体は、有効期間と非キャプチャに関する厳密なチェックセットによって適用されるため、高パフォーマンスのプログラミングに役立ちます。 規則は次のとおりです。

* これらは、関数パラメーター、メソッドパラメーター、ローカル変数、メソッドが返す値として使用できます。
* これらは、クラスまたは通常の構造体の静的メンバーまたはインスタンスメンバーにすることはできません。
* これらは、クロージャコンストラクト (`async` メソッドまたはラムダ式) によってキャプチャすることはできません。
* これらは、ジェネリックパラメーターとして使用することはできません。

この最後の点は、 F#入力の型をパラメーター化するジェネリック関数である `|>` のように、パイプラインスタイルのプログラミングにとって非常に重要です。 この制限は、インラインであるため、将来の `|>` に対して緩和される可能性があり、その本体で非インラインジェネリック関数を呼び出すことはありません。

これらの規則は使用法を厳密に制限していますが、高パフォーマンスコンピューティングの約束を安全な方法で実現するために使用されます。

## <a name="byref-returns"></a>Byref の戻り値

関数またはF#メンバーからの Byref 戻り値は、生成および使用できます。 `byref`を返すメソッドを使用する場合、値は暗黙的に逆参照されます。 次に例を示します。

```fsharp
let squareAndPrint (data : byref<int>) = 
    let squared = data*data    // data is implicitly dereferenced
    printfn "%d" squared
```

値を表す値を返すには、値を含む変数が現在のスコープよりも長く有効である必要があります。
また、byref を返すには `&value` を使用します (value は、現在のスコープよりも長い変数です)。

```fsharp
let mutable sum = 0
let safeSum (bytes: Span<byte>) =
    for i in 0 .. bytes.Length - 1 do
        sum <- sum + int bytes.[i]
    &sum  // sum lives longer than the scope of this function.
```

複数のチェーン呼び出しを通じて参照を渡すなど、暗黙的な逆参照を回避するには、`&x` (`x` は値) を使用します。

また、戻り `byref`に直接割り当てることもできます。 次のような (非常に必須の) プログラムを考えてみましょう。

```fsharp
type C() =
    let mutable nums = [| 1; 3; 7; 15; 31; 63; 127; 255; 511; 1023 |]

    override _.ToString() = String.Join(' ', nums)

    member _.FindLargestSmallerThan(target: int) =
        let mutable ctr = nums.Length - 1

        while ctr > 0 && nums.[ctr] >= target do ctr <- ctr - 1

        if ctr > 0 then &nums.[ctr] else &nums.[0]

[<EntryPoint>]
let main argv =
    let c = C()
    printfn "Original sequence: %s" (c.ToString())

    let v = &c.FindLargestSmallerThan 16

    v <- v*2 // Directly assign to the byref return

    printfn "New sequence:      %s" (c.ToString())

    0 // return an integer exit code
```

出力結果は次のとおりです。

```console
Original sequence: 1 3 7 15 31 63 127 255 511 1023
New sequence:      1 3 7 30 31 63 127 255 511 1023
```

## <a name="scoping-for-byrefs"></a>Byref のスコープ

`let`バインドされた値は、その参照が定義されているスコープを超えてはなりません。 たとえば、次のような場合は許可されません。

```fsharp
let test2 () =
    let x = 12
    &x // Error: 'x' exceeds its defined scope!

let test () =
    let x =
        let y = 1
        &y // Error: `y` exceeds its defined scope!
    ()
```

これにより、最適化でコンパイルするかどうかによって、異なる結果が得られるのを防ぐことができます。
