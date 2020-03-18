---
title: Public (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- vb.Public
helpviewer_keywords:
- Public keyword [Visual Basic]
- Public keyword [Visual Basic], syntax
- Public access modifier
ms.assetid: 284c9e1b-ed23-499b-9bc9-ad87c11485a5
ms.openlocfilehash: 35bf1a65e0b8f24a1263adc480719c69b95dff9b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351294"
---
# <a name="public-visual-basic"></a>Public (Visual Basic)
1つ以上の宣言されたプログラミング要素にアクセス制限がないことを指定します。  
  
## <a name="remarks"></a>コメント  
 クラスライブラリなど、コンポーネントまたはコンポーネントのセットを発行する場合は、通常、アセンブリと相互運用できるコードでプログラミング要素にアクセスできるようにする必要があります。 要素に対して無制限のアクセス権を付与するには、`Public`で宣言します。  
  
 パブリックアクセスは、プログラミング要素へのアクセスを制限する必要がない場合の通常のレベルです。 インターフェイス、モジュール、クラス、または構造体で宣言されている要素のアクセスレベルは、既定では、宣言しない場合は `Public` になることに注意してください。  
  
## <a name="rules"></a>ルール  
  
- **宣言コンテキスト。** `Public` は、モジュール、インターフェイス、または名前空間レベルでのみ使用できます。 つまり、`Public` 要素の宣言コンテキストは、ソースファイル、名前空間、インターフェイス、モジュール、クラス、または構造体である必要があり、プロシージャにすることはできません。  
  
## <a name="behavior"></a>動作  
  
- **アクセスレベル。** モジュール、クラス、または構造体にアクセスできるすべてのコードは、その `Public` 要素にアクセスできます。  
  
- **既定のアクセス。** プロシージャ内のローカル変数は、既定でパブリックアクセスに設定されているので、アクセス修飾子を使用することはできません。  
  
- **アクセス修飾子。** アクセスレベルを指定するキーワードは、*アクセス修飾子*と呼ばれます。 アクセス修飾子の比較については、「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。  
  
 `Public` 修飾子は、次のコンテキストで使用できます。  
  
 [Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md)  
  
 [Const ステートメント](../../../visual-basic/language-reference/statements/const-statement.md)  
  
 [Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md)  
  
 [Delegate ステートメント](../../../visual-basic/language-reference/statements/delegate-statement.md)  
  
 [Dim ステートメント](../../../visual-basic/language-reference/statements/dim-statement.md)  
  
 [Enum ステートメント](../../../visual-basic/language-reference/statements/enum-statement.md)  
  
 [Event ステートメント](../../../visual-basic/language-reference/statements/event-statement.md)  
  
 [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)  
  
 [Interface ステートメント](../../../visual-basic/language-reference/statements/interface-statement.md)  
  
 [Module ステートメント](../../../visual-basic/language-reference/statements/module-statement.md)  
  
 [Operator ステートメント](../../../visual-basic/language-reference/statements/operator-statement.md)  
  
 [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)  
  
 [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)  
  
 [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)  
  
## <a name="see-also"></a>関連項目

- [Protected](../../../visual-basic/language-reference/modifiers/protected.md)
- [Friend](../../../visual-basic/language-reference/modifiers/friend.md)
- [Private](../../../visual-basic/language-reference/modifiers/private.md)
- [Private Protected](private-protected.md)
- [Protected Friend](protected-friend.md)
- [Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)
- [Visual Basic におけるプロシージャ](../../../visual-basic/programming-guide/language-features/procedures/index.md)
- [構造体](../../../visual-basic/programming-guide/language-features/data-types/structures.md)
- [クラスとオブジェクト](../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
