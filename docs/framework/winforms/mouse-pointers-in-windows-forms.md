---
title: マウスポインター
ms.date: 03/30/2017
helpviewer_keywords:
- pointers [Windows Forms], setting
- mouse pointers
- mouse cursors
- mouse pointers [Windows Forms], setting
- cursors [Windows Forms], setting
- mouse [Windows Forms], cursors
ms.assetid: c3400d85-de5b-42e8-abc3-d6088d69ee53
ms.openlocfilehash: 4e8349effcd9b59045e39b763b4cb8b7d2fceb91
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76740956"
---
# <a name="mouse-pointers-in-windows-forms"></a>Windows フォームにおけるマウス ポインター
マウス*ポインター*はカーソルとも呼ばれ、マウスを使用してユーザーが入力する画面上のフォーカスポイントを指定するビットマップです。 このトピックでは Windows フォームのマウスポインターの概要について説明し、マウスポインターを変更および制御する方法について説明します。  
  
## <a name="accessing-the-mouse-pointer"></a>マウスポインターへのアクセス  
 マウスポインターは <xref:System.Windows.Forms.Cursor> クラスによって表され、各 <xref:System.Windows.Forms.Control> には、そのコントロールのポインターを指定する <xref:System.Windows.Forms.Control.Cursor%2A?displayProperty=nameWithType> プロパティがあります。 <xref:System.Windows.Forms.Cursor> クラスには、ポインターを記述するプロパティ (<xref:System.Windows.Forms.Cursor.Position%2A> や <xref:System.Windows.Forms.Cursor.HotSpot%2A> プロパティなど) と、ポインターの外観を変更できるメソッド (<xref:System.Windows.Forms.Cursor.Show%2A>、<xref:System.Windows.Forms.Cursor.Hide%2A>、<xref:System.Windows.Forms.Cursor.DrawStretched%2A> メソッドなど) が含まれています。  
  
## <a name="controlling-the-mouse-pointer"></a>マウスポインターの制御  
 場合によっては、マウスポインターを使用できる領域を制限したり、マウスの位置を変更したりすることができます。 <xref:System.Windows.Forms.Cursor>の <xref:System.Windows.Forms.Cursor.Position%2A> プロパティを使用して、マウスの現在の位置を取得または設定できます。 また、マウスポインターを使用して <xref:System.Windows.Forms.Cursor.Clip%2A> プロパティを設定できる領域を制限することもできます。 既定では、クリップ領域は画面全体です。  
  
## <a name="changing-the-mouse-pointer"></a>マウスポインターを変更する  
 マウスポインターを変更することは、ユーザーにフィードバックを提供するための重要な方法です。 たとえば、<xref:System.Windows.Forms.Control.MouseEnter> および <xref:System.Windows.Forms.Control.MouseLeave> イベントのハンドラーでマウスポインターを変更して、計算が行われていることをユーザーに通知したり、コントロールのユーザーの操作を制限したりすることができます。 場合によっては、アプリケーションがドラッグアンドドロップ操作に関係しているときなど、システムイベントが原因でマウスポインターが変化することがあります。  
  
 マウスポインターを変更する主な方法は、コントロールの <xref:System.Windows.Forms.Control.Cursor%2A?displayProperty=nameWithType> または <xref:System.Windows.Forms.Control.DefaultCursor%2A> プロパティを新しい <xref:System.Windows.Forms.Cursor>に設定することです。 マウスポインターを変更する例については、<xref:System.Windows.Forms.Cursor> クラスのコード例を参照してください。 さらに、<xref:System.Windows.Forms.Cursors> クラスは、手に似たポインターなど、さまざまな種類のポインターに対して <xref:System.Windows.Forms.Cursor> オブジェクトのセットを公開します。 砂時計に似た待機ポインターを表示するには、マウスポインターがコントロール上にあるときは常に、<xref:System.Windows.Forms.Control> クラスの <xref:System.Windows.Forms.Control.UseWaitCursor%2A> プロパティを使用します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Cursor>
- [Windows フォーム アプリケーションにおけるマウス入力](mouse-input-in-a-windows-forms-application.md)
- [Windows フォームにおけるドラッグ アンド ドロップ機能](drag-and-drop-functionality-in-windows-forms.md)
