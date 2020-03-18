---
title: 継承されたイベント ハンドラーのトラブルシューティング
ms.date: 07/20/2015
helpviewer_keywords:
- troubleshooting events [Visual Basic]
- inherited events [Visual Basic]
- troubleshooting Visual Basic, event handlers
- event handling, troubleshooting
- event handlers, troubleshooting
ms.assetid: e1c8759f-5370-4308-8476-8c48b73509bf
ms.openlocfilehash: fd2ef1c25233cc1eaad6bcde68923688393b471d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345108"
---
# <a name="troubleshooting-inherited-event-handlers-in-visual-basic"></a>Visual Basic での継承されたイベント ハンドラーのトラブルシューティング
このトピックでは、継承されたコンポーネントのイベントハンドラーで発生する一般的な問題について説明します。  
  
## <a name="procedures"></a>手順  
  
#### <a name="code-in-event-handler-executes-twice-for-every-call"></a>イベントハンドラーのコードは、すべての呼び出しに対して2回実行されます。  
  
- 継承されたイベントハンドラーに[Handles](../../../../visual-basic/language-reference/statements/handles-clause.md)句を含めることはできません。 基底クラスのメソッドは既にイベントに関連付けられており、それに応じて起動されます。 継承されたメソッドから `Handles` 句を削除します。  
  
     [!code-vb[VbVbalrEvents#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#32)]  
  
- 継承されたメソッドに `Handles` キーワードがない場合は、コードに追加の[AddHandler ステートメント](../../../../visual-basic/language-reference/statements/addhandler-statement.md)、または同じイベントを処理する追加のメソッドが含まれていないことを確認します。  
  
## <a name="see-also"></a>参照

- [イベント](../../../../visual-basic/programming-guide/language-features/events/index.md)
