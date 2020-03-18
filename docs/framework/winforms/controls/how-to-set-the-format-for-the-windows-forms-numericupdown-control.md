---
title: NumericUpDown コントロールの形式を設定する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- NumericUpDown control [Windows Forms], formatting values
- up-down controls [Windows Forms], formatting numeric values
ms.assetid: fa7c5557-6bfb-45b2-975d-8887b23b0ba0
ms.openlocfilehash: 5ef1c801e96bef7b92e7e69dc36491144c456eeb
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742178"
---
# <a name="how-to-set-the-format-for-the-windows-forms-numericupdown-control"></a>方法 : Windows フォームの NumericUpDown コントロールの書式を設定する
Windows フォーム <xref:System.Windows.Forms.NumericUpDown> コントロールでの値の表示方法を構成できます。 <xref:System.Windows.Forms.NumericUpDown.DecimalPlaces%2A> プロパティは、小数点の後に表示する数字の数を決定します。既定値は0です。 <xref:System.Windows.Forms.NumericUpDown.ThousandsSeparator%2A> プロパティは、3桁の10進数の間に区切り記号を挿入するかどうかを決定します。既定値は `false`です。 <xref:System.Windows.Forms.NumericUpDown.Hexadecimal%2A> プロパティが `true`に設定されている場合、コントロールでは、10進数形式ではなく16進数で値を表示できます。既定値は `false`です。  
  
### <a name="to-format-the-numeric-value"></a>数値の書式を設定するには  
  
- Decimal 値を表示するには、<xref:System.Windows.Forms.NumericUpDown.DecimalPlaces%2A> プロパティを整数に設定し、<xref:System.Windows.Forms.NumericUpDown.ThousandsSeparator%2A> プロパティを `true` または `false`に設定します。  
  
    ```vb  
    NumericUpDown1.DecimalPlaces = 2  
    NumericUpDown1.ThousandsSeparator = True  
    ```  
  
    ```csharp  
    numericUpDown1.DecimalPlaces = 2;  
    numericUpDown1.ThousandsSeparator = true;  
    ```  
  
    ```cpp  
    numericUpDown1->DecimalPlaces = 2;  
    numericUpDown1->ThousandsSeparator = true;  
    ```  
  
     または  
  
- <xref:System.Windows.Forms.NumericUpDown.Hexadecimal%2A> プロパティを `true`に設定して、16進数の値を表示します。  
  
    ```vb  
    NumericUpDown1.Hexadecimal = True  
    ```  
  
    ```csharp  
    numericUpDown1.Hexadecimal = true;  
    ```  
  
    ```cpp  
    numericUpDown1->Hexadecimal = true;  
    ```  
  
    > [!NOTE]
    > 値が16進数としてフォームに表示されている場合でも、<xref:System.Windows.Forms.NumericUpDown.Value%2A> プロパティで実行するテストでは、その10進値がテストされます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.NumericUpDown>
- [NumericUpDown コントロール](numericupdown-control-windows-forms.md)
- [NumericUpDown コントロールの概要](numericupdown-control-overview-windows-forms.md)
