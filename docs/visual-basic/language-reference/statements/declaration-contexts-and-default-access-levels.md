---
title: 宣言コンテキストと既定のアクセス レベル
ms.date: 07/20/2015
helpviewer_keywords:
- module level, defined
- declaration contexts, Visual Basic
- procedure level, defined
- namespace level, defined
- access levels, Visual Basic
- access levels, default levels
ms.assetid: bf63b96e-e825-4745-88c8-5dae222728db
ms.openlocfilehash: 1ba25d830b1e7529bdf09c1195cc1fe7f9b2243b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354103"
---
# <a name="declaration-contexts-and-default-access-levels-visual-basic"></a>宣言コンテキストと既定のアクセス レベル (Visual Basic)
このトピックでは、他の型を使用して宣言できる Visual Basic 型と、指定されていない場合のアクセスレベルの既定値について説明します。  
  
## <a name="declaration-context-levels"></a>宣言コンテキストレベル  
 プログラミング要素の*宣言コンテキスト*は、そのプログラミング要素が宣言されているコードの領域です。 多くの場合、これは別のプログラミング要素であり、それを含んでいる*要素*と呼びます。  
  
 宣言コンテキストのレベルは次のとおりです。  
  
- *名前空間レベル*-ソースファイルまたは名前空間内で、クラス、構造体、モジュール、またはインターフェイス内ではありません。  
  
- *モジュールレベル*: クラス、構造体、モジュール、またはインターフェイス内で、プロシージャまたはブロック内には含まれません。  
  
- プロシージャ*レベル*-プロシージャまたはブロック内 (`If` や `For`など)  
  
 宣言コンテキストに応じて、宣言されたさまざまなプログラミング要素の既定のアクセスレベルを次の表に示します。  
  
|宣言された要素|名前空間レベル|モジュールレベル|プロシージャレベル|  
|----------------------|---------------------|------------------|---------------------|  
|変数 ([Dim ステートメント](../../../visual-basic/language-reference/statements/dim-statement.md))|使用不可|`Private` (`Structure`で`Public`、`Interface`では使用できません)|`Public`|  
|定数 ([Const ステートメント](../../../visual-basic/language-reference/statements/const-statement.md))|使用不可|`Private` (`Structure`で`Public`、`Interface`では使用できません)|`Public`|  
|列挙型 ([Enum ステートメント](../../../visual-basic/language-reference/statements/enum-statement.md))|`Friend`|`Public`|使用不可|  
|クラス ([Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md))|`Friend`|`Public`|使用不可|  
|構造体 ([Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md))|`Friend`|`Public`|使用不可|  
|モジュール ([Module ステートメント](../../../visual-basic/language-reference/statements/module-statement.md))|`Friend`|使用不可|使用不可|  
|インターフェイス ([Interface ステートメント](../../../visual-basic/language-reference/statements/interface-statement.md))|`Friend`|`Public`|使用不可|  
|プロシージャ ([Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)、 [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md))|使用不可|`Public`|使用不可|  
|外部参照 ([Declare ステートメント](../../../visual-basic/language-reference/statements/declare-statement.md))|使用不可|`Public` (`Interface`では許可されていません)|使用不可|  
|演算子 ([Operator ステートメント](../../../visual-basic/language-reference/statements/operator-statement.md))|使用不可|`Public` (`Interface` または `Module`では許可されていません)|使用不可|  
|プロパティ ([Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md))|使用不可|`Public`|使用不可|  
|既定のプロパティ ([Default](../../../visual-basic/language-reference/modifiers/default.md))|使用不可|`Public` (`Module`では許可されていません)|使用不可|  
|イベント ([Event ステートメント](../../../visual-basic/language-reference/statements/event-statement.md))|使用不可|`Public`|使用不可|  
|デリゲート ([Delegate ステートメント](../../../visual-basic/language-reference/statements/delegate-statement.md))|`Friend`|`Public`|使用不可|  
  
 詳細については、「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [Friend](../../../visual-basic/language-reference/modifiers/friend.md)
- [Private](../../../visual-basic/language-reference/modifiers/private.md)
- [Public](../../../visual-basic/language-reference/modifiers/public.md)
