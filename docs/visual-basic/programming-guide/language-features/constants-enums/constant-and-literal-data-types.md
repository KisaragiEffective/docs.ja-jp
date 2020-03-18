---
title: 定数とリテラルのデータ型
ms.date: 07/20/2015
helpviewer_keywords:
- declaring constants [Visual Basic], literal data types
- data types [Visual Basic], declaring
- constants [Visual Basic], declaring
- explicit declarations
- literals [Visual Basic], coercing data type
- declarations [Visual Basic], data types
ms.assetid: 057206d2-3a5b-40b9-b3af-57446f9b52fa
ms.openlocfilehash: 8ebecddfab0724023c269e92c1fc5534f096bf1c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74333725"
---
# <a name="constant-and-literal-data-types-visual-basic"></a>定数とリテラルのデータ型 (Visual Basic)
リテラルは、変数の値または式の結果 (数値3や文字列 "Hello" など) としてではなく、単独で表現される値です。 定数は、リテラルの代わりとなる意味のある名前で、値が変更される可能性がある変数とは対照的に、プログラム全体で同じ値を保持します。  
  
 [オプションの推論](../../../../visual-basic/language-reference/statements/option-infer-statement.md)が `Off`、 [option Strict](../../../../visual-basic/language-reference/statements/option-strict-statement.md)が `On`場合、データ型を使用してすべての定数を明示的に宣言する必要があります。 次の例では、`MyByte` のデータ型がデータ型 `Byte`として明示的に宣言されています。  
  
 [!code-vb[VbVbalrConstants#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConstants/VB/Class1.vb#1)]  
  
 `Option Infer` が `On` または `Option Strict` が `Off`場合は、`As` 句を使用してデータ型を指定せずに定数を宣言できます。 コンパイラは、式の型から定数の型を特定します。 数値整数リテラルは、既定で `Integer` データ型にキャストされます。 浮動小数点数の既定のデータ型は `Double`であり、キーワード `True` と `False` は `Boolean` 定数を指定します。  
  
## <a name="literals-and-type-coercion"></a>リテラルと型の強制変換  
 場合によっては、リテラルを特定のデータ型に強制的に適用することが必要になることがあります。たとえば、`Decimal`型の変数に特に大きな整数リテラル値を割り当てる場合などです。 次の例では、エラーが生成されます。  
  
```vb  
Dim myDecimal as Decimal  
myDecimal = 100000000000000000000   ' This causes a compiler error.  
```  
  
 このエラーは、リテラルの表現によって発生します。 `Decimal` のデータ型はこの値を大きく保持できますが、リテラルは暗黙的に `Long`として表されます。これはできません。  
  
 リテラルを特定のデータ型に強制的に変換するには、型文字を追加する方法と、それを囲む文字内に配置する方法の2つの方法があります。 型文字または囲み文字は、リテラルの前または後に記述する必要があります。  
  
 前の例を機能させるには、リテラルに `D` 型文字を追加して、それを `Decimal`として表すことができます。  
  
 [!code-vb[VbVbalrConstants#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConstants/VB/Class1.vb#2)]  
  
 次の例は、型文字と囲み文字の正しい使用法を示しています。  
  
 [!code-vb[VbVbalrConstants#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConstants/VB/Class1.vb#3)]  
  
 次の表は、Visual Basic で使用できる囲み文字と型文字を示しています。  
  
|データ型|囲み文字|追加された型文字|  
|---|---|---|  
|`Boolean`|(なし)|(なし)|  
|`Byte`|(なし)|(なし)|  
|`Char`|"|C|  
|`Date`|#|(なし)|  
|`Decimal`|(なし)|D または@|  
|`Double`|(なし)|R または#|  
|`Integer`|(なし)|I または%|  
|`Long`|(なし)|L または &|  
|`Short`|(なし)|S|  
|`Single`|(なし)|F または!|  
|`String`|"|(なし)|  
  
## <a name="see-also"></a>参照

- [ユーザー定義定数](../../../../visual-basic/programming-guide/language-features/constants-enums/user-defined-constants.md)
- [方法 : 定数を宣言する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-declare-a-constant.md)
- [定数の概要](../../../../visual-basic/programming-guide/language-features/constants-enums/constants-overview.md)
- [Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)
- [Option Explicit ステートメント](../../../../visual-basic/language-reference/statements/option-explicit-statement.md)
- [列挙型の概要](../../../../visual-basic/programming-guide/language-features/constants-enums/enumerations-overview.md)
- [方法: 列挙型を宣言する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-declare-enumerations.md)
- [列挙型と名前の修飾](../../../../visual-basic/programming-guide/language-features/constants-enums/enumerations-and-name-qualification.md)
- [データの種類](../../../../visual-basic/language-reference/data-types/index.md)
- [定数と列挙体](../../../../visual-basic/language-reference/constants-and-enumerations.md)
