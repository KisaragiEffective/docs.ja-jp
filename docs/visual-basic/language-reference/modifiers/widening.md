---
title: Widening
ms.date: 07/20/2015
f1_keywords:
- vb.widening
helpviewer_keywords:
- conversions [Visual Basic], type
- type conversion [Visual Basic]
- conversions [Visual Basic], data type
- Widening keyword [Visual Basic]
- data type conversion [Visual Basic]
ms.assetid: 646ae263-94d3-40a2-b0cc-64f619292f56
ms.openlocfilehash: 1c9aa78549ca6e41c9fe54c12e0aaec8e7cc30cb
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347831"
---
# <a name="widening-visual-basic"></a>Widening (Visual Basic)
変換演算子 (`CType`) が、クラスまたは構造体を、元のクラスまたは構造体のすべての可能な値を保持できる型に変換することを示します。  
  
## <a name="converting-with-the-widening-keyword"></a>拡大キーワードを使用した変換  
 変換プロシージャでは、`Widening`に加えて `Public Shared` を指定する必要があります。  
  
 拡大変換は実行時に常に成功し、データ損失は発生しません。 例として、`Double`、`Char` `String`、および派生型から基本型への `Single` があります。 この最後の変換は、派生型に基本型のすべてのメンバーが含まれているため、拡張されます。したがって、は基本型のインスタンスです。  
  
 `Option Strict` が `On`場合でも、使用しているコードでは、拡大変換に `CType` を使用する必要はありません。  
  
 このコンテキストでは、`Widening` キーワードを使用できます。  
  
 [Operator ステートメント](../../../visual-basic/language-reference/statements/operator-statement.md)  
  
 拡大変換と縮小変換演算子の定義の例については、「[方法: 変換演算子を定義する](../../../visual-basic/programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)」を参照してください。

  
## <a name="see-also"></a>参照

- [Operator ステートメント](../../../visual-basic/language-reference/statements/operator-statement.md)
- [Narrowing](../../../visual-basic/language-reference/modifiers/narrowing.md)
- [拡大変換と縮小変換](../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [方法 : 演算子を定義する](../../../visual-basic/programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [CType 関数](../../../visual-basic/language-reference/functions/ctype-function.md)
- [Option Strict ステートメント](../../../visual-basic/language-reference/statements/option-strict-statement.md)
- [方法 : 変換演算子を定義する](../../../visual-basic/programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)
