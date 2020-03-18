---
title: Char データ型 (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- vb.Char
helpviewer_keywords:
- literal type characters [Visual Basic], C
- Char data type
- C literal type character [Visual Basic]
- data types [Visual Basic], assigning
- Char data type [Visual Basic], character literals
ms.assetid: cd7547a9-7855-4e8e-b216-35d74a362657
ms.openlocfilehash: 1ed5b19a307d094fc1d5a6bb0251c57052dc9bc1
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344045"
---
# <a name="char-data-type-visual-basic"></a>Char データ型 (Visual Basic)

0 ~ 65535 の値の範囲内の符号なし16ビット (2 バイト) コードポイントを保持します。 各*コードポイント*(文字コード) は、1つの Unicode 文字を表します。

## <a name="remarks"></a>コメント

1文字だけを保持する必要があり、`String`のオーバーヘッドを必要としない場合は、`Char` データ型を使用します。 場合によっては、複数の文字を保持するために、`Char` 要素の配列 `Char()`を使用できます。

`Char` の既定値は、コードポイントが0の文字です。

## <a name="unicode-characters"></a>Unicode 文字

Unicode の最初の128コードポイント (0 ~ 127) は、標準の U.S. キーボードの文字と記号に対応しています。 これらの最初の128コードポイントは、ASCII 文字セットで定義されているものと同じです。 2番目の128コードポイント (128 ~ 255) は、ラテン語に基づくアルファベット文字、アクセント、通貨記号、分数などの特殊文字を表します。 Unicode では、世界中のコードポイント (256-65535) を使用して、世界中のテキスト文字、分音記号、数学記号、技術記号などのさまざまなシンボルを使用します。

`Char` 変数に <xref:System.Char.IsDigit%2A> や <xref:System.Char.IsPunctuation%2A> などのメソッドを使用して、Unicode 分類を決定することができます。

## <a name="type-conversions"></a>型変換

Visual Basic は、`Char` と数値型の間で直接変換されません。 <xref:Microsoft.VisualBasic.Strings.Asc%2A> または <xref:Microsoft.VisualBasic.Strings.AscW%2A> 関数を使用すると、`Char` 値をそのコードポイントを表す `Integer` に変換できます。 <xref:Microsoft.VisualBasic.Strings.Chr%2A> または <xref:Microsoft.VisualBasic.Strings.ChrW%2A> 関数を使用すると、`Integer` 値をそのコードポイントを持つ `Char` に変換できます。

型チェックスイッチ ([Option Strict ステートメント](../../../visual-basic/language-reference/statements/option-strict-statement.md)) がオンになっている場合は、リテラル型の文字を1文字の文字列リテラルに追加して、`Char` データ型として識別する必要があります。 これを次の例に示します。 `charVar` 変数への最初の代入では、`Option Strict` がオンになっているため、コンパイラエラー [BC30512](../../misc/bc30512.md)が生成されます。 リテラルの型文字 `c` が `Char` 値としてリテラルを識別するため、2番目のが正常にコンパイルされます。

```vb
Option Strict On

Module CharType
    Public Sub Main()
        Dim charVar As Char

        ' This statement generates compiler error BC30512 because Option Strict is On.  
        charVar = "Z"  

        ' The following statement succeeds because it specifies a Char literal.  
        charVar = "Z"c
    End Sub
End Module
```

## <a name="programming-tips"></a>プログラミングのヒント

- **負の数値。** `Char` は符号なしの型であり、負の値を表すことはできません。 どのような場合でも、`Char` を使用して数値を保持しないでください。

- **相互運用に関する考慮事項。** .NET Framework 用に作成されていないコンポーネント (オートメーションや COM オブジェクトなど) とのインターフェイスを使用する場合は、文字型のデータ幅が異なる (8 ビット) ことに注意してください。 このようなコンポーネントに8ビットの引数を渡す場合は、新しい Visual Basic コードで `Char` ではなく、`Byte` として宣言します。

- **拡大変換。  **   `Char` データ型は、`String`に拡大変換されます。 つまり、`Char` を `String` に変換することができ、<xref:System.OverflowException?displayProperty=nameWithType>は発生しません。

- **文字を入力します。** リテラル型の文字 `C` を1つの文字列リテラルに追加すると、`Char` データ型に強制されます。 `Char` に識別子の型文字がありません。

- **フレームワークの種類。** .NET Framework において対応する型は、<xref:System.Char?displayProperty=nameWithType> 構造体です。

## <a name="see-also"></a>参照

- <xref:System.Char?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Strings.Asc%2A>
- <xref:Microsoft.VisualBasic.Strings.AscW%2A>
- <xref:Microsoft.VisualBasic.Strings.Chr%2A>
- <xref:Microsoft.VisualBasic.Strings.ChrW%2A>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [String データ型](../../../visual-basic/language-reference/data-types/string-data-type.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [方法 : 符号なしの型を使用する Windows の機能を呼び出す](../../../visual-basic/programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
