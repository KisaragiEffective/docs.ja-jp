---
title: F# 4.5 の新機能 - F# ガイド
description: F# 4.5 で使用できる新しい機能の概要を説明します。
ms.date: 11/27/2019
ms.openlocfilehash: 2c978c66a4bf231398508cbc1cbb8839228ea8e9
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2020
ms.locfileid: "84202349"
---
# <a name="whats-new-in-f-45"></a>F# 4.5 の新機能

F# 4.5 では、F# 言語に複数の機能強化が加えられています。 これらの機能の多くは、F# で効率的なコードを記述できるようにし、このコードの安全性を保証するためにまとめられています。 これにより、言語へのいくつかの概念と、これらのコンストラクトを使用するときの大量のコンパイラ分析が、追加されています。

## <a name="get-started"></a>開始

F# 4.5 は、すべての .NET Core ディストリビューションと Visual Studio ツールで使用できます。 詳細については、「[F# の使用を開始する](../get-started/index.md)」をご覧ください。

## <a name="span-and-byref-like-structs"></a>Span および byref に似た構造体

.NET Core で導入された <xref:System.Span%601> 型を使用すると、厳密に型指定された方法でメモリ内のバッファーを表すことができます。F# 4.5 以降では、F# でこれを使用できます。 次の例では、別のバッファー表現を使用して、<xref:System.Span%601> で動作する関数を再利用する方法を示します。

```fsharp
let safeSum (bytes: Span<byte>) =
    let mutable sum = 0
    for i in 0 .. bytes.Length - 1 do
        sum <- sum + int bytes.[i]
    sum

// managed memory
let arrayMemory = Array.zeroCreate<byte>(100)
let arraySpan = new Span<byte>(arrayMemory)

safeSum(arraySpan) |> printfn "res = %d"

// native memory
let nativeMemory = Marshal.AllocHGlobal(100);
let nativeSpan = new Span<byte>(nativeMemory.ToPointer(), 100)

safeSum(nativeSpan) |> printfn "res = %d"
Marshal.FreeHGlobal(nativeMemory)

// stack memory
let mem = NativePtr.stackalloc<byte>(100)
let mem2 = mem |> NativePtr.toVoidPtr
let stackSpan = Span<byte>(mem2, 100)

safeSum(stackSpan) |> printfn "res = %d"
```

これの重要な点は、Span とその他の [byref に似た構造体](../language-reference/structures.md#byreflike-structs)は、コンパイラによって実行される静的分析が非常に厳密であり、予期しない状態になる可能性がある方法での使用が制限されます。 これは、F# 4.5 で導入されたパフォーマンス、表現力、安全性の間の基本的なトレードオフです。

## <a name="revamped-byrefs"></a>byref の改良

F# 4.5 より前の F# の [byref](../language-reference/byrefs.md) は安全ではないため、多くのアプリケーションでは不安定でした。 byref に関する安定性の問題が F# 4.5 で対処されており、span と byref に似た構造体に対して実行されるものと同じ静的分析も適用されました。

### <a name="inreft-and-outreft"></a>inref<'T> と outref<'T>

読み取り専用、書き込み専用、および読み取り/書き込みマネージド ポインターの概念を表すために、F# 4.5 では、それぞれ読み取り専用ポインターと書き込み専用ポインターを表す `inref<'T>` および `outref<'T>` 型が導入されています。 それぞれに異なるセマンティクスがあります。 たとえば、`inref<'T>` に書き込むことはできません。

```fsharp
let f (dt: inref<DateTime>) =
    dt <- DateTime.Now // ERROR - cannot write to an inref!
```

既定では、型の推定では、何かが既に変更可能として宣言されていない限り、F# コードでの不変の性質に合わせて、マネージド ポインターは `inref<'T>` として推論されます。 何かを書き込み可能にするには、アドレスを操作する関数またはメンバーに渡す前に、型を `mutable` として宣言する必要があります。 詳細については、「[byref](../language-reference/byrefs.md)」を参照してください。

## <a name="readonly-structs"></a>Readonly 構造体

F# 4.5 以降では、次のように <xref:System.Runtime.CompilerServices.IsReadOnlyAttribute> を使用して構造体に注釈を付けることができます。

```fsharp
[<IsReadOnly; Struct>]
type S(count1: int, count2: int) =
    member x.Count1 = count1
    member x.Count2 = count2
```

これにより、構造体で変更可能なメンバーを宣言することはできず、アセンブリから使用するときに、F# と C# で読み取り専用として扱うことができるメタデータが生成されます。 詳細については、「[ReadOnly 構造体](../language-reference/structures.md#readonly-structs)」を参照してください。

## <a name="void-pointers"></a>void ポインター

`voidptr` 型と次のような関数が F# 4.5 に追加されています。

* void ポインターをネイティブ int ポインターに変換する `NativePtr.ofVoidPtr`
* ネイティブ int ポインターを void ポインターに変換する `NativePtr.toVoidPtr`

これは、void ポインターを使用するネイティブ コンポーネントと相互運用する場合に役立ちます。

## <a name="the-match-keyword"></a>`match!` キーワード

`match!` キーワードは、評価式の内部でのパターン マッチングを強化します。

```fsharp
// Code that returns an asynchronous option
let checkBananaAsync (s: string) =
    async {
        if s = "banana" then
            return Some s
        else
            return None
    }

// Now you can use 'match!'
let funcWithString (s: string) =
    async {
        match! checkBananaAsync s with
        | Some bananaString -> printfn "It's banana!"
        | None -> printfn "%s" s
}
```

これにより、非同期などの評価式での混合オプション (または他の型) が含まれることの多いコードを短縮できます。 詳細については、「[match!](../language-reference/computation-expressions.md#match)」参照してください。

## <a name="relaxed-upcasting-requirements-in-array-list-and-sequence-expressions"></a>配列、リスト、シーケンス式での緩和されたアップキャスト要件

配列、リスト、およびシーケンス式の内部で別の型から継承する可能性のある混合型では、従来、`:>` または `upcast` を使用して派生型をその親型にアップキャストする必要がありました。 これが、次に示すように、緩和されました。

```fsharp
let x0 : obj list  = [ "a" ] // ok pre-F# 4.5
let x1 : obj list  = [ "a"; "b" ] // ok pre-F# 4.5
let x2 : obj list  = [ yield "a" :> obj ] // ok pre-F# 4.5

let x3 : obj list  = [ yield "a" ] // Now ok for F# 4.5, and can replace x2
```

## <a name="indentation-relaxation-for-array-and-list-expressions"></a>配列とリストの式のインデントの緩和

F# 4.5 より前は、メソッド呼び出しに引数として渡すときに、配列とリストの式を過度にインデントする必要がありました。 これは必要なくなりました。

```fsharp
module NoExcessiveIndenting =
    System.Console.WriteLine(format="{0}", arg = [|
        "hello"
    |])
    System.Console.WriteLine([|
        "hello"
    |])
```
