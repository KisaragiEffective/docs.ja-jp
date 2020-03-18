---
title: パネルの背景を設定する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- background colors [Windows Forms], Windows Forms Panel controls
- background images [Windows Forms], Windows Forms Panel controls
- Panel control [Windows Forms], background
- colors [Windows Forms], Windows Forms Panel controls
ms.assetid: 096cbd8d-45cc-47b8-b1ef-a27f60ea8be0
ms.openlocfilehash: ba2619354403793aea7ca15d43649da9637079a6
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744734"
---
# <a name="how-to-set-the-background-of-a-windows-forms-panel"></a>方法 : Windows フォーム パネルの背景を設定する
Windows フォーム <xref:System.Windows.Forms.Panel> コントロールには、背景色と背景画像の両方を表示できます。 <xref:System.Windows.Forms.Control.BackColor%2A> プロパティは、ラベルやラジオボタンなど、含まれるコントロールの背景色を設定します。 <xref:System.Windows.Forms.Control.BackgroundImage%2A> プロパティが設定されていない場合、<xref:System.Windows.Forms.Control.BackColor%2A> 選択によってパネル全体が表示されます。 <xref:System.Windows.Forms.Control.BackgroundImage%2A> プロパティが設定されている場合、イメージは、含まれているコントロールの背後に表示されます。  
  
### <a name="to-set-the-background-programmatically"></a>バックグラウンドをプログラムによって設定するには  
  
1. パネルの <xref:System.Windows.Forms.Control.BackColor%2A> プロパティを <xref:System.Drawing.Color?displayProperty=nameWithType>型の値に設定します。  
  
    ```vb  
    Panel1.BackColor = Color.AliceBlue  
    ```  
  
    ```csharp  
    panel1.BackColor = Color.AliceBlue;  
    ```  
  
    ```cpp  
    panel1->BackColor = Color::AliceBlue;  
    ```  
  
2. <xref:System.Drawing.Image?displayProperty=nameWithType> クラスの <xref:System.Drawing.Image.FromFile%2A> メソッドを使用して、パネルの <xref:System.Windows.Forms.Control.BackgroundImage%2A> プロパティを設定します。  
  
    ```vb  
    ' You should replace the bolded image   
    ' in the sample below with an image of your own choosing.  
    Panel1.BackgroundImage = Image.FromFile _  
        (System.Environment.GetFolderPath _  
        (System.Environment.SpecialFolder.Personal) _  
        & "\Image.gif")  
    ```  
  
    ```csharp  
    // You should replace the bolded image   
    // in the sample below with an image of your own choosing.  
    // Note the escape character used (@) when specifying the path.  
    panel1.BackgroundImage = Image.FromFile  
       (System.Environment.GetFolderPath  
       (System.Environment.SpecialFolder.Personal)  
       + @"\Image.gif");  
    ```  
  
    ```cpp  
    // You should replace the bolded image   
    // in the sample below with an image of your own choosing.  
    panel1->BackgroundImage = Image::FromFile(String::Concat(  
       System::Environment::GetFolderPath  
       (System::Environment::SpecialFolder::Personal),  
       "\\Image.gif"));  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Control.BackColor%2A>
- <xref:System.Windows.Forms.Control.BackgroundImage%2A>
- [Panel コントロール](panel-control-windows-forms.md)
- [Panel コントロールの概要](panel-control-overview-windows-forms.md)
