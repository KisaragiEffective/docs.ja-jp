---
title: アクティブ パターン
description: アクティブパターンを使用して、 F#プログラミング言語で入力データを分割する名前付きパーティションを定義する方法について説明します。
ms.date: 05/16/2016
ms.openlocfilehash: f5ed4a8600cba10d23d01628aba6ca07e543c586
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425090"
---
# <a name="active-patterns"></a>アクティブ パターン

*アクティブパターン*を使用すると、入力データを分割する名前付きパーティションを定義できます。これにより、判別共用体の場合と同様に、パターン一致式でこれらの名前を使用できるようになります。 アクティブ パターンを使用すると、パーティションごとにカスタマイズした方法でデータを分解できます。

## <a name="syntax"></a>構文

```fsharp
// Active pattern of one choice.
let (|identifier|) [arguments] valueToMatch= expression

// Active Pattern with multiple choices.
// Uses a FSharp.Core.Choice<_,...,_> based on the number of case names. In F#, the limitation n <= 7 applies.
let (|identifer1|identifier2|...|) valueToMatch = expression

// Partial active pattern definition.
// Uses a FSharp.Core.option<_> to represent if the type is satisfied at the call site.
let (|identifier|_|) [arguments ] valueToMatch = expression
```

## <a name="remarks"></a>Remarks

前の構文では、識別子は*引数*によって表される入力データのパーティションの名前、つまり、引数のすべての値のセットのサブセットの名前です。 アクティブなパターン定義には、最大7つのパーティションを指定できます。 *式*では、データを分解するフォームを記述します。 アクティブパターン定義を使用すると、引数として指定された値を持つ名前付きパーティションを決定するための規則を定義できます。 (| および |) 記号は*バナナクリップ*と呼ばれ、この型の let バインドによって作成される関数は、*アクティブレコグナイザー*と呼ばれます。

例として、引数を持つ次のアクティブパターンについて考えてみます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5001.fs)]

次の例に示すように、パターンマッチング式でアクティブパターンを使用できます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5002.fs)]

このプログラムの出力は次のとおりです。

```console
7 is odd
11 is odd
32 is even
```

アクティブパターンのもう1つの用途は、データ型を複数の方法で分解することです。たとえば、同じ基になるデータにさまざまな表現が含まれている場合などです。 たとえば、`Color` オブジェクトは、RGB 表現または HSB 表現に分解することができます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5003.fs)]

上記のプログラムの出力は次のようになります。

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

これらの2つの方法でアクティブパターンを使用することにより、データを適切な形式に分割および分解し、計算に最も便利な形式の適切なデータに対して適切な計算を実行できます。

結果として得られるパターン一致式により、非常に読みやすい方法でデータを書き込むことができるため、複雑な分岐やデータ分析コードを大幅に簡素化できます。

## <a name="partial-active-patterns"></a>部分的なアクティブパターン

場合によっては、入力領域の一部だけをパーティション分割する必要があります。 その場合は、それぞれの入力に一致し、他の入力と一致しない部分パターンのセットを記述します。 常に値を生成しないアクティブパターンは、*部分的なアクティブパターン*と呼ばれます。これらの値は、オプションの型である戻り値を持ちます。 部分的なアクティブパターンを定義するには、バナナクリップ内のパターンの一覧の末尾にワイルドカード文字 (\_) を使用します。 次のコードは、部分的なアクティブパターンの使用方法を示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5004.fs)]

前の例の出力は次のようになります。

```console
1.100000 : Floating point
0 : Integer
0.000000 : Floating point
10 : Integer
Something else : Not matched.
```

部分的なアクティブパターンを使用する場合、個別の選択肢を相互に不整合にしたり、相互に排他的に指定したりすることはできますが、そうする必要はありません。 次の例では、パターンの Square と pattern キューブは、2つの数値 (64 など) の両方を持つため、不整合にはなりません。 次のプログラムでは、パターンとパターンを使用して、正方形とキューブのパターンを結合しています。 四角形とキューブの両方であり、キューブのみであるすべての整数が1000まで出力されます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5005.fs)]

出力は次のとおりです。

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

## <a name="parameterized-active-patterns"></a>パラメーター化されたアクティブパターン

アクティブパターンは、一致する項目に対して少なくとも1つの引数を受け取りますが、追加の引数を受け取る可能性があります。この場合、パラメーター化された*アクティブパターン*という名前が適用されます。 追加の引数を使用すると、一般的なパターンを特殊化できます。 たとえば、正規表現を使用して文字列を解析するアクティブパターンでは、次のコードに示すように、正規表現を追加のパラメーターとして使用することがよくあります。これには、前のコード例で定義されている `Integer` 部分的なアクティブパターンも使用します。 この例では、さまざまな日付形式の正規表現を使用する文字列が、general ParseRegex アクティブパターンをカスタマイズするために指定されています。 整数アクティブパターンは、一致した文字列を DateTime コンストラクターに渡すことができる整数に変換するために使用されます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5006.fs)]

前のコードの出力は次のようになります。

```console
12/22/2008 12:00:00 AM 1/1/2009 12:00:00 AM 1/15/2008 12:00:00 AM 12/28/1995 12:00:00 AM
```

アクティブパターンは、パターンマッチング式のみに制限されていません。 let バインドで使用することもできます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5007.fs)]

前のコードの出力は次のようになります。

```console
Hello, random citizen!
Hello, George!
```

## <a name="see-also"></a>関連項目

- [F# 言語リファレンス](index.md)
- [match 式](match-expressions.md)
