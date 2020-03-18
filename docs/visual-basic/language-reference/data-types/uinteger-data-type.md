---
title: UInteger 型
ms.date: 01/31/2018
f1_keywords:
- vb.uinteger
helpviewer_keywords:
- numbers [Visual Basic], whole
- UInteger data type
- literal type characters [Visual Basic], UI
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- integers [Visual Basic], types
- UI literal type characters [Visual Basic]
- data types [Visual Basic], integral
ms.assetid: db7ddd34-4f23-46f5-84dd-8b0f83bb8729
ms.openlocfilehash: ccff61608aed797734cb4f5ca0571d7ed73ba98b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343900"
---
# <a name="uinteger-data-type"></a>UInteger データ型

0 ~ 4294967295 の値の範囲内で、符号なし32ビット (4 バイト) の整数を保持します。

## <a name="remarks"></a>コメント

`UInteger` データ型は、最も効率的なデータ幅で最も大きな符号なしの値を提供します。

`UInteger` の既定値は 0 です。

## <a name="literal-assignments"></a>リテラルの代入

`UInteger` 変数は、10進リテラル、16進リテラル、8進数リテラル、または (Visual Basic 2017 で始まる) バイナリリテラルを割り当てることによって、宣言および初期化できます。 整数リテラルが `UInteger` の範囲外にある場合 (つまり、<xref:System.UInt32.MinValue?displayProperty=nameWithType> より小さいか、<xref:System.UInt32.MaxValue?displayProperty=nameWithType> より大きい場合)、コンパイル エラーが発生します。

次の例では、整数 3,000,000,000 を 10 進リテラル、16 進リテラル、バイナリ リテラルで表したものが、`UInteger` 値に割り当てられています。

[!code-vb[UInteger](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#UInt)]

> [!NOTE]
> プレフィックス `&h` または `&H` を使用して、16進リテラル、プレフィックス `&b` または `&B` がバイナリリテラルを示すようにし、プレフィックス `&o` または `&O` を使用して8進数リテラルを表します。 10 進リテラルには、プレフィックスはありません。

Visual Basic 2017 以降では、次の例に示すように、アンダースコア文字 (`_`) を桁区切り記号として使用して、読みやすくすることもできます。

[!code-vb[UInteger](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#UIntS)]

Visual Basic 15.5 以降では、アンダースコア文字 (`_`) をプレフィックスと16進数、バイナリ、または8進数の間の先頭の区切り記号として使用することもできます。 例 :

```vb
Dim number As UInteger = &H_0F8C_0326
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

数値リテラルには、次の例に示すように、`UInteger` データ型を示す `UI` または `ui`[型の文字](../../programming-guide/language-features/data-types/type-characters.md)を含めることもできます。

```vb
Dim number = &H_0FAC_14D7ui
```

## <a name="programming-tips"></a>プログラミングのヒント

`UInteger` データ型および `Integer` データ型は、32ビットプロセッサで最適なパフォーマンスを提供します。これは、使用するビット数が少ない場合でも、より少ない整数型 (`UShort`、`Short`、`Byte`、および `SByte`) であるためです。そのため、読み込み、格納、およびフェッチにかかる時間が長くなります。

- **負の数値。** `UInteger` は符号なしの型であるため、負の数を表すことはできません。 `UInteger`型に評価される式に対して単項マイナス記号 (`-`) 演算子を使用すると、Visual Basic 式が最初に `Long` に変換されます。

- **CLS 準拠。** `UInteger` のデータ型は[共通言語仕様](https://www.ecma-international.org/publications/standards/Ecma-335.htm)(cls) の一部ではないため、cls 準拠のコードはそれを使用するコンポーネントを使用できません。

- **相互運用に関する考慮事項。** .NET Framework 用に作成されていないコンポーネント (オートメーションや COM オブジェクトなど) とやり取りしている場合は、`uint` などの型が他の環境で異なるデータ幅 (16 ビット) を持つ可能性があることに注意してください。 このようなコンポーネントに16ビットの引数を渡す場合は、マネージ Visual Basic コードで `UInteger` ではなく、`UShort` として宣言します。

- **広げ.** `UInteger` のデータ型は、`Long`、`ULong`、`Decimal`、`Single`、および `Double`に拡大変換されます。 つまり、<xref:System.OverflowException?displayProperty=nameWithType> エラーが発生することなく、`UInteger` をこれらの型のいずれかに変換できます。

- **文字を入力します。** リテラルに `UI` リテラル型文字を追加すると、`UInteger` データ型に強制されます。 `UInteger` に識別子の型文字がありません。

- **フレームワークの種類。** .NET Framework において対応する型は、<xref:System.UInt32?displayProperty=nameWithType> 構造体です。

## <a name="see-also"></a>参照

- <xref:System.UInt32>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [方法 : 符号なしの型を使用する Windows の機能を呼び出す](../../../visual-basic/programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
