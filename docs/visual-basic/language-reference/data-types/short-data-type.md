---
title: Short 型
ms.date: 01/31/2018
f1_keywords:
- vb.Short
helpviewer_keywords:
- numbers [Visual Basic], whole
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- integers [Visual Basic], types
- data types [Visual Basic], integral
- S literal type character [Visual Basic]
- Short data type
- literal type characters [Visual Basic], S
ms.assetid: 65fcbcf3-a841-400e-885e-301497729a8b
ms.openlocfilehash: 8dfdfb56de32e4b3a96729b09ccf46a6fee9a424
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343937"
---
# <a name="short-data-type-visual-basic"></a>Short データ型 (Visual Basic)

-32768 ~ 32767 の範囲の値を範囲とする符号付き16ビット (2 バイト) 整数を保持します。  
  
## <a name="remarks"></a>コメント  

 `Integer`の完全なデータ幅を必要としない整数値を格納するには、`Short` データ型を使用します。 場合によっては、共通言語ランタイムが `Short` 変数をまとめてパックし、メモリ消費量を節約できます。  
  
 `Short` の既定値は 0 です。  
  
## <a name="literal-assignments"></a>リテラルの代入

`Short` 変数は、10進リテラル、16進リテラル、8進数リテラル、または (Visual Basic 2017 で始まる) バイナリリテラルを割り当てることによって、宣言および初期化できます。 整数リテラルが `Short` の範囲外にある場合 (つまり、<xref:System.Int16.MinValue?displayProperty=nameWithType> より小さいか、<xref:System.Int16.MaxValue?displayProperty=nameWithType> より大きい場合)、コンパイル エラーが発生します。

次の例では、整数1034を10進リテラル、16進リテラル、バイナリリテラルで表したものが、[整数](integer-data-type.md)から `Short` 値に暗黙的に変換されています。

[!code-vb[Short](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#Short)]

> [!NOTE]
> プレフィックス `&h` または `&H` を使用して、16進リテラル、プレフィックス `&b` または `&B` がバイナリリテラルを示すようにし、プレフィックス `&o` または `&O` を使用して8進数リテラルを表します。 10 進リテラルには、プレフィックスはありません。

Visual Basic 2017 以降では、次の例に示すように、アンダースコア文字 (`_`) を桁区切り記号として使用して、読みやすくすることもできます。

[!code-vb[Short](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#ShortS)]

Visual Basic 15.5 以降では、アンダースコア文字 (`_`) をプレフィックスと16進数、バイナリ、または8進数の間の先頭の区切り記号として使用することもできます。 例 :

```vb
Dim number As Short = &H_3264
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

数値リテラルには、次の例に示すように、`Short` データ型を示す `S`[型文字](../../programming-guide/language-features/data-types/type-characters.md)を含めることもできます。

```vb
Dim number = &H_3264S
```

## <a name="programming-tips"></a>プログラミングのヒント

- **広げ.** `Short` のデータ型は、`Integer`、`Long`、`Decimal`、`Single`、または `Double`に拡大変換されます。 これは、`Short` エラーを発生させることなく、これらの型のいずれかに <xref:System.OverflowException?displayProperty=nameWithType> を変換できることを意味します。  
  
- **文字を入力します。** あるリテラルにリテラルの型文字 `S` を付けると、そのリテラルは `Short` に変換されます。 `Short` に識別子の型文字がありません。  
  
- **フレームワークの種類。** .NET Framework において対応する型は、<xref:System.Int16?displayProperty=nameWithType> 構造体です。  
  
## <a name="see-also"></a>参照

- <xref:System.Int16?displayProperty=nameWithType>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [Integer データ型](../../../visual-basic/language-reference/data-types/integer-data-type.md)
- [Long データ型](../../../visual-basic/language-reference/data-types/long-data-type.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
