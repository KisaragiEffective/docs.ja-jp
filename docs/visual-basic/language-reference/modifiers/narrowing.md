---
title: Narrowing
ms.date: 07/20/2015
f1_keywords:
- vb.narrowing
helpviewer_keywords:
- conversions [Visual Basic], type
- type conversion [Visual Basic]
- conversions [Visual Basic], data type
- Narrowing keyword [Visual Basic]
- data type conversion [Visual Basic]
ms.assetid: a207ee91-aca4-4771-b4e2-713f029bf2bb
ms.openlocfilehash: b252f7939e812f31103d4bd98ffd50953679f042
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351476"
---
# <a name="narrowing-visual-basic"></a>Narrowing (Visual Basic)
変換演算子 (`CType`) が、クラスまたは構造体を、元のクラスまたは構造体の使用可能な値の一部を保持できない可能性のある型に変換することを示します。  
  
## <a name="converting-with-the-narrowing-keyword"></a>縮小キーワードを使用した変換  
 変換プロシージャでは、`Narrowing`に加えて `Public Shared` を指定する必要があります。  
  
 縮小変換は実行時に必ず成功するわけではないため、失敗したり、データ損失が発生したりする可能性があります。 例として、`Integer`、`String` `Date`、および基本型から派生型への `Long` があります。 基本型には派生型のすべてのメンバーが含まれていないため、派生型のインスタンスではないため、この最後の変換は縮小変換です。  
  
 `Option Strict` が `On`場合、使用しているコードはすべての縮小変換に `CType` を使用する必要があります。  
  
 このコンテキストでは、`Narrowing` キーワードを使用できます。  
  
 [Operator ステートメント](../../../visual-basic/language-reference/statements/operator-statement.md)  
  
## <a name="see-also"></a>参照

- [Operator ステートメント](../../../visual-basic/language-reference/statements/operator-statement.md)
- [Widening](../../../visual-basic/language-reference/modifiers/widening.md)
- [拡大変換と縮小変換](../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [方法 : 演算子を定義する](../../../visual-basic/programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [CType 関数](../../../visual-basic/language-reference/functions/ctype-function.md)
- [Option Strict ステートメント](../../../visual-basic/language-reference/statements/option-strict-statement.md)
