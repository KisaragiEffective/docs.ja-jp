---
title: コントロール内のイベント
ms.date: 03/30/2017
helpviewer_keywords:
- events [Windows Forms], custom controls (using code)
- custom controls [Windows Forms], events overview (using code)
ms.assetid: 7e3d1379-87aa-437c-afce-c99454eff30e
ms.openlocfilehash: c60713917b9c84aa7fad50fb1c81fc9252149ad6
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745755"
---
# <a name="events-in-windows-forms-controls"></a>Windows フォーム コントロールのイベント
Windows フォームコントロールは、<xref:System.Windows.Forms.Control?displayProperty=nameWithType>から60を超えるイベントを継承します。 このようなイベントには、コントロールが描画される原因となる <xref:System.Windows.Forms.Control.Paint> イベント、<xref:System.Windows.Forms.Control.Resize> と <xref:System.Windows.Forms.Control.Layout> イベント、低レベルのマウスおよびキーボードイベントなどのウィンドウの表示に関連するイベントが含まれます。 一部の下位レベルのイベントは、<xref:System.Windows.Forms.Control> によって <xref:System.Windows.Forms.Control.Click> や <xref:System.Windows.Forms.Control.DoubleClick>などのセマンティックイベントに合成されます。 継承されたイベントの詳細については、「<xref:System.Windows.Forms.Control>」を参照してください。  
  
 継承されたイベントの機能をカスタム コントロールでオーバーライドする必要がある場合、デリゲートを結び付けるのではなく、継承された `On`*EventName* メソッドをオーバーライドします。 .NET Framework でのイベント モデルに詳しくない場合は、「[コンポーネントからのイベントの生成](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/sh2e3k5z(v=vs.120))」を参照してください。  
  
## <a name="see-also"></a>参照

- [OnPaint メソッドのオーバーライド](overriding-the-onpaint-method.md)
- [ユーザーの入力の処理](handling-user-input.md)
- [イベントの定義](defining-an-event-in-windows-forms-controls.md)
- [イベント](../../../standard/events/index.md)
