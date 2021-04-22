---
title: アクティブ パターン
description: F# プログラミング言語で、アクティブ パターンを使用して、入力データを分割する名前付きパーティションを定義する方法について説明します。
ms.date: 05/16/2016
ms.openlocfilehash: 7b6da38baa35f30ae6de8a930be60a4e4fc0fc0d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493892"
---
# <a name="active-patterns"></a>アクティブ パターン

"*アクティブ パターン*" を使用すると、入力データを分割する名前付きパーティションを定義できます。これにより、判別共用体の場合と同様に、パターン マッチング式でこれらの名前を使用できるようになります。 アクティブ パターンを使用すると、パーティションごとにカスタマイズした方法でデータを分解できます。

## <a name="syntax"></a>構文

```fsharp
// Active pattern of one choice.
let (|identifier|) [arguments] valueToMatch = expression

// Active Pattern with multiple choices.
// Uses a FSharp.Core.Choice<_,...,_> based on the number of case names. In F#, the limitation n <= 7 applies.
let (|identifer1|identifier2|...|) valueToMatch = expression

// Partial active pattern definition.
// Uses a FSharp.Core.option<_> to represent if the type is satisfied at the call site.
let (|identifier|_|) [arguments ] valueToMatch = expression
```

## <a name="remarks"></a>解説

前の構文では、識別子は "*引数*" によって表される入力データのパーティションの名前、つまり引数のすべての値のセットのサブセットの名前です。 アクティブ パターンの定義で、最大 7 つのパーティションを指定できます。 "*式*" では、データを分解する形式を記述します。 アクティブ パターン定義を使用すると、引数として指定された値を割り当てる名前付きパーティションを決定するためのルールを定義できます。 (| と |) の記号は "*バナナ クリップ*" と呼ばれ、この型の let バインドによって作成される関数は、"*アクティブ認識エンジン*" と呼ばれます。

例として、引数を持つ次のアクティブ パターンについて考えます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5001.fs)]

次の例に示すように、パターン マッチング式でアクティブ パターンを使用できます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5002.fs)]

このプログラムの出力は次のようになります。

```console
7 is odd
11 is odd
32 is even
```

アクティブ パターンのもう 1 つの用途は、基になるデータは同じでも、さまざまな表現の可能性がある場合のように、データ型を複数の方法で分解することです。 たとえば、`Color` オブジェクトは、RGB 表現または HSB 表現に分解することができます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5003.fs)]

上のプログラムの出力は次のようになります。

```console
Red
 Red: 255 Green: 0 Blue: 0
 Hue: 360.000000 Saturation: 1.000000 Brightness: 0.500000
Black
 Red: 0 Green: 0 Blue: 0
 Hue: 0.000000 Saturation: 0.000000 Brightness: 0.000000
White
 Red: 255 Green: 255 Blue: 255
 Hue: 0.000000 Saturation: 0.000000 Brightness: 1.000000
Gray
 Red: 128 Green: 128 Blue: 128
 Hue: 0.000000 Saturation: 0.000000 Brightness: 0.501961
BlanchedAlmond
 Red: 255 Green: 235 Blue: 205
 Hue: 36.000000 Saturation: 1.000000 Brightness: 0.901961
```

アクティブ パターンのこれら 2 つの使用方法を組み合わせることで、データを適切な形式に分割および分解し、計算に最も便利な形式を使用して、適切なデータに対して適切な計算を実行できます。

結果として得られるパターン マッチング式により、非常に読みやすい方法でデータを書き込むことができるため、複雑になる可能性のある分岐やデータ分析のコードを大幅に簡素化できます。

## <a name="partial-active-patterns"></a>部分的なアクティブ パターン

場合によっては、入力領域の一部だけをパーティション分割する必要があります。 その場合は、一部の入力には一致しても、他の入力とは一致しない、部分パターンのセットを記述します。 値が生成されないことのあるアクティブ パターンは、"*部分的なアクティブ パターン*" と呼ばれます。これらには、オプションの型である戻り値があります。 部分的なアクティブ パターンを定義するには、バナナ クリップ内のパターンのリストの末尾にワイルドカード文字 (\_) を使用します。 次のコードは、部分的なアクティブ パターンの使用方法を示したものです。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5004.fs)]

前の例の出力は、次のようになります。

```console
1.100000 : Floating point
0 : Integer
0.000000 : Floating point
10 : Integer
Something else : Not matched.
```

部分的なアクティブ パターンを使用する場合、個別の選択肢を互いに素または相互に排他的にすることができますが、そうする必要はありません。 次の例では、パターン Square とパターン Cube は、正方形と立方体のどちらでもある数値 (64 など) があるため、互いに素にはなりません。 次のプログラムでは、AND パターンを使用して、Square と Cube パターンを結合しています。 1000 までの整数で、正方形でも立方体でもあるものと、立方体のみであるものがすべて出力されます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5005.fs)]

出力は次のようになります。

```console
1 is a cube and a square
8 is a cube
27 is a cube
64 is a cube and a square
125 is a cube
216 is a cube
343 is a cube
512 is a cube
729 is a cube and a square
1000 is a cube
```

## <a name="parameterized-active-patterns"></a>パラメーター化されたアクティブ パターン

アクティブ パターンは、常に、照合対象の項目について少なくとも 1 つの引数を受け取りますが、追加の引数を受け取る可能性があります。この場合は、"*パラメーター化されたアクティブ パターン*" と呼ばれます。 追加の引数を使用すると、一般的なパターンを特殊化できます。 たとえば、正規表現を使用して文字列を解析するアクティブ パターンでは、次のコードに示すように、正規表現を追加のパラメーターとして含めることがよくあります。これには、前のコード例で定義した部分的なアクティブ パターン `Integer` も使用されています。 この例では、さまざまな日付形式の正規表現を使用する文字列が、一般的な ParseRegex アクティブ パターンをカスタマイズするために指定されています。 一致した文字列を DateTime コンストラクターに渡すことができる整数に変換するために、Integer アクティブ パターンが使用されています。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5006.fs)]

前のコードの出力は、次のようになります。

```console
12/22/2008 12:00:00 AM 1/1/2009 12:00:00 AM 1/15/2008 12:00:00 AM 12/28/1995 12:00:00 AM
```

アクティブ パターンは、パターン マッチング式のみに制限されてはいません。let バインドで使用することもできます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5007.fs)]

前のコードの出力は、次のようになります。

```console
Hello, random citizen!
Hello, George!
```

## <a name="see-also"></a>関連項目

- [F# 言語リファレンス](index.md)
- [match 式](match-expressions.md)
