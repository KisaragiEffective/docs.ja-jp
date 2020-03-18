---
title: 静的に解決される型パラメーター
description: F#静的に解決される型パラメーターを使用する方法について説明します。このパラメーターは、実行時ではなくコンパイル時に実際の型に置き換えられます。
ms.date: 05/16/2016
ms.openlocfilehash: 017c18dd3caaa484ddc653557573f548e3224ca0
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425008"
---
# <a name="statically-resolved-type-parameters"></a>静的に解決される型パラメーター

*静的に解決される型パラメーター*は、実行時ではなくコンパイル時に実際の型に置き換えられる型パラメーターです。 これらの前にはキャレット (^) 記号が付きます。

## <a name="syntax"></a>構文

```fsharp
ˆtype-parameter
```

## <a name="remarks"></a>Remarks

F# 言語には、異なる 2 種類の型パラメーターがあります。 1 つ目の種類は、標準のジェネリック型パラメーターです。 これらは、`'T` や `'U` のようにアポストロフィ (') で示されます。 これらは、他の .NET Framework 言語のジェネリック型パラメーターと同じです。 もう 1 つの種類は、静的に解決される型パラメーターであり、`^T` や `^U` のようにキャレット記号で示されます。

静的に解決される型パラメーターは、主にメンバー制約で使用する場合に役立ちます。これは、型引数を使用するために特定のメンバーが必要であることを指定できる制約です。 この種の制約を、標準のジェネリック型パラメーターを使用して作成する方法はありません。

2 種類の型パラメーターの類似点と相違点の概要を次の表に示します。

|特性|ジェネリック|静的に解決される|
|-------|-------|-------------------|
|構文|`'T`、 `'U`|`^T`、 `^U`|
|解決時期|実行時|コンパイル時|
|メンバー制約|メンバー制約では使用できません。|メンバー制約で使用できます。|
|コード生成|標準のジェネリック型パラメーターを持つ型 (またはメソッド) では、単一のジェネリック型またはジェネリック メソッドが生成されます。|各型で必要とされる、型およびメソッドの複数のインスタンス化が生成されます。|
|型での使用|型で使用できます。|型では使用できません。|
|インライン関数での使用|いいえ。 標準のジェネリック型パラメーターではインライン関数をパラメーター化できません。|はい。 静的に解決される型パラメーターは、インライン以外の関数またはメソッドでは使用できません。|

多数の F# コア ライブラリ関数 (特に演算子) には、静的に解決される型パラメーターがあります。 これらの関数および演算子はインラインであり、数値の計算に効率的なコードが生成されます。

演算子を使用するインライン メソッドおよび関数、または静的に解決される型パラメーターを持つ他の関数を使用するインライン メソッドおよび関数では、それらのメソッドおよび関数自体でも静的に解決される型パラメーターを使用できます。 多くの場合、型推論では、それらのインライン関数が静的に解決される型パラメーターを持つと推論されます。 静的に解決される型パラメーターを持つと推論される演算子の定義を次の例に示します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-3/snippet401.fs)]

解決された `(+@)` の型は、`(+)` および `(*)` の使用に基づいており、これらの両方では型推論によって、静的に解決される型パラメーターによるメンバー制約が推論されます。 F# インタープリターに表示される解決された型は、次のとおりです。

```fsharp
^a -> ^c -> ^d
when (^a or ^b) : (static member ( + ) : ^a * ^b -> ^d) and
(^a or ^c) : (static member ( * ) : ^a * ^c -> ^b)
```

出力は次のとおりです。

```console
2
1.500000
```

F# 4.1 以降では、静的に解決される型パラメーターシグネチャで具象型名を指定することもできます。  以前のバージョンの言語では、型名は実際にはコンパイラによって推論される可能性がありましたが、実際にはシグネチャで指定できませんでした。  4\.1 のF#ように、静的に解決される型パラメーターシグネチャで具象型名を指定することもできます。 次に例を示します。

```fsharp
let inline konst x _ = x

type CFunctor() =
    static member inline fmap (f: ^a -> ^b, a: ^a list) = List.map f a
    static member inline fmap (f: ^a -> ^b, a: ^a option) =
        match a with
        | None -> None
        | Some x -> Some (f x)

    // default implementation of replace
    static member inline replace< ^a, ^b, ^c, ^d, ^e when ^a :> CFunctor and (^a or ^d): (static member fmap: (^b -> ^c) * ^d -> ^e) > (a, f) =
        ((^a or ^d) : (static member fmap : (^b -> ^c) * ^d -> ^e) (konst a, f))

    // call overridden replace if present
    static member inline replace< ^a, ^b, ^c when ^b: (static member replace: ^a * ^b -> ^c)>(a: ^a, f: ^b) =
        (^b : (static member replace: ^a * ^b -> ^c) (a, f))

let inline replace_instance< ^a, ^b, ^c, ^d when (^a or ^c): (static member replace: ^b * ^c -> ^d)> (a: ^b, f: ^c) =
        ((^a or ^c): (static member replace: ^b * ^c -> ^d) (a, f))

// Note the concrete type 'CFunctor' specified in the signature
let inline replace (a: ^a) (f: ^b): ^a0 when (CFunctor or  ^b): (static member replace: ^a *  ^b ->  ^a0) =
    replace_instance<CFunctor, _, _, _> (a, f)
```

## <a name="see-also"></a>関連項目

- [ジェネリック](index.md)
- [型推論](../type-inference.md)
- [自動ジェネリック化](automatic-generalization.md)
- [制約](constraints.md)
- [インライン関数](../functions/inline-functions.md)
