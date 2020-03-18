---
title: Handles 句
ms.date: 07/20/2015
f1_keywords:
- Handles
- vb.Handles
helpviewer_keywords:
- Handles keyword [Visual Basic]
ms.assetid: 1b051c0e-f499-42f6-acb5-6f4f27824b40
ms.openlocfilehash: 2fecad919722f3da25c48f133a9c92b5e683d5e4
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345910"
---
# <a name="handles-clause-visual-basic"></a>Handles 句 (Visual Basic)
プロシージャが指定されたイベントを処理することを宣言します。  
  
## <a name="syntax"></a>構文  
  
```vb  
proceduredeclaration Handles eventlist  
```  
  
## <a name="parts"></a>指定項目  
 `proceduredeclaration`  
 イベントを処理するプロシージャの `Sub` プロシージャ宣言。  
  
 `eventlist`  
 コンマで区切られた、`proceduredeclaration` が処理するイベントの一覧。 イベントは、現在のクラスの基底クラス、または `WithEvents` キーワードを使用して宣言されたオブジェクトによって発生する必要があります。  
  
## <a name="remarks"></a>コメント  
 プロシージャ宣言の最後で `Handles` キーワードを使用すると、 `WithEvents` キーワードで宣言されたオブジェクト変数によって発生したイベントが処理されるようになります。 また、`Handles` キーワードを派生クラスで使用すると、基底クラスからのイベントを処理することもできます。  
  
 `Handles` キーワードと `AddHandler` ステートメントはどちらも特定のプロシージャで特定のイベントを処理するように指定できますが、両者には違いがあります。 `Handles` キーワードは、プロシージャの定義時に特定のイベントを処理するよう指定する場合に使用します。 `AddHandler` ステートメントは、実行時にプロシージャをイベントに接続します。 詳細については、「 [AddHandler ステートメント](../../../visual-basic/language-reference/statements/addhandler-statement.md)」を参照してください。  
  
 カスタム イベントの場合、アプリケーションは、プロシージャをイベント ハンドラーとして追加するときにイベントの `AddHandler` アクセサーを呼び出します。 カスタムイベントの詳細については、「 [Event Statement](../../../visual-basic/language-reference/statements/event-statement.md)」を参照してください。  
  
## <a name="example"></a>例  
 [!code-vb[VbVbalrEvents#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#2)]  
  
 派生クラスで `Handles` ステートメントを使用して基底クラスからのイベントを処理する方法を次の例に示します。  
  
 [!code-vb[VbVbalrEvents#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#3)]  
  
## <a name="example"></a>例  
 次の例には、 **WPF アプリケーション**プロジェクトの2つのボタンイベントハンドラーが含まれています。  
  
 [!code-vb[VbVbalrEvents#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/class3.vb#41)]  
  
## <a name="example"></a>例  
 次の例は、前の例と同じです。 `eventlist` 句の `Handles` には 2 つのボタンのイベントが含まれています。  
  
 [!code-vb[VbVbalrEvents#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/class3.vb#42)]  
  
## <a name="see-also"></a>参照

- [WithEvents](../../../visual-basic/language-reference/modifiers/withevents.md)
- [AddHandler ステートメント](../../../visual-basic/language-reference/statements/addhandler-statement.md)
- [RemoveHandler ステートメント](../../../visual-basic/language-reference/statements/removehandler-statement.md)
- [Event ステートメント](../../../visual-basic/language-reference/statements/event-statement.md)
- [RaiseEvent ステートメント](../../../visual-basic/language-reference/statements/raiseevent-statement.md)
- [イベント](../../../visual-basic/programming-guide/language-features/events/index.md)
