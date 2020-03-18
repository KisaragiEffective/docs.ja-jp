---
title: コントロールでのイベントの定義
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- events [Windows Forms], defining within Windows Forms custom controls
- custom controls [Windows Forms], events using code
ms.assetid: d89f1096-8061-42e2-a855-a1f053f1940a
ms.openlocfilehash: d45c369e1fc82ee009a85b5b35fe6aa754873436
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746081"
---
# <a name="defining-an-event-in-windows-forms-controls"></a>Windows フォーム コントロールのイベントの定義
カスタムイベントの定義の詳細については、「[イベント](../../../standard/events/index.md)」を参照してください。 関連データがないイベントを定義する場合、イベント データの基本型である <xref:System.EventArgs> を使用し、イベント デリゲートとして <xref:System.EventHandler> を使用します。 イベントメンバーと、イベントを発生させる protected `On`*EventName*メソッドを定義するだけです。  
  
 `FlashTrackBar` カスタム コントロールによる `ValueChanged` カスタム イベントの定義方法を示すコードを次に示します。 `FlashTrackBar` サンプルの完全なコードについては、「[方法: 進行状況を示す Windows フォームコントロールを作成](how-to-create-a-windows-forms-control-that-shows-progress.md)する」を参照してください。  
  
```vb  
Option Explicit  
Option Strict  
  
Imports System  
Imports System.Windows.Forms  
Imports System.Drawing  
  
Public Class FlashTrackBar  
   Inherits Control  
  
   ' The event does not have any data, so EventHandler is adequate   
   ' as the event delegate.          
   ' Define the event member using the event keyword.  
   ' In this case, for efficiency, the event is defined   
   ' using the event property construct.  
   Public Event ValueChanged As EventHandler  
   ' The protected method that raises the ValueChanged   
   ' event when the value has actually   
   ' changed. Derived controls can override this method.    
   Protected Overridable Sub OnValueChanged(e As EventArgs)  
      RaiseEvent ValueChanged(Me, e)  
   End Sub  
End Class  
```  
  
```csharp  
using System;  
using System.Windows.Forms;  
using System.Drawing;  
  
public class FlashTrackBar : Control {  
   // The event does not have any data, so EventHandler is adequate   
   // as the event delegate.  
   private EventHandler onValueChanged;  
   // Define the event member using the event keyword.  
   // In this case, for efficiency, the event is defined   
   // using the event property construct.  
   public event EventHandler ValueChanged {  
            add {  
                onValueChanged += value;  
            }  
            remove {  
                onValueChanged -= value;  
            }  
        }  
   // The protected method that raises the ValueChanged  
   // event when the value has actually   
   // changed. Derived controls can override this method.    
   protected virtual void OnValueChanged(EventArgs e) 
   {  
       ValueChanged?.Invoke(this, e);  
   }  
}  
```  
  
## <a name="see-also"></a>参照

- [Windows フォーム コントロールのイベント](events-in-windows-forms-controls.md)
- [イベント](../../../standard/events/index.md)
