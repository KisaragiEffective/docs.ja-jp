---
title: '方法: カスタム イベントを宣言してブロックを回避する'
ms.date: 07/20/2015
helpviewer_keywords:
- declaring events [Visual Basic], custom
- events [Visual Basic], custom
- custom events [Visual Basic]
ms.assetid: 998b6a90-67c5-4d2c-8b11-366d3e355505
ms.openlocfilehash: 8d73d9c4590afb33e7176f647069cafcb3a9d7d8
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345147"
---
# <a name="how-to-declare-custom-events-to-avoid-blocking-visual-basic"></a>方法: カスタム イベントを宣言してブロックを回避する (Visual Basic)
1つのイベントハンドラーが後続のイベントハンドラーをブロックしないようにすることが重要な状況がいくつかあります。 カスタムイベントを使用すると、イベントでイベントハンドラーを非同期に呼び出すことができます。  
  
 既定では、イベント宣言のバッキングストアフィールドは、すべてのイベントハンドラーを順番に結合するマルチキャストデリゲートです。 これは、1つのハンドラーの完了に時間がかかる場合、完了するまで他のハンドラーがブロックされることを意味します。 (適切に動作するイベントハンドラーでは、時間がかかるか、ブロックする可能性のある操作を実行しないでください)。  
  
 によって提供さ Visual Basic イベントの既定の実装を使用する代わりに、カスタムイベントを使用してイベントハンドラーを非同期的に実行できます。  
  
## <a name="example"></a>例  
 この例では、`AddHandler` アクセサーは、`Click` イベントの各ハンドラーのデリゲートを `EventHandlerList` フィールドに格納されている <xref:System.Collections.ArrayList> に追加します。  
  
 コードで `Click` イベントが発生すると、`RaiseEvent` アクセサーは <xref:System.Web.Services.Protocols.LogicalMethodInfo.BeginInvoke%2A> メソッドを使用して、すべてのイベントハンドラーデリゲートを非同期的に呼び出します。 このメソッドは、ワーカースレッド上の各ハンドラーを呼び出し、すぐに制御を戻します。そのため、ハンドラーは互いにブロックできません。  
  
 [!code-vb[VbVbalrEvents#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#27)]  
  
## <a name="see-also"></a>参照

- <xref:System.Collections.ArrayList>
- <xref:System.Web.Services.Protocols.LogicalMethodInfo.BeginInvoke%2A>
- [イベント](../../../../visual-basic/programming-guide/language-features/events/index.md)
- [方法: カスタム イベントを宣言してメモリを節約する](../../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-conserve-memory.md)
