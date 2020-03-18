---
title: UShort 型
ms.date: 01/31/2018
f1_keywords:
- vb.ushort
helpviewer_keywords:
- numbers [Visual Basic], whole
- literal type characters [Visual Basic], US
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- integers [Visual Basic], types
- data types [Visual Basic], integral
- UShort data type
- US literal type characters [Visual Basic]
ms.assetid: 138db892-665d-4ba8-9cae-d8d91c4a8f39
ms.openlocfilehash: 7cdbd5fb192fd5cc1be6260dcdcdb1f30cf3f865
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343850"
---
# <a name="ushort-data-type-visual-basic"></a>UShort データ型 (Visual Basic)

0 ~ 65535 の値の範囲内の符号なし16ビット (2 バイト) 整数を保持します。  
  
## <a name="remarks"></a>コメント

 `Byte`に対して大きすぎるバイナリデータを格納するには、`UShort` データ型を使用します。  
  
 `UShort` の既定値は 0 です。  

## <a name="literal-assignments"></a>リテラルの代入

`UShort` 変数は、10進リテラル、16進リテラル、8進数リテラル、または (Visual Basic 2017 で始まる) バイナリリテラルを割り当てることによって、宣言および初期化できます。 整数リテラルが `UShort` の範囲外にある場合 (つまり、<xref:System.UInt16.MinValue?displayProperty=nameWithType> より小さいか、<xref:System.UInt16.MaxValue?displayProperty=nameWithType> より大きい場合)、コンパイル エラーが発生します。

次の例では、整数65034を10進リテラル、16進リテラル、バイナリリテラルで表したものが `UShort` 値に割り当てられています。
  
[!code-vb[UShort](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#UShort)]

> [!NOTE]
> プレフィックス `&h` または `&H` を使用して、16進リテラル、プレフィックス `&b` または `&B` がバイナリリテラルを示すようにし、プレフィックス `&o` または `&O` を使用して8進数リテラルを表します。 10 進リテラルには、プレフィックスはありません。

Visual Basic 2017 以降では、次の例に示すように、アンダースコア文字 (`_`) を桁区切り記号として使用して、読みやすくすることもできます。

[!code-vb[UShort](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#UShortS)]

Visual Basic 15.5 以降では、アンダースコア文字 (`_`) をプレフィックスと16進数、バイナリ、または8進数の間の先頭の区切り記号として使用することもできます。 例 :

```vb
Dim number As UShort = &H_FF8C
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

数値リテラルには、次の例に示すように、`UShort` データ型を示す `US` または `us`[型の文字](../../programming-guide/language-features/data-types/type-characters.md)を含めることもできます。

```vb
Dim number = &H_5826us
```

## <a name="programming-tips"></a>プログラミングのヒント
  
- **負の数値。** `UShort` は符号なしの型であるため、負の数を表すことはできません。 `UShort`型に評価される式に対して単項マイナス記号 (`-`) 演算子を使用すると、Visual Basic 式が最初に `Integer` に変換されます。  
  
- **CLS 準拠。** `UShort` のデータ型は[共通言語仕様](https://www.ecma-international.org/publications/standards/Ecma-335.htm)(cls) の一部ではないため、cls 準拠のコードはそれを使用するコンポーネントを使用できません。
  
- **広げ.** `UShort` のデータ型は、`Integer`、`UInteger`、`Long`、`ULong`、`Decimal`、`Single`、`Double`に拡大変換されます。 つまり、<xref:System.OverflowException?displayProperty=nameWithType> エラーが発生することなく、`UShort` をこれらの型のいずれかに変換できます。  
  
- **文字を入力します。** リテラルに `US` リテラル型文字を追加すると、`UShort` データ型に強制されます。 `UShort` に識別子の型文字がありません。  
  
- **フレームワークの種類。** .NET Framework において対応する型は、<xref:System.UInt16?displayProperty=nameWithType> 構造体です。  
  
## <a name="see-also"></a>参照

- <xref:System.UInt16>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [方法 : 符号なしの型を使用する Windows の機能を呼び出す](../../../visual-basic/programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
