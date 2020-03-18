---
title: Byte データ型 (Visual Basic)
ms.date: 01/31/2018
f1_keywords:
- vb.Byte
helpviewer_keywords:
- Byte data type
- data types [Visual Basic], assigning
ms.assetid: eed44dff-eaee-4937-a89f-444e418e74f6
ms.openlocfilehash: 347d7e7d0f09e089886bc81bd0be659deaca9b46
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344078"
---
# <a name="byte-data-type-visual-basic"></a>Byte データ型 (Visual Basic)

0 ~ 255 の範囲の値を範囲とする符号なし8ビット (1 バイト) 整数を保持します。

## <a name="remarks"></a>コメント

バイナリデータを格納するには、`Byte` データ型を使用します。  
  
`Byte` の既定値は 0 です。

## <a name="literal-assignments"></a>リテラルの代入

`Byte` 変数は、10進リテラル、16進リテラル、8進数リテラル、または (Visual Basic 2017 で始まる) バイナリリテラルを割り当てることによって、宣言および初期化できます。 整数リテラルが `Byte` の範囲外の場合 (つまり、<xref:System.Byte.MinValue?displayProperty=nameWithType> より小さいか <xref:System.Byte.MaxValue?displayProperty=nameWithType>より大きい場合)、コンパイルエラーが発生します。

次の例では、整数201を10進リテラル、16進リテラル、バイナリリテラルで表したものが、[整数](integer-data-type.md)から `byte` 値に暗黙的に変換されています。

[!code-vb[Byte](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#Byte)]

> [!NOTE]
> プレフィックス `&h` または `&H` を使用して、16進リテラル、プレフィックス `&b` または `&B` がバイナリリテラルを示すようにし、プレフィックス `&o` または `&O` を使用して8進数リテラルを表します。 10 進リテラルには、プレフィックスはありません。

Visual Basic 2017 以降では、次の例に示すように、アンダースコア文字 (`_`) を桁区切り記号として使用して、読みやすくすることもできます。

[!code-vb[Byte](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#ByteS)]  

Visual Basic 15.5 以降では、アンダースコア文字 (`_`) をプレフィックスと16進数、バイナリ、または8進数の間の先頭の区切り記号として使用することもできます。 例 :

```vb
Dim number As Byte = &H_6A
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

## <a name="programming-tips"></a>プログラミングのヒント

- **負の数値。** `Byte` は符号なしの型であるため、負の数を表すことはできません。 `Byte`型に評価される式に対して単項マイナス記号 (`-`) 演算子を使用すると、Visual Basic 式が最初に `Short` に変換されます。
  
- **書式変換。** Visual Basic がファイルの読み取りまたは書き込みを行うとき、または Dll、メソッド、およびプロパティを呼び出すときに、データ形式間で自動的に変換することができます。 `Byte` の変数および配列に格納されているバイナリデータは、そのような形式の変換中に保持されます。 バイナリデータには `String` 変数を使用しないでください。 ANSI 形式と Unicode 形式の間の変換中は、その内容が破損する可能性があります。

- **拡大変換。  **   `Byte` のデータ型は、`Short`、`UShort`、`Integer`、`UInteger`、`Long`、`ULong`、`Decimal`、`Single`、`Double`に拡大変換されます。 つまり、<xref:System.OverflowException?displayProperty=nameWithType> エラーが発生することなく、`Byte` をこれらの型のいずれかに変換できます。
  
- **文字を入力します。** `Byte` には、リテラルの型文字または識別子の型文字がありません。

- **フレームワークの種類。** .NET Framework において対応する型は、<xref:System.Byte?displayProperty=nameWithType> 構造体です。

## <a name="example"></a>例

 次の例では、`b` は `Byte` 変数です。 ステートメントでは、変数の範囲と、それに対するビットシフト演算子の適用を示します。

 [!code-vb[VbVbalrDataTypes#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#16)]  

## <a name="see-also"></a>参照

- <xref:System.Byte?displayProperty=nameWithType>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
