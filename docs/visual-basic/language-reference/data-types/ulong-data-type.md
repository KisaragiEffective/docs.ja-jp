---
title: ULong 型
ms.date: 01/31/2018
f1_keywords:
- vb.ulong
helpviewer_keywords:
- numbers [Visual Basic], whole
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- integers [Visual Basic], types
- data types [Visual Basic], integral
- literal type characters [Visual Basic], UL
- ULong data type
- UL literal type characters [Visual Basic]
ms.assetid: 017e0702-774e-44ae-bedc-786b424ca84e
ms.openlocfilehash: 3c6cd4086e08b808c158948756b4806f098196b9
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343881"
---
# <a name="ulong-data-type-visual-basic"></a>ULong データ型 (Visual Basic)

0 ~ 18446744073709551615 (1.84 倍以上 10 ^ 19) の値の範囲内で、符号なし64ビット (8 バイト) の整数を保持します。

## <a name="remarks"></a>コメント

`ULong` データ型を使用して、`UInteger`に対して大きすぎるバイナリデータや、可能性のある最大の符号なし整数値を格納します。

`ULong` の既定値は 0 です。

## <a name="literal-assignments"></a>リテラルの代入

`ULong` 変数は、10進リテラル、16進リテラル、8進数リテラル、または (Visual Basic 2017 で始まる) バイナリリテラルを割り当てることによって、宣言および初期化できます。 整数リテラルが `ULong` の範囲外にある場合 (つまり、<xref:System.UInt64.MinValue?displayProperty=nameWithType> より小さいか、<xref:System.UInt64.MaxValue?displayProperty=nameWithType> より大きい場合)、コンパイル エラーが発生します。

次の例では、整数 7,934,076,125 を 10 進リテラル、16 進リテラル、バイナリ リテラルで表したものが、`ULong` 値に割り当てられています。

[!code-vb[ULong](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#ULong)]

> [!NOTE]
> プレフィックス `&h` または `&H` を使用して、16進リテラル、プレフィックス `&b` または `&B` がバイナリリテラルを示すようにし、プレフィックス `&o` または `&O` を使用して8進数リテラルを表します。 10 進リテラルには、プレフィックスはありません。

Visual Basic 2017 以降では、次の例に示すように、アンダースコア文字 (`_`) を桁区切り記号として使用して、読みやすくすることもできます。

[!code-vb[ULong](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#LongS)]

Visual Basic 15.5 以降では、アンダースコア文字 (`_`) をプレフィックスと16進数、バイナリ、または8進数の間の先頭の区切り記号として使用することもできます。 例 :

```vb
Dim number As ULong = &H_F9AC_0326_1489_D68C
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

数値リテラルには、次の例に示すように、`ULong` データ型を示す `UL` または `ul`[型の文字](../../programming-guide/language-features/data-types/type-characters.md)を含めることもできます。

```vb
Dim number = &H_00_00_0A_96_2F_AC_14_D7ul
```

## <a name="programming-tips"></a>プログラミングのヒント

- **負の数値。** `ULong` は符号なしの型であるため、負の数を表すことはできません。 `ULong`型に評価される式に対して単項マイナス記号 (`-`) 演算子を使用すると、Visual Basic 式が最初に `Decimal` に変換されます。

- **CLS 準拠。** `ULong` のデータ型は[共通言語仕様](https://www.ecma-international.org/publications/standards/Ecma-335.htm)(cls) の一部ではないため、cls 準拠のコードはそれを使用するコンポーネントを使用できません。

- **相互運用に関する考慮事項。** .NET Framework 用に作成されていないコンポーネント (オートメーションや COM オブジェクトなど) とやり取りしている場合は、`ulong` などの型が他の環境で異なるデータ幅 (32 ビット) を持つ可能性があることに注意してください。 このようなコンポーネントに32ビットの引数を渡す場合は、マネージ Visual Basic コードで `ULong` の代わりに `UInteger` として宣言します。

  さらに、オートメーションでは、Windows 95、Windows 98、Windows ME、または Windows 2000 で64ビットの整数はサポートされません。 これらのプラットフォームのオートメーションコンポーネントに Visual Basic `ULong` 引数を渡すことはできません。

- **広げ.** `ULong` のデータ型は、`Decimal`、`Single`、および `Double`に拡大変換されます。 つまり、<xref:System.OverflowException?displayProperty=nameWithType> エラーが発生することなく、`ULong` をこれらの型のいずれかに変換できます。

- **文字を入力します。** リテラルに `UL` リテラル型文字を追加すると、`ULong` データ型に強制されます。 `ULong` に識別子の型文字がありません。

- **フレームワークの種類。** .NET Framework において対応する型は、<xref:System.UInt64?displayProperty=nameWithType> 構造体です。

## <a name="see-also"></a>参照

- <xref:System.UInt64>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [方法 : 符号なしの型を使用する Windows の機能を呼び出す](../../../visual-basic/programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
