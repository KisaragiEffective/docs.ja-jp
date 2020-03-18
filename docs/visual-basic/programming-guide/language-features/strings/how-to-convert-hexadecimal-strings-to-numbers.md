---
title: '方法 : 16 進文字列を数値に変換する'
ms.date: 01/31/2018
helpviewer_keywords:
- numbers [Visual Basic], hexadecimals
- hexadecimals [Visual Basic], decimals
- examples [Visual Basic], string conversion
- decimals [Visual Basic], hexadecimals
- string conversion [Visual Basic], hexadecimal to numbers
ms.assetid: 76675807-eadb-4c08-bd50-e6c6ff4b8ced
ms.openlocfilehash: f0a97a0c212a64bfa4db4606ee526b666f07877a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347172"
---
# <a name="how-to-convert-hexadecimal-strings-to-numbers-visual-basic"></a>方法: 16 進文字列を数値に変換する (Visual Basic)

この例では、<xref:System.Convert.ToInt32%2A?displayProperty=nameWithType> メソッドを使用して、16進数の文字列を整数に変換します。

## <a name="to-convert-a-hexadecimal-string-to-a-number"></a>16進数の文字列を数値に変換するには

- Base-16 で表現された数値を整数に変換するには、<xref:System.Convert.ToInt32(System.String,System.Int32)> メソッドを使用します。

  <xref:System.Convert.ToInt32(System.String,System.Int32)> メソッドの最初の引数は、変換する文字列です。 2番目の引数は、数値がどのように表されるかを示します。16進数は base 16 です。

  [!code-vb[VbVbalrStrings#62](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#62)]

- 16進数の文字列には次の制限があることに注意してください。

  - `&h` プレフィックスを含めることはできません。
  - `_` 桁区切り記号を含めることはできません。

  プレフィックスまたは桁区切り記号がある場合、<xref:System.Convert.ToInt32(System.String,System.Int32)> メソッドを呼び出すと、<xref:System.FormatException>がスローされます。

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Conversion.Hex%2A>
- <xref:System.Convert.ToInt32%2A?displayProperty=nameWithType>
