---
title: 名前空間
description: F# の名前空間を使用して、プログラム要素のグループに名前を割り当て、関連する機能の区分にコードを編成する方法を説明します。
ms.date: 12/08/2018
ms.openlocfilehash: bf71843349434a1ea91c58dbc0477373dbb0c449
ms.sourcegitcommit: de7f589de07a9979b6ac28f54c3e534a617d9425
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/05/2020
ms.locfileid: "82796133"
---
# <a name="namespaces"></a>名前空間

名前空間を使用すると、F# のプログラム要素のグループに名前を割り当てて、関連する機能の区分にコードを編成することができます。 名前空間は、通常、F# ファイル内の最上位の要素です。

## <a name="syntax"></a>構文

```fsharp
namespace [rec] [parent-namespaces.]identifier
```

## <a name="remarks"></a>解説

名前空間にコードを配置する場合は、ファイルの最初の宣言で名前空間を宣言する必要があります。 そのようにすると、ファイル内に他の名前空間宣言が存在しない場合は、ファイル全体の内容がその名前空間の一部になります。 その場合、次の名前空間宣言までのすべてのコードは、最初の名前空間内にあるものと見なされます。

名前空間に値と関数を直接含めることはできません。 代わりに、値と関数はモジュールに含める必要があり、モジュールは名前空間に含まれます。 名前空間には、型とモジュールを含めることができます。

名前空間より上で XML ドキュメント コメントを宣言できますが、それらは無視されます。 コンパイラ ディレクティブも、名前空間より上で宣言できます。

名前空間は、namespace キーワードを使用して明示的に宣言することも、モジュールを宣言するときに暗黙的に宣言することもできます。 名前空間を明示的に宣言するには、namespace キーワードを使用し、その後で名前空間名を指定します。 次の例で示すコード ファイルでは、`Widgets` 名前空間が宣言されており、その名前空間に型とモジュールが含まれます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6406.fs)]

ファイルの内容全体が 1 つのモジュールに含まれている場合は、`module` キーワードを使用し、完全修飾モジュール名で新しい名前空間名を指定することにより、名前空間を暗黙的に宣言することもできます。 次の例で示すコード ファイルでは、名前空間 `Widgets` とモジュール `WidgetsModule` が宣言されており、関数が含まれます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6401.fs)]

次のコードは前のコードと同等ですが、モジュールはローカル モジュール宣言です。 その場合、名前空間は独自の行に記述する必要があります。

[!code-fsharp[Main](~/samples/snippets/fsharp/namespaces/snippet6402.fs)]

1 つ以上の名前空間内の同じファイルで複数のモジュールが必要な場合は、ローカル モジュール宣言を使用する必要があります。 ローカル モジュール宣言を使用する場合、モジュール宣言で修飾された名前空間を使用することはできません。 次のコードは、名前空間宣言と 2 つのローカル モジュール宣言を含むファイルを示したものです。 この場合、モジュールは名前空間に直接含まれています。ファイルと同じ名前を持つ、暗黙的に作成されるモジュールはありません。 `do` バインドなどのファイル内の他のコードは、名前空間内にありますが、内部モジュール内ではないため、モジュール名を使用してモジュール メンバー `widgetFunction` を修飾する必要があります。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6403.fs)]

この例の出力は次のとおりです。

```fsharp
Module1 10 20
Module2 5 6
```

詳細については、「[モジュール](modules.md)」を参照してください。

## <a name="nested-namespaces"></a>入れ子になった名前空間

入れ子になった名前空間を作成する場合は、それを完全修飾する必要があります。 そうしないと、新しい最上位レベルの名前空間が作成されます。 名前空間の宣言では、インデントは無視されます。

次に、入れ子の名前空間を宣言する方法の例を示します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6404.fs)]

## <a name="namespaces-in-files-and-assemblies"></a>ファイルとアセンブリ内の名前空間

名前空間は、1 つのプロジェクトまたはコンパイルで複数のファイルにまたがることができます。 "*名前空間フラグメント*" という用語は、1 つのファイルに含まれる名前空間の部分を意味します。 名前空間は、複数のアセンブリにまたがることもできます。 たとえば、`System` 名前空間には .NET Framework 全体が含まれています。これは、多数のアセンブリにまたがり、入れ子になった多数の名前空間が含まれます。

## <a name="global-namespace"></a>グローバル名前空間

.NET の最上位レベルの名前空間に名前を配置するには、定義済みの名前空間 `global` を使用します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6407.fs)]

また、global を使用して、最上位レベルの .NET 名前空間を参照することもできます。たとえば、他の名前空間との名前の競合を解決する場合などです。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6408.fs)]

## <a name="recursive-namespaces"></a>再帰的な名前空間

名前空間を再帰として宣言し、含まれるすべてのコードを相互に再帰的にすることもできます。  これは、`namespace rec` を使用して行われます。 `namespace rec` を使用すると、型とモジュールの間で相互参照コードを記述できないという問題を軽減できます。 以下にこの例を示します。

```fsharp
namespace rec MutualReferences

type Orientation = Up | Down
type PeelState = Peeled | Unpeeled

// This exception depends on the type below.
exception DontSqueezeTheBananaException of Banana

type Banana(orientation : Orientation) =
    member val IsPeeled = false with get, set
    member val Orientation = orientation with get, set
    member val Sides: PeelState list = [ Unpeeled; Unpeeled; Unpeeled; Unpeeled] with get, set

    member self.Peel() = BananaHelpers.peel self // Note the dependency on the BananaHelpers module.
    member self.SqueezeJuiceOut() = raise (DontSqueezeTheBananaException self) // This member depends on the exception above.

module BananaHelpers =
    let peel (b: Banana) =
        let flip (banana: Banana) =
            match banana.Orientation with
            | Up ->
                banana.Orientation <- Down
                banana
            | Down -> banana

        let peelSides (banana: Banana) =
            banana.Sides
            |> List.map (function
                         | Unpeeled -> Peeled
                         | Peeled -> Peeled)

        match b.Orientation with
        | Up ->   b |> flip |> peelSides
        | Down -> b |> peelSides
```

例外 `DontSqueezeTheBananaException` とクラス `Banana` の両方が相互に参照していることに注目してください。  さらに、モジュール `BananaHelpers` とクラス `Banana` も相互に参照しています。 `MutualReferences` 名前空間から `rec` キーワードを削除した場合、これを F# で表現することはできません。

この機能は、最上位レベルの[モジュール](modules.md)にも使用できます。

## <a name="see-also"></a>関連項目

- [F# 言語リファレンス](index.md)
- [モジュール](modules.md)
- [F# RFC FS-1009 - ファイル内のより大きなスコープで相互参照型とモジュールを許可する](https://github.com/fsharp/fslang-design/blob/master/FSharp-4.1/FS-1009-mutually-referential-types-and-modules-single-scope.md)
