---
title: '方法: カスタム イベントを宣言してメモリを節約する'
ms.date: 07/20/2015
helpviewer_keywords:
- declaring events [Visual Basic], custom
- events [Visual Basic], custom
- custom events [Visual Basic]
ms.assetid: 87ebee87-260c-462f-979c-407874debd19
ms.openlocfilehash: 3cc2d3ea57f1a8daf704c2c929baf3f2acf78c17
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345132"
---
# <a name="how-to-declare-custom-events-to-conserve-memory-visual-basic"></a>方法: カスタム イベントを宣言してメモリを節約する (Visual Basic)
アプリケーションでメモリ使用量を少なくすることが重要な状況がいくつかあります。 カスタムイベントを使用すると、アプリケーションが処理するイベントにのみメモリを使用できます。  
  
 既定では、クラスがイベントを宣言すると、コンパイラは、イベント情報を格納するために、フィールドにメモリを割り当てます。 クラスに使用されているイベントの数が多い場合は、不必要にメモリを占有します。  
  
 Visual Basic が提供するイベントの既定の実装を使用する代わりに、カスタムイベントを使用してメモリ使用量をより慎重に管理できます。  
  
## <a name="example"></a>例  
 この例では、クラスは、`Events` フィールドに格納されている <xref:System.ComponentModel.EventHandlerList> クラスの1つのインスタンスを使用して、使用中のイベントに関する情報を格納します。 <xref:System.ComponentModel.EventHandlerList> クラスは、デリゲートを保持するように設計された最適化リストクラスです。  
  
 クラスのすべてのイベントでは、`Events` フィールドを使用して、各イベントを処理しているメソッドを追跡します。  
  
 [!code-vb[VbVbalrEvents#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#22)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.ComponentModel.EventHandlerList>
- [イベント](../../../../visual-basic/programming-guide/language-features/events/index.md)
- [方法: カスタム イベントを宣言してブロックを回避する](../../../../visual-basic/programming-guide/language-features/events/how-to-declare-custom-events-to-avoid-blocking.md)
