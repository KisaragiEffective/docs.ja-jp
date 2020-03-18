---
title: 変更できる引数と変更できない引数の違い
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedure arguments
- arguments [Visual Basic], nonmodifiable
- Visual Basic code, procedures
- arguments [Visual Basic], modifiable
ms.assetid: 87b2df69-e1f7-4657-9caf-b3f48d693428
ms.openlocfilehash: 989795ee2cdd3a78b71bad4d95cf9b384c2719bd
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74341387"
---
# <a name="differences-between-modifiable-and-nonmodifiable-arguments-visual-basic"></a>変更できる引数と変更できない引数の違い (Visual Basic)
プロシージャを呼び出す場合、通常は1つ以上の引数を渡します。 各引数は、基になるプログラミング要素に対応します。 基になる要素と引数自体の両方を変更可能または変更できません。  
  
## <a name="modifiable-and-nonmodifiable-elements"></a>変更可能な要素と変更不可能な要素  
 プログラミング要素には、変更可能な*要素*(値を変更することができます)、または作成後に固定値を持つ変更でき*ない要素の*いずれかを指定できます。  
  
 次の表に、変更可能なプログラミング要素と変更できないプログラミング要素の一覧を示します。  
  
|変更可能な要素|不変の要素|  
|-------------------------|----------------------------|  
|ローカル変数 (プロシージャ内で宣言されます)。読み取り専用を除き、オブジェクト変数を含みます。|読み取り専用の変数、フィールド、およびプロパティ|  
|フィールド (モジュール、クラス、および構造体のメンバー変数) (読み取り専用を除く)|定数とリテラル|  
|プロパティ (読み取り専用を除く)|列挙型のメンバー|  
|配列要素|式 (要素が変更可能な場合でも)|  
  
## <a name="modifiable-and-nonmodifiable-arguments"></a>変更可能な引数と変更不可能な引数  
 変更可能な*引数*は、変更可能な基になる要素を持つものです。 呼び出し元のコードは、新しい値をいつでも格納できます。引数[ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)を渡すと、プロシージャ内のコードは、呼び出し元のコード内の基になる要素を変更することもできます。  
  
 *不変の引数*は、基になる要素が不変であるか、または[ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)で渡されています。 プロシージャは、変更可能な要素であっても、呼び出し元のコード内の基になる要素を変更することはできません。 変更できない要素の場合は、呼び出し元のコード自体が変更することはできません。  
  
 呼び出されたプロシージャは、変更できない引数のローカルコピーを変更する場合がありますが、その変更は呼び出し元のコードの基になる要素に影響しません。  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [方法: プロシージャに引数を渡す](./how-to-pass-arguments-to-a-procedure.md)
- [引数の値渡しと参照渡し](./passing-arguments-by-value-and-by-reference.md)
- [引数の値渡しと参照渡しの違い](./differences-between-passing-an-argument-by-value-and-by-reference.md)
- [方法: プロシージャ引数の値を変更する](./how-to-change-the-value-of-a-procedure-argument.md)
- [方法: プロシージャ引数の値が変化しないようにする](./how-to-protect-a-procedure-argument-against-value-changes.md)
- [方法: 引数の値渡しを強制する](./how-to-force-an-argument-to-be-passed-by-value.md)
- [位置と名前による引数渡し](./passing-arguments-by-position-and-by-name.md)
- [値型と参照型](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
