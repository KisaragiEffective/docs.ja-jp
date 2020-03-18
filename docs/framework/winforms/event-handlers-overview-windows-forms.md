---
title: イベント ハンドラーの概要
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Windows Forms, event handling
- event handling [Windows Forms], Windows Forms
- event handlers [Windows Forms], about event handlers
ms.assetid: 228112e1-1711-42ee-8ffa-ff3555bffe66
ms.openlocfilehash: 10ba458197973ede35849a86fec35003f139b8d2
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743461"
---
# <a name="event-handlers-overview-windows-forms"></a>イベント ハンドラーの概要 (Windows フォーム)
イベント ハンドラーは、イベントにバインドされるメソッドです イベントが発生したとき、そのイベント ハンドラー内のコードが実行されます 各イベント ハンドラーは、イベントを正しく処理できるように 2 つのパラメーターを提供しています。 次の例は、<xref:System.Windows.Forms.Button> コントロールの <xref:System.Windows.Forms.Control.Click> イベントのイベントハンドラーを示しています。  
  
```vb  
Private Sub button1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles button1.Click  
  
End Sub  
```  
  
```csharp  
private void button1_Click(object sender, System.EventArgs e)   
{  
  
}  
```  
  
```cpp  
private:  
  void button1_Click(System::Object ^ sender,  
    System::EventArgs ^ e)  
  {  
  
  }  
```  
  
 最初のパラメーターである`sender`は、イベントを発生させたオブジェクトへの参照を提供します。 上の例の2番目のパラメーター `e`は、処理されるイベントに固有のオブジェクトを渡します。 そのオブジェクトのプロパティ (また、ときには、そのメソッド) を参照することによって、マウス イベントにおけるマウスの位置や、あるいはドラッグ アンド ドロップのイベントで移動したデータのような情報を取得できます。  
  
 多くのイベントは、2 番目のパラメーターに異なるイベント オブジェクト型を持つ イベント ハンドラーを有しています。 <xref:System.Windows.Forms.Control.MouseDown> イベントや <xref:System.Windows.Forms.Control.MouseUp> イベントのイベントハンドラーの中には、2番目のパラメーターと同じオブジェクト型を持つものもあります。 この種のイベントでは、両方のイベントを処理するために同じイベント ハンドラーを使用することができます。  
  
 また、異なるコントロールの同じイベントを処理するために同じイベント ハンドラーを使用することもできます。 たとえば、フォームに <xref:System.Windows.Forms.RadioButton> コントロールのグループがある場合、<xref:System.Windows.Forms.Control.Click> イベントの1つのイベントハンドラーを作成し、各コントロールの <xref:System.Windows.Forms.Control.Click> イベントを1つのイベントハンドラーにバインドすることができます。 詳細については、「[方法: Windows フォームの1つのイベントハンドラーに複数のイベントを接続](how-to-connect-multiple-events-to-a-single-event-handler-in-windows-forms.md)する」を参照してください。  
  
## <a name="see-also"></a>参照

- [Windows フォーム内のイベント ハンドラーの作成](creating-event-handlers-in-windows-forms.md)
- [イベントの概要](events-overview-windows-forms.md)
