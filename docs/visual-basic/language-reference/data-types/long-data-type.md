---
title: Long 型
ms.date: 01/31/2018
f1_keywords:
- vb.Long
helpviewer_keywords:
- identifier type characters [Visual Basic], &
- numbers [Visual Basic], whole
- whole numbers
- integral data types [Visual Basic]
- '& identifier type character'
- integer numbers
- literal type characters [Visual Basic], L
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- L literal type character [Visual Basic]
- integers [Visual Basic], types
- Long keyword [Visual Basic]
- data types [Visual Basic], integral
- data types [Visual Basic], assigning
- Long data type
ms.assetid: b4770c34-1804-4f8c-b512-c10b0893e516
ms.openlocfilehash: 16d7409c802e97b1f33474d810134db4d9f0ad6c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343972"
---
# <a name="long-data-type-visual-basic"></a>Long データ型 (Visual Basic)

-9223372036854775808 ~ 9223372036854775807 (9.2... E + 18) の値の範囲内で、64ビット (8 バイト) の符号付き整数を保持します。

## <a name="remarks"></a>コメント

`Long` データ型を使用して、`Integer` データ型には大きすぎる整数値を格納します。

`Long` の既定値は 0 です。

## <a name="literal-assignments"></a>リテラルの代入

`Long` 変数は、10進リテラル、16進リテラル、8進数リテラル、または (Visual Basic 2017 で始まる) バイナリリテラルを割り当てることによって、宣言および初期化できます。 整数リテラルが `Long` の範囲外にある場合 (つまり、<xref:System.Int64.MinValue?displayProperty=nameWithType> より小さいか、<xref:System.Int64.MaxValue?displayProperty=nameWithType> より大きい場合)、コンパイル エラーが発生します。

次の例では、整数 4,294,967,296 を 10 進リテラル、16 進リテラル、バイナリ リテラルで表したものが、`Long` 値に割り当てられています。

[!code-vb[long](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#Long)]

> [!NOTE]
> プレフィックス `&h` または `&H` を使用して、16進リテラル、プレフィックス `&b` または `&B` がバイナリリテラルを示すようにし、プレフィックス `&o` または `&O` を使用して8進数リテラルを表します。 10 進リテラルには、プレフィックスはありません。

Visual Basic 2017 以降では、次の例に示すように、アンダースコア文字 (`_`) を桁区切り記号として使用して、読みやすくすることもできます。

[!code-vb[long](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#LongS)]

Visual Basic 15.5 以降では、アンダースコア文字 (`_`) をプレフィックスと16進数、バイナリ、または8進数の間の先頭の区切り記号として使用することもできます。 次に例を示します。

```vb
Dim number As Long = &H_0FAC_0326_1489_D68C
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

数値リテラルには、次の例に示すように、`Long` データ型を示す `L`[型文字](../../programming-guide/language-features/data-types/type-characters.md)を含めることもできます。

```vb
Dim number = &H_0FAC_0326_1489_D68CL
```

## <a name="programming-tips"></a>プログラミングのヒント

- **相互運用に関する考慮事項。** .NET Framework 用に作成されていないコンポーネント (たとえば、オートメーションや COM オブジェクト) とやり取りする場合は、他の環境で `Long` が異なるデータ幅 (32 ビット) を持っていることに注意してください。 このようなコンポーネントに32ビットの引数を渡す場合は、新しい Visual Basic コードで `Long` の代わりに `Integer` として宣言します。

- **広げ.** `Long` のデータ型は、`Decimal`、`Single`、または `Double`に拡大変換されます。 これは、`Long` エラーを発生させることなく、これらの型のいずれかに <xref:System.OverflowException?displayProperty=nameWithType> を変換できることを意味します。

- **文字を入力します。** あるリテラルにリテラルの型文字 `L` を付けると、そのリテラルは `Long` に変換されます。 ある識別子に識別子の型文字 `&` を付けると、その識別子は整数型 (`Long`) に変換されます。

- **Framework のデータ型。** .NET Framework において対応する型は、<xref:System.Int64?displayProperty=nameWithType> 構造体です。

## <a name="see-also"></a>関連項目

- <xref:System.Int64>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [Integer データ型](../../../visual-basic/language-reference/data-types/integer-data-type.md)
- [Short データ型](../../../visual-basic/language-reference/data-types/short-data-type.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
