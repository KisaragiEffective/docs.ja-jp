---
title: Implements 句
ms.date: 07/20/2015
f1_keywords:
- vb.ImplementsClause
helpviewer_keywords:
- interface implementation [Visual Basic], reimplementation
- interface members [Visual Basic], reimplementation
- interface members [Visual Basic], Implements keyword
- interface members
- members [Visual Basic], reimplementation
- interface implementation [Visual Basic], Implements keyword
- interface members [Visual Basic], implementing
- Implements keyword [Visual Basic]
- Implements statement [Visual Basic], about Implements
- members [Visual Basic], implementing
- members [Visual Basic], Implements keyword
- reimplementation
ms.assetid: 5252cdf9-964d-4fc6-af0f-0449b7126b5a
ms.openlocfilehash: f114aee75356e59eafd9d3ba6af9c64402cb374f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345873"
---
# <a name="implements-clause-visual-basic"></a>Implements 句 (Visual Basic)
クラスまたは構造体のメンバーが、インターフェイスで定義されているメンバーの実装を提供していることを示します。  
  
## <a name="remarks"></a>コメント  
`Implements` キーワードは[Implements ステートメント](../../../visual-basic/language-reference/statements/implements-statement.md)と同じではありません。 `Implements` ステートメントを使用して、クラスまたは構造体が1つ以上のインターフェイスを実装することを指定し、各メンバーに対して `Implements` キーワードを使用して、実装するインターフェイスとメンバーを指定します。

クラスまたは構造体がインターフェイスを実装する場合は、[Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md)または [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)の直後に `Implements` ステートメントを含める必要があります。また、インターフェイスで定義されているすべてのメンバーを実装する必要があります。

## <a name="reimplementation"></a>再実装  
派生クラスでは、基底クラスに既に実装されているインターフェイスメンバーを再実装できます。 これは、次の点で基底クラスのメンバーをオーバーライドすることとは異なります。

- 基底クラスのメンバーを再実装するのに、[Overridable](../../../visual-basic/language-reference/modifiers/overridable.md) とする必要はありません。
- 別の名前を使用してメンバーを再実装できます。

`Implements` キーワードは、次のコンテキストで使用できます。

- [Event ステートメント](../../../visual-basic/language-reference/statements/event-statement.md)
- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)
- [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)
- [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)  
  
## <a name="see-also"></a>参照

- [Implements ステートメント](../../../visual-basic/language-reference/statements/implements-statement.md)
- [Interface ステートメント](../../../visual-basic/language-reference/statements/interface-statement.md)
- [Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md)
- [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)
