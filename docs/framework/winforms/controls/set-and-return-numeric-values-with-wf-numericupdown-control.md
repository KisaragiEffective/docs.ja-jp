---
title: NumericUpDown コントロールを使用して数値を設定して返す
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- numeric values [Windows Forms], Windows Forms
- Windows Forms, numeric values
- Windows Forms controls, NumericUpDown
- NumericUpDown control [Windows Forms], setting and returning values
ms.assetid: 5bd8f8cd-4c12-49ea-9cc3-2a647d064689
ms.openlocfilehash: a0b264fec9619b467c293bcb96278c4517775ac3
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743019"
---
# <a name="how-to-set-and-return-numeric-values-with-the-windows-forms-numericupdown-control"></a>方法 : Windows フォームの NumericUpDown コントロールを使用して数値を設定および取得する
<xref:System.Windows.Forms.NumericUpDown> コントロール Windows フォームの数値は <xref:System.Windows.Forms.NumericUpDown.Value%2A> プロパティによって決定されます。 他のプロパティと同様に、コントロールの値の条件付きテストを記述できます。 <xref:System.Windows.Forms.NumericUpDown.Value%2A> プロパティが設定されたら、それに対して操作を実行するコードを記述して直接調整できます。または、<xref:System.Windows.Forms.NumericUpDown.UpButton%2A> メソッドと <xref:System.Windows.Forms.NumericUpDown.DownButton%2A> メソッドを呼び出すこともできます。  
  
### <a name="to-set-the-numeric-value"></a>数値を設定するには  
  
1. コードまたはプロパティウィンドウの <xref:System.Windows.Forms.NumericUpDown.Value%2A> プロパティに値を割り当てます。  
  
    ```vb  
    NumericUpDown1.Value = 55  
    ```  
  
    ```csharp  
    numericUpDown1.Value = 55;  
    ```  
  
    ```cpp  
    numericUpDown1->Value = 55;  
    ```  
  
     または  
  
2. <xref:System.Windows.Forms.NumericUpDown.UpButton%2A> または <xref:System.Windows.Forms.NumericUpDown.DownButton%2A> メソッドを呼び出して、<xref:System.Windows.Forms.NumericUpDown.Increment%2A> プロパティで指定した量だけ値を増減させます。  
  
    ```vb  
    NumericUpDown1.UpButton()  
    ```  
  
    ```csharp  
    numericUpDown1.UpButton();  
    ```  
  
    ```cpp  
    numericUpDown1->UpButton();  
    ```  
  
### <a name="to-return-the-numeric-value"></a>数値を取得するには  
  
- コード内の <xref:System.Windows.Forms.NumericUpDown.Value%2A> プロパティにアクセスします。  
  
    ```vb  
    If NumericUpDown1.Value >= 65 Then  
       MessageBox.Show("Age is: " & NumericUpDown1.Value.ToString)  
    Else  
       MessageBox.Show("The customer is ineligible for a senior citizen discount.")  
    End If  
    ```  
  
    ```csharp  
    if(numericUpDown1.Value >= 65)  
    {  
       MessageBox.Show("Age is: " + numericUpDown1.Value.ToString());  
    }  
    else  
    {  
       MessageBox.Show("The customer is ineligible for a senior citizen discount.");  
    }  
    ```  
  
    ```cpp  
    if(numericUpDown1->Value >= 65)  
    {  
       MessageBox::Show(String::Concat("Age is: ",  
          numericUpDown1->Value.ToString()));  
    }  
    else  
    {  
       MessageBox::Show  
          ("The customer is ineligible for a senior citizen discount.");  
    }  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.NumericUpDown>
- <xref:System.Windows.Forms.NumericUpDown.Value%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.NumericUpDown.Increment%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.NumericUpDown.UpButton%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.NumericUpDown.DownButton%2A?displayProperty=nameWithType>
- [NumericUpDown コントロール](numericupdown-control-windows-forms.md)
- [NumericUpDown コントロールの概要](numericupdown-control-overview-windows-forms.md)
