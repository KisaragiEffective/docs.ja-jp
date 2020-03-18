---
title: 名前空間
description: 名前F#空間を使用して、プログラム要素のグループに名前を添付できるようにすることで、関連する機能の領域にコードを整理する方法について説明します。
ms.date: 12/08/2018
ms.openlocfilehash: a55da1592b04c64576b4c66de61b5ca137289a6f
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425042"
---
# <a name="namespaces"></a>名前空間

名前空間を使用すると、プログラム要素のF#グループに名前を付けることができるので、関連する機能の領域にコードを整理できます。 名前空間は、通常、ファイル内F#の最上位の要素です。

## <a name="syntax"></a>構文

```fsharp
namespace [rec] [parent-namespaces.]identifier
```

## <a name="remarks"></a>Remarks

名前空間にコードを配置する場合は、ファイルの最初の宣言で名前空間を宣言する必要があります。 ファイル内に他の名前空間宣言が存在しない場合は、ファイル全体の内容が名前空間の一部になります。 その場合、次の名前空間宣言までのすべてのコードは、最初の名前空間内にあると見なされます。

名前空間には、値と関数を直接含めることはできません。 代わりに、値と関数をモジュールに含める必要があり、モジュールは名前空間に含まれています。 名前空間には、型、モジュールを含めることができます。

XML ドキュメントコメントは名前空間の上に宣言できますが、無視されます。 コンパイラディレクティブは、名前空間の上に宣言することもできます。

名前空間は、namespace キーワードを使用して明示的に宣言することも、モジュールを宣言するときに暗黙的に宣言することもできます。 名前空間を明示的に宣言するには、namespace キーワードを使用し、その後に名前空間名を指定します。 次の例は、型とその名前空間に含まれるモジュールを使用して `Widgets` 名前空間を宣言するコードファイルを示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6406.fs)]

ファイルの内容全体が1つのモジュールに含まれている場合は、`module` キーワードを使用して、完全修飾モジュール名に新しい名前空間名を指定することで、名前空間を暗黙的に宣言することもできます。 次の例では、関数を含む名前空間 `Widgets` とモジュール `WidgetsModule`を宣言するコードファイルを示します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6401.fs)]

次のコードは、前のコードに相当しますが、モジュールはローカルモジュール宣言です。 この場合、名前空間は独自の行に記述する必要があります。

[!code-fsharp[Main](~/samples/snippets/fsharp/namespaces/snippet6402.fs)]

1つ以上の名前空間で同じファイルに複数のモジュールが必要な場合は、ローカルモジュール宣言を使用する必要があります。 ローカルモジュール宣言を使用する場合、モジュール宣言で修飾された名前空間を使用することはできません。 次のコードは、名前空間宣言と2つのローカルモジュール宣言を含むファイルを示しています。 この場合、モジュールは名前空間に直接含まれています。ファイルと同じ名前を持つ、暗黙的に作成されたモジュールはありません。 ファイル内のその他のコード (`do` バインドなど) は名前空間にありますが、内部モジュールには存在しません。そのため、モジュール名を使用して `widgetFunction` モジュールメンバーを修飾する必要があります。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6403.fs)]

この例の出力は次のようになります。

```fsharp
Module1 10 20
Module2 5 6
```

詳細については、「[モジュール](modules.md)」を参照してください。

## <a name="nested-namespaces"></a>入れ子になった名前空間

入れ子になった名前空間を作成する場合は、完全修飾する必要があります。 それ以外の場合は、新しい最上位レベルの名前空間を作成します。 名前空間の宣言では、インデントは無視されます。

次の例は、入れ子になった名前空間を宣言する方法を示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6404.fs)]

## <a name="namespaces-in-files-and-assemblies"></a>ファイルとアセンブリ内の名前空間

名前空間は、1つのプロジェクトまたはコンパイルで複数のファイルにまたがることができます。 *名前空間フラグメント*という用語は、1つのファイルに含まれる名前空間の部分を記述します。 名前空間は、複数のアセンブリにまたがることもできます。 たとえば、`System` 名前空間には、多数のアセンブリにまたがり、入れ子になった多数の名前空間が含まれている .NET Framework 全体が含まれます。

## <a name="global-namespace"></a>グローバル名前空間

定義済みの名前空間 `global` を使用して、.NET の最上位レベルの名前空間に名前を付けます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6407.fs)]

また、グローバルを使用してトップレベルの .NET 名前空間を参照することもできます。たとえば、名前の競合を他の名前空間と解決するために使用できます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6408.fs)]

## <a name="recursive-namespaces"></a>再帰的な名前空間

また、含まれているすべてのコードが相互に再帰的になるように、名前空間を再帰として宣言することもできます。  これは `namespace rec`によって行われます。 `namespace rec` を使用すると、型とモジュール間で相互参照コードを記述できないという問題を軽減できます。 この例を次に示します。

```fsharp
namespace rec MutualReferences

type Orientation = Up | Down
type PeelState = Peeled | Unpeeled

// This exception depends on the type below.
exception DontSqueezeTheBananaException of Banana

type BananaPeel() = class end

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

例外 `DontSqueezeTheBananaException` とクラス `Banana` 両方が相互に参照されていることに注意してください。  さらに、モジュール `BananaHelpers` とクラス `Banana` も相互に参照します。 `MutualReferences` 名前空間から `rec` キーワードをF#削除した場合、でを表すことはできません。

この機能は、最上位レベルの[モジュール](modules.md)でも使用できます。

## <a name="see-also"></a>関連項目

- [F# 言語リファレンス](index.md)
- [モジュール](modules.md)
- [F#RFC FS-1009-ファイル内のより大きなスコープで相互参照型とモジュールを許可する](https://github.com/fsharp/fslang-design/blob/master/FSharp-4.1/FS-1009-mutually-referential-types-and-modules-single-scope.md)
