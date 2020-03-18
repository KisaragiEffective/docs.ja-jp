---
title: 判別共用体
description: 判別共用体のF#使用方法について説明します。
ms.date: 05/16/2016
ms.openlocfilehash: 539e2843c0bbc8c5ac9c0597ffc5443f8cd127f8
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452644"
---
# <a name="discriminated-unions"></a>判別共用体

判別共用体は、数多くの名前付きケースのうちのいずれかである可能性がある値をサポートします。ケースの値や型が、それぞれ異なる場合もあります。 判別共用体は、異種データ、有効ケースやエラー ケースなどの特殊なケースを持つ可能性のあるデータ、インスタンスごとに型が異なるデータ、小さいオブジェクト階層に対する代替手段などの場合に役立ちます。 さらに、再帰的な判別共用体は、ツリー データ構造を表すために使用されます。

## <a name="syntax"></a>構文

```fsharp
[ attributes ]
type [accessibility-modifier] type-name =
    | case-identifier1 [of [ fieldname1 : ] type1 [ * [ fieldname2 : ] type2 ...]
    | case-identifier2 [of [fieldname3 : ]type3 [ * [ fieldname4 : ]type4 ...]

    [ member-list ]
```

## <a name="remarks"></a>コメント

判別共用体は他の言語の共用体型と似ていますが、異なる点もあります。 C++ の共用体型や Visual Basic のバリアント型と同じように、値に格納されるデータは固定ではありません。複数の異なるオプションのいずれかを格納できます。 ただし、これらの他の言語の共用体とは異なり、使用可能な各オプションには*ケース識別子*が指定されています。 ケース識別子には、使用できる各種の値の型の名前を指定します。指定した型のオブジェクトは値の型として使用できます。値は省略可能です。 値を省略する場合は、ケースが列挙型のケースと等しくなります。 値がある場合は、各値に、指定した型の単一値、あるいは同じ型または異なる型の複数のフィールドを集約したタプルを使用できます。 個々のフィールドには名前を付けることができますが、同じケース内の他のフィールドに名前が付けられていても、名前は省略可能です。

判別共用体のアクセシビリティの既定値は `public`です。

たとえば、Shape 型の次のような宣言を検討します。

```fsharp
type Shape =
    | Rectangle of width : float * length : float
    | Circle of radius : float
    | Prism of width : float * float * height : float
```

前のコードでは、判別共用体の Shape を宣言します。これには四角形、円、およびプリズムの 3 つのケースのいずれかの値を指定できます。 各ケースに異なるフィールド セットがあります。 四角形の場合は、2 つの名前付きフィールドがあり、両方とも `float` 型で、幅と長さの名前が付いています。 円の場合は、1 つの名前付きフィールドがあり、半径の名前が付いています。 Prism ケースには3つのフィールドがあり、そのうち2つ (幅と高さ) は名前付きフィールドです。 名前のないフィールドは匿名フィールドと呼ばれます。

次の例では、名前付きフィールドと匿名フィールドに値を設定してオブジェクトを構築します。

```fsharp
let rect = Rectangle(length = 1.3, width = 10.0)
let circ = Circle (1.0)
let prism = Prism(5., 2.0, height = 3.0)
```

このコードは、初期化時に名前付きフィールドを使用することも、宣言時のフィールドの順序に依存して各フィールドの値のみを順番に提供することもできることを示します。 前のコードの `rect` のコンストラクターの呼び出しでは名前付きフィールドを使用しますが、`circ` のコンストラクター呼び出しでは順序を使用します。 `prism` の構造のように、順序付きフィールドと名前付きフィールドを混在させることができます。

`option` 型は、F# コア ライブラリの単純な判別共用体です。 `option` 型は、次のように宣言されます。

```fsharp
// The option type is a discriminated union.
type Option<'a> =
    | Some of 'a
    | None
```

このコードでは、`Option` 型が、`Some` と `None` の 2 つのケースを持つ判別共用体として指定されています。 `Some` ケースには、型が型パラメーター `'a` によって表される 1 つの匿名フィールドで構成される、関連付けられた値があります。 `None` ケースには、関連する値はありません。 したがって、`option` 型は、なんらかの型の値を持つジェネリック型、または値を持たないジェネリック型を指定します。 また、`Option` 型には小文字の型のエイリアス `option` があり、より一般的に使用されます。

ケース識別子は、判別共用体型のコンストラクターとして使用できます。 たとえば、`option` 型の値を作成するには、次のようなコードが使用されます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2001.fs)]

ケース識別子は、パターン マッチ式でも使用されます。 パターン マッチ式では、個別のケースに関連付けられる値に対して識別子が提供されます。 たとえば、次のコードの `x` は、`Some` 型の `option` ケースと関連付けられた値が指定された識別子です。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2002.fs)]

パターン一致式では、名前付きフィールドを使用して判別共用体一致を指定できます。 前に宣言された Shape 型には、次のコードに示すように、フィールドの値を抽出するために名前付きフィールドを使用できます。

```fsharp
let getShapeWidth shape =
    match shape with
    | Rectangle(width = w) -> w
    | Circle(radius = r) -> 2. * r
    | Prism(width = w) -> w
```

通常は、共用体の名前で、修飾せずにケース識別子を使用できます。 名前を共用体の名前で常に修飾する場合は、 [RequireQualifiedAccess](https://msdn.microsoft.com/visualfsharpdocs/conceptual/core.requirequalifiedaccessattribute-class-[fsharp])属性を共用体型の定義に適用します。

### <a name="unwrapping-discriminated-unions"></a>判別共用体のラップ解除

判別F#共用体では、単一の型をラップするためにドメインモデリングでよく使用されます。 パターンマッチングを使用して基になる値を簡単に抽出することもできます。 1つのケースに一致式を使用する必要はありません。

```fsharp
let ([UnionCaseIdentifier] [values]) = [UnionValue]
```

この動作を次の例で示します。

```fsharp
type ShaderProgram = | ShaderProgram of id:int

let someFunctionUsingShaderProgram shaderProgram =
    let (ShaderProgram id) = shaderProgram
    // Use the unwrapped value
    ...
```

関数のパラメーターでは、パターンマッチングも直接許可されているので、1つのケースをラップ解除できます。

```fsharp
let someFunctionUsingShaderProgram (ShaderProgram id) =
    // Use the unwrapped value
    ...
```

## <a name="struct-discriminated-unions"></a>構造体の判別共用体

判別共用体を構造体として表すこともできます。  これは、`[<Struct>]` 属性を使用して行われます。

```fsharp
[<Struct>]
type SingleCase = Case of string

[<Struct>]
type Multicase =
    | Case1 of Case1 : string
    | Case2 of Case2 : int
    | Case3 of Case3 : double
```

これらは値型であり、参照型ではないため、参照判別共用体の場合と比較して、追加の考慮事項があります。

1. 値型としてコピーされ、値型のセマンティクスを持ちます。
2. Multicase struct 判別共用体と共に再帰型定義を使用することはできません。
3. Multicase struct 判別共用体には、一意のケース名を指定する必要があります。

## <a name="using-discriminated-unions-instead-of-object-hierarchies"></a>オブジェクト階層ではなく、判別共用体を使用する場合

多くの場合、小さいオブジェクト階層をさらに簡単に表すために判別共用体を使用できます。 たとえば、円や正方形などの派生型を持つ `Shape` 基底クラスの代わりに、次の判別共用体を使用できます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2003.fs)]

オブジェクト指向の実装で使用される、面積や周を計算するための仮想メソッドの代わりに、パターン マッチを使用して、これらの量を計算するための適切な式に分岐できます。 次の例では、図形に応じて、異なる数式を使用して面積を計算しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2004.fs)]

出力は次のようになります。

```console
Area of circle that has radius 15.000000: 706.858347
Area of square that has side 10.000000: 100.000000
Area of rectangle that has height 5.000000 and width 10.000000 is 50.000000
```

## <a name="using-discriminated-unions-for-tree-data-structures"></a>ツリー データ構造への判別共用体の使用

判別共用体は再帰的に使用できます。つまり、共用体自体を 1 つ以上のケースの型に含めることができます。 再帰的な判別共用体を使用してツリー構造を作成でき、このツリー構造をプログラミング言語での式のモデル化に使用できます。 次のコードでは、再帰的な判別共用体を使用して、バイナリ ツリーのデータ構造を作成しています。 この共用体を構成する 2 つのケースは、整数値および左と右のサブツリーを持つノードである `Node` と、ツリーを終了する `Tip` です。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2005.fs)]

このコードでは、`resultSumTree` の値は 10 です。 次の図は、`myTree` のツリー構造を示しています。

![MyTree のツリー構造を示す図。](../media/discriminated-unions/tree-structure-mytree.png)

判別共用体は、ツリーのノードが異種の場合でも、問題なく機能します。 次のコードの `Expression` 型は、数と変数の加算と乗算をサポートする簡単なプログラミング言語での式の抽象構文ツリーを表します。 共用体の一部のケースは再帰的ではなく、数 (`Number`) または変数 (`Variable`) を表します。 他のケースは再帰的で、演算 (`Add` および `Multiply`) を表し、オペランドも式です。 `Evaluate` 関数では、match 式を使用して再帰的に構文ツリーを処理しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2006.fs)]

このコードを実行すると、`result` の値は 5 になります。

## <a name="members"></a>メンバー

判別共用体のメンバーを定義することができます。 次の例は、プロパティを定義し、インターフェイスを実装する方法を示しています。

```fsharp
open System

type IPrintable =
    abstract Print: unit -> unit

type Shape =
    | Circle of float
    | EquilateralTriangle of float
    | Square of float
    | Rectangle of float * float

    member this.Area =
        match this with
        | Circle r -> 2.0 * Math.PI * r
        | EquilateralTriangle s -> s * s * sqrt 3.0 / 4.0
        | Square s -> s * s
        | Rectangle(l, w) -> l * w

    interface IPrintable with
        member this.Print () =
            match this with
            | Circle r -> printfn "Circle with radius %f" r
            | EquilateralTriangle s -> printfn "Equilateral Triangle of side %f" s
            | Square s -> printfn "Square with side %f" s
            | Rectangle(l, w) -> printfn "Rectangle with length %f and width %f" l w
```

## <a name="common-attributes"></a>共通属性

判別共用体では、一般的に次の属性が見られます。

- `[<RequireQualifiedAccess>]`
- `[<NoEquality>]`
- `[<NoComparison>]`
- `[<Struct>]`

## <a name="see-also"></a>参照

- [F# 言語リファレンス](index.md)
