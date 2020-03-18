---
title: SByte 型
ms.date: 04/20/2017
f1_keywords:
- vb.sbyte
helpviewer_keywords:
- numbers [Visual Basic], whole
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- integers [Visual Basic], types
- data types [Visual Basic], integral
- SByte data type
ms.assetid: 5c38374a-18a1-4cc2-b493-299e3dcaa60f
ms.openlocfilehash: 01a0a4a261213d7e6e2016bf49128092e5b22308
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343950"
---
# <a name="sbyte-data-type-visual-basic"></a>SByte データ型 (Visual Basic)

-128 ~ 127 の範囲の値を範囲とする、符号付き8ビット (1 バイト) の整数を保持します。

## <a name="remarks"></a>コメント

`SByte` データ型を使用して、`Integer` の完全なデータ幅または `Short`の半分のデータ幅を必要としない整数値を格納します。 場合によっては、共通言語ランタイムが `SByte` 変数をまとめてパックし、メモリ消費量を節約できることがあります。

`SByte` の既定値は 0 です。

## <a name="literal-assignments"></a>リテラルの代入

`SByte` 変数は、10進リテラル、16進リテラル、8進数リテラル、または (Visual Basic 2017 で始まる) バイナリリテラルを割り当てることによって、宣言および初期化できます。

次の例では、10進リテラル、16進リテラル、バイナリリテラルとして表される-102 と等しい整数が `SByte` 値に割り当てられています。 この例では、`/removeintchecks` コンパイラスイッチを使用してコンパイルする必要があります。

[!code-vb[SByte](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#SByte)]

> [!NOTE]
> プレフィックス `&h` または `&H` を使用して、16進リテラル、プレフィックス `&b` または `&B` がバイナリリテラルを示すようにし、プレフィックス `&o` または `&O` を使用して8進数リテラルを表します。 10 進リテラルには、プレフィックスはありません。

Visual Basic 2017 以降では、次の例に示すように、アンダースコア文字 (`_`) を桁区切り記号として使用して、読みやすくすることもできます。

[!code-vb[SByteSeparator](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#SByteS)]

Visual Basic 15.5 以降では、アンダースコア文字 (`_`) をプレフィックスと16進数、バイナリ、または8進数の間の先頭の区切り記号として使用することもできます。 例 :

```vb
Dim number As SByte = &H_F9
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

整数リテラルが `SByte` の範囲外にある場合 (つまり、<xref:System.SByte.MinValue?displayProperty=nameWithType> より小さいか、<xref:System.SByte.MaxValue?displayProperty=nameWithType> より大きい場合)、コンパイル エラーが発生します。 整数リテラルにサフィックスがない場合、[整数](integer-data-type.md)が推論されます。 整数リテラルが `Integer` 型の範囲外の場合は、 [Long](long-data-type.md)が推定されます。 これは、前の例では、`0x9A` と `0b10011010` の数値リテラルが32ビット符号付き整数として解釈されることを意味し、156は <xref:System.SByte.MaxValue?displayProperty=nameWithType>を超えています。 10進数以外の整数を `SByte`に割り当てる次のようなコードを正常にコンパイルするには、次のいずれかの操作を行います。

- `/removeintchecks` コンパイラスイッチを使用してコンパイルすることにより、整数範囲のチェックを無効にします。

- `SByte`に割り当てるリテラル値を明示的に定義するには、[型文字](../../programming-guide/language-features/data-types/type-characters.md)を使用します。 次の例では、負のリテラル `Short` 値を `SByte`に割り当てます。 負の数値の場合は、数値リテラルの上位ワードの上位ビットを設定する必要があることに注意してください。 この例の場合、これはリテラル `Short` 値のビット15です。

   [!code-vb[SByteTypeChars](../../../../samples/snippets/visualbasic/language-reference/data-types/sbyte-assignment.vb#1)]

## <a name="programming-tips"></a>プログラミングのヒント

- **CLS 準拠。** `SByte` のデータ型は[共通言語仕様](https://www.ecma-international.org/publications/standards/Ecma-335.htm)(cls) の一部ではないため、cls 準拠のコードはそれを使用するコンポーネントを使用できません。

- **広げ.** `SByte` のデータ型は、`Short`、`Integer`、`Long`、`Decimal`、`Single`、および `Double`に拡大変換されます。 つまり、<xref:System.OverflowException?displayProperty=nameWithType> エラーが発生することなく、`SByte` をこれらの型のいずれかに変換できます。

- **文字を入力します。** `SByte` には、リテラルの型文字または識別子の型文字がありません。

- **フレームワークの種類。** .NET Framework において対応する型は、<xref:System.SByte?displayProperty=nameWithType> 構造体です。

## <a name="see-also"></a>参照

- <xref:System.SByte?displayProperty=nameWithType>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [Short データ型](../../../visual-basic/language-reference/data-types/short-data-type.md)
- [Integer データ型](../../../visual-basic/language-reference/data-types/integer-data-type.md)
- [Long データ型](../../../visual-basic/language-reference/data-types/long-data-type.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
