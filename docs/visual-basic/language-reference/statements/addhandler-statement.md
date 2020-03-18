---
title: AddHandler ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.AddHandlerMethod
- addhandler
- vb.addhandler
helpviewer_keywords:
- AddHandler statement [Visual Basic]
ms.assetid: cfe69799-2a0f-42c0-a99e-09fed954da01
ms.openlocfilehash: c110116af75d4fb39c016b8d6afcdb707fa6599b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350189"
---
# <a name="addhandler-statement"></a>AddHandler ステートメント
実行時にイベントをイベントハンドラーに関連付けます。  
  
## <a name="syntax"></a>構文  
  
```vb  
AddHandler event, AddressOf eventhandler  
```  
  
## <a name="parts"></a>指定項目  
|||
|---|---|
|イベント|処理するイベントの名前。|  
|`eventhandler`|イベントを処理するプロシージャの名前。|
|||
  
## <a name="remarks"></a>コメント  
 `AddHandler` および `RemoveHandler` ステートメントを使用すると、プログラムの実行中にいつでもイベント処理を開始および停止できます。  
  
 `eventhandler` プロシージャのシグネチャは、イベント `event`の署名と一致している必要があります。  
  

 `Handles` キーワードと `AddHandler` ステートメントはどちらも特定のプロシージャで特定のイベントを処理するように指定できますが、両者には違いがあります。`AddHandler` ステートメントは、実行時にプロシージャをイベントに接続させます。`Handles` キーワードは、プロシージャの定義時に特定のイベントを処理するよう指定する場合に使用します。詳細については、「[Handles 句 (Visual Basic)](../../../visual-basic/language-reference/statements/handles-clause.md)」を参照してください。  

  
> [!NOTE]
> カスタムイベントの場合は、`AddHandler` ステートメントによって、イベントの `AddHandler` アクセサーが呼び出されます。 カスタムイベントの詳細については、「[Event ステートメント](../../../visual-basic/language-reference/statements/event-statement.md)」を参照してください。  
  
## <a name="example"></a>例  
 [!code-vb[VbVbalrEvents#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#17)]  
  
## <a name="see-also"></a>関連項目

- [RemoveHandler ステートメント](../../../visual-basic/language-reference/statements/removehandler-statement.md)
- [Handles 句](../../../visual-basic/language-reference/statements/handles-clause.md)
- [Event ステートメント](../../../visual-basic/language-reference/statements/event-statement.md)
- [イベント](../../../visual-basic/programming-guide/language-features/events/index.md)
