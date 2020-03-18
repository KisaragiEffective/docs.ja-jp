---
title: F# 4.6 の新機能- F#ガイド
description: 4\.6 でF#利用可能な新機能の概要を説明します。
ms.date: 11/27/2019
ms.openlocfilehash: 620c1edd8ea212fee306a02d5844b6b322808251
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980393"
---
# <a name="whats-new-in-f-46"></a>4\.6 のF#新機能

F#4.6 では、言語にF#複数の機能強化が追加されています。

## <a name="get-started"></a>作業開始

F#4.6 は、すべての .NET Core ディストリビューションと Visual Studio ツールで使用できます。 詳細につい[ては、 F# ](../get-started/index.md) 「」を参照してください。

## <a name="anonymous-records"></a>匿名レコード

[匿名レコード](../language-reference/anonymous-records.md)は、4.6 でF# F#導入された新しい種類の種類です。 これらは、使用前に宣言する必要のない名前付きの値の単純な集計です。 これらは、構造体または参照型として宣言できます。 既定では、これらは参照型です。

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    {| Diameter = d; Area = a; Circumference = c |}

let r = 2.0
let stats = getCircleStats r
printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
    r stats.Diameter stats.Area stats.Circumference
```

また、値型をグループ化し、パフォーマンスを重視するシナリオで動作する場合に、の構造体として宣言することもできます。

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    struct {| Diameter = d; Area = a; Circumference = c |}

let r = 2.0
let stats = getCircleStats r
printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
    r stats.Diameter stats.Area stats.Circumference
```

非常に強力であり、さまざまなシナリオで使用できます。 詳細については、「[匿名レコード](../language-reference/anonymous-records.md)」を参照してください。

## <a name="valueoption-functions"></a>ValueOption 関数

4\.5 でF#追加された valueoption 型には、"モジュールバインド関数のパリティ" がオプションの種類として含まれるようになりました。 一般的に使用される例には、次のようなものがあります。

```fsharp
// Multiply a value option by 2 if it has  value
let xOpt = ValueSome 99
let result = xOpt |> ValueOption.map (fun v -> v * 2)

// Reverse a string if it exists
let strOpt = ValueSome "Mirror image"
let reverse (str: string) =
    match str with
    | null
    | "" -> ValueNone
    | s ->
        str.ToCharArray()
        |> Array.rev
        |> string
        |> ValueSome

let reversedString = strOpt |> ValueOption.bind reverse
```

これにより、値型を持つシナリオでは、ValueOption をオプションと同じように使用して、パフォーマンスを向上させることができます。
