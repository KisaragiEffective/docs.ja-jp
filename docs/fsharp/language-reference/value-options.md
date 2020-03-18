---
title: 値のオプション
description: F# Value オプションの型について説明します。これは、オプションの型の構造体のバージョンです。
ms.date: 12/04/2019
ms.openlocfilehash: 0e9882ab4acdf2757705ef6022516d3572d87ef2
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74837117"
---
# <a name="value-options"></a>値のオプション

次の2つのF#状況では、の値オプションの種類が使用されます。

1. シナリオは、 [ F#オプション](options.md)に適しています。
2. 構造体を使用すると、シナリオにおけるパフォーマンス上の利点が得られます。

すべてのパフォーマンスを重視するシナリオは、構造体を使用して "解決" されるわけではありません。 参照型の代わりに使用する場合は、コピーにかかる追加コストを考慮する必要があります。 ただし、大F#規模なプログラムでは、多くの場合、ホットパスを通過する多くの省略可能な型がインスタンス化されるため、多くの場合、構造体はプログラムの有効期間全体のパフォーマンスを向上させることができます。

## <a name="definition"></a>Definition

Value オプションは、参照オプションの型に似た[構造体の判別共用体](discriminated-unions.md#struct-discriminated-unions)として定義されます。 その定義は、次のように考えることができます。

```fsharp
[<StructuralEquality; StructuralComparison>]
[<Struct>]
type ValueOption<'T> =
    | ValueNone
    | ValueSome of 'T
```

値オプションは構造的等価性と比較に準拠しています。 主な違いは、コンパイルされた名前、型名、およびケース名はすべて、値型であることを示しています。

## <a name="using-value-options"></a>Value オプションの使用

値のオプションは、[オプション](options.md)と同じように使用されます。 値が存在することを示すために `ValueSome` を使用し、値が存在しない場合は `ValueNone` を使用します。

```fsharp
let tryParseDateTime (s: string) =
    match System.DateTime.TryParse(s) with
    | (true, dt) -> ValueSome dt
    | (false, _) -> ValueNone

let possibleDateString1 = "1990-12-25"
let possibleDateString2 = "This is not a date"

let result1 = tryParseDateTime possibleDateString1
let result2 = tryParseDateTime possibleDateString2

match (result1, result2) with
| ValueSome d1, ValueSome d2 -> printfn "Both are dates!"
| ValueSome d1, ValueNone -> printfn "Only the first is a date!"
| ValueNone, ValueSome d2 -> printfn "Only the second is a date!"
| ValueNone, ValueNone -> printfn "None of them are dates!"
```

[オプション](options.md)と同様に、`ValueOption` を返す関数の名前付け規則は、`try`にプレフィックスを付けることです。

## <a name="value-option-properties-and-methods"></a>Value オプションのプロパティとメソッド

現時点では、Value オプションには、`Value`の1つのプロパティがあります。 このプロパティが呼び出されたときに値が存在しない場合は、<xref:System.InvalidOperationException> が発生します。

## <a name="value-option-functions"></a>Value オプションの関数

Fsharp.core の `ValueOption` モジュールには、`Option` モジュールと同等の機能が含まれています。 `defaultValueArg`のように、名前にはいくつかの違いがあります。

```fsharp
val defaultValueArg : arg:'T voption -> defaultValue:'T -> 'T
```

これは、`Option` モジュールの `defaultArg` と同じように動作しますが、代わりに値オプションを操作します。

## <a name="see-also"></a>参照

- [Options](options.md)
