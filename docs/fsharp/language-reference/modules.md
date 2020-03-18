---
title: モジュール
description: F#モジュールがF#プログラムにおける値、型、関数値などのコードのF#グループ化について説明します。
ms.date: 04/24/2017
ms.openlocfilehash: fbde0c8b001d88614ba2de49c4aa7bfa098c6945
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425051"
---
# <a name="modules"></a>モジュール

言語のコンテキストでは、*モジュール*とは、 F#プログラムにおけるF#値、型、関数値などのコードのグループです。 F# モジュールのコードをグループ化すると、関連するコードをまとめることができ、プログラム内で名前の競合を回避するのに役立ちます

## <a name="syntax"></a>構文

```fsharp
// Top-level module declaration.
module [accessibility-modifier] [qualified-namespace.]module-name
declarations
// Local module declaration.
module [accessibility-modifier] module-name =
    declarations
```

## <a name="remarks"></a>Remarks

F#モジュールは、`do` バインド内F#の型、値、関数値、コードなどのコード構造をグループ化したものです。 静的メンバーのみを持つ共通言語ランタイム (CLR) クラスとして実装されます。 モジュール宣言には、ファイル全体がモジュールに含まれるかどうかに応じて、最上位のモジュール宣言とローカルモジュール宣言の2種類があります。 最上位レベルのモジュール宣言には、モジュール内のファイル全体が含まれます。 最上位レベルのモジュール宣言は、ファイル内の最初の宣言としてのみ使用できます。

最上位のモジュール宣言の構文では、省略可能な*修飾名前空間*は、モジュールを含む入れ子になった名前空間の名前のシーケンスです。 修飾された名前空間は、事前に宣言されている必要はありません。

最上位のモジュールで宣言にインデントを付ける必要はありません。 ローカルモジュール内のすべての宣言をインデントする必要があります。 ローカルモジュール宣言では、そのモジュール宣言の下でインデントされた宣言のみがモジュールの一部になります。

コードファイルが最上位のモジュール宣言または名前空間宣言で開始されていない場合、ローカルモジュールを含め、ファイルの内容全体が、ファイルと同じ名前を持つ、暗黙的に作成された最上位レベルのモジュールの一部になります。拡張子はありません。最初の文字を大文字に変換したもの。 たとえば、次のファイルについて考えてみます。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6601.fs)]

このファイルは、次のように記述されているかのようにコンパイルされます。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6602.fs)]

ファイルに複数のモジュールがある場合は、モジュールごとにローカルモジュール宣言を使用する必要があります。 外側の名前空間が宣言されている場合、これらのモジュールは外側の名前空間の一部になります。 外側の名前空間が宣言されていない場合、これらのモジュールは、暗黙的に作成された最上位レベルのモジュールの一部になります。 次のコード例は、複数のモジュールを含むコードファイルを示しています。 コンパイラは、`Multiplemodules`という名前のトップレベルモジュールを暗黙的に作成し、その最上位モジュールに入れ子になって `MyModule2` `MyModule1` します。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6603.fs)]

1つのプロジェクトまたは1つのコンパイルに複数のファイルがある場合、またはライブラリをビルドする場合は、ファイルの先頭に名前空間宣言またはモジュール宣言を含める必要があります。 F#コンパイラは、プロジェクトまたはコンパイルコマンドラインにファイルが1つしかない場合に、暗黙的にモジュール名を特定するだけで、アプリケーションを作成します。

*アクセシビリティ修飾子*には、`public`、`private`、`internal`のいずれかを指定できます。 詳細については、「[Access Control](access-control.md)」(アクセス制御) を参照してください。 既定値はパブリックです。

## <a name="referencing-code-in-modules"></a>参照 (モジュールのコードを)

別のモジュールから関数、型、および値を参照する場合は、修飾名を使用するか、モジュールを開く必要があります。 修飾名を使用する場合は、名前空間、モジュール、および必要なプログラム要素の識別子を指定する必要があります。 修飾されたパスの各部分は、次のようにドット (.) で区切ります。

`Namespace1.Namespace2.ModuleName.Identifier`

モジュールまたは1つ以上の名前空間を開いて、コードを簡略化できます。 名前空間とモジュールを開く方法の詳細については、「[宣言のインポート: `open` キーワード](import-declarations-the-open-keyword.md)」を参照してください。

次のコード例は、ファイルの末尾までのすべてのコードを含む最上位のモジュールを示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6604.fs)]

同じプロジェクト内の別のファイルからこのコードを使用するには、次の例に示すように、修飾名を使用するか、または関数を使用する前にモジュールを開きます。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6605.fs)]

## <a name="nested-modules"></a>入れ子になったモジュール

モジュールは入れ子にすることができます。 内部モジュールは、新しいモジュールではなく内部モジュールであることを示すために、外部モジュール宣言と同じようにインデントする必要があります。 たとえば、次の2つの例を比較します。 モジュール `Z` は、次のコードの内部モジュールです。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6607.fs)]

ただし、モジュール `Z` は、次のコードのモジュール `Y` の兄弟です。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6608.fs)]
モジュール `Z` は、次のコードの兄弟モジュールでもあります。これは、モジュール `Y`内の他の宣言とは別にインデントされないためです。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6609.fs)]
最後に、外側のモジュールに宣言がなく、その後に別のモジュール宣言が直後にある場合、新しいモジュール宣言は内部モジュールと見なされますが、2番目のモジュール定義が次の値よりも多くインデントされていない場合は、コンパイラによって警告が表示されます。まずは。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6610.fs)]
警告を回避するには、内側のモジュールをインデントします。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6611.fs)]
ファイル内のすべてのコードを1つの外部モジュールに配置し、内部モジュールを使用する場合、外側のモジュールでは等号を必要とせず、外側のモジュールで使用される内部モジュール宣言を含む宣言をインデントする必要はありません。 内部モジュール宣言内の宣言は、インデントする必要があります。 次のコードはこのケースを示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/modules/snippet6612.fs)]

## <a name="recursive-modules"></a>再帰的なモジュール

F#4.1 では、含まれているすべてのコードを相互に再帰的に使用できるモジュールの概念が導入されています。  これは `module rec`によって行われます。  `module rec` を使用すると、型とモジュール間で相互参照コードを記述できないという問題を軽減できます。  この例を次に示します。

```fsharp
module rec RecursiveModule =
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

例外 `DontSqueezeTheBananaException` とクラス `Banana` 両方が相互に参照されていることに注意してください。  さらに、モジュール `BananaHelpers` とクラス `Banana` も相互に参照します。  `RecursiveModule` モジュールから `rec` キーワードを削除しF#た場合、でを表すことはできません。

この機能は、4.1 を使用しF#た[名前空間](namespaces.md)でも可能です。

## <a name="see-also"></a>関連項目

- [F# 言語リファレンス](index.md)
- [名前空間](namespaces.md)
- [F#RFC FS-1009-ファイル内のより大きなスコープで相互参照型とモジュールを許可する](https://github.com/fsharp/fslang-design/blob/master/FSharp-4.1/FS-1009-mutually-referential-types-and-modules-single-scope.md)
