---
title: ColorDialog コンポーネントの外観の変更
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- ColorDialog component [Windows Forms], examples
- ColorDialog component [Windows Forms], formatting appearance
- color dialog box [Windows Forms], configuring appearance
ms.assetid: bba4e262-1cd7-4f63-89cf-330a36f7b539
ms.openlocfilehash: 0402d7f3c03a0771512a03ac54e1b093c9fe6e9b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746640"
---
# <a name="how-to-change-the-appearance-of-the-windows-forms-colordialog-component"></a>方法 : Windows フォーム ColorDialog コンポーネントの表示形式を変更する
Windows フォーム <xref:System.Windows.Forms.ColorDialog> コンポーネントの外観は、多くのプロパティを使用して構成できます。 ダイアログボックスには2つのセクションがあります。1つは基本色を示し、もう1つはユーザーがカスタムカラーを定義できるようにするものです。  
  
 ほとんどのプロパティは、ダイアログボックスからユーザーが選択できる色を制限します。 <xref:System.Windows.Forms.ColorDialog.AllowFullOpen%2A> プロパティが `true`に設定されている場合、ユーザーはカスタム色を定義できます。 ダイアログボックスを拡張してカスタムの色を定義すると、<xref:System.Windows.Forms.ColorDialog.FullOpen%2A> プロパティが `true` ます。それ以外の場合は、ユーザーは [カスタムカラーの定義] ボタンをクリックする必要があります。 <xref:System.Windows.Forms.ColorDialog.AnyColor%2A> プロパティが `true`に設定されている場合、ダイアログボックスには、基本色のセットで使用可能なすべての色が表示されます。 <xref:System.Windows.Forms.ColorDialog.SolidColorOnly%2A> プロパティが `true`に設定されている場合、ユーザーはディザーカラーを選択できません。選択できるのは純色のみです。  
  
 <xref:System.Windows.Forms.ColorDialog.ShowHelp%2A> プロパティが `true`に設定されている場合、ダイアログボックスに [ヘルプ] ボタンが表示されます。 ユーザーが [ヘルプ] ボタンをクリックすると、<xref:System.Windows.Forms.ColorDialog> コンポーネントの <xref:System.Windows.Forms.CommonDialog.HelpRequest> イベントが発生します。  
  
### <a name="to-configure-the-appearance-of-the-color-dialog-box"></a>[色] ダイアログボックスの外観を構成するには  
  
1. <xref:System.Windows.Forms.ColorDialog.AllowFullOpen%2A>、<xref:System.Windows.Forms.ColorDialog.AnyColor%2A>、<xref:System.Windows.Forms.ColorDialog.SolidColorOnly%2A>、および <xref:System.Windows.Forms.ColorDialog.ShowHelp%2A> プロパティを目的の値に設定します。  
  
    ```vb  
    ColorDialog1.AllowFullOpen = True  
    ColorDialog1.AnyColor = True  
    ColorDialog1.SolidColorOnly = False  
    ColorDialog1.ShowHelp = True  
    ```  
  
    ```csharp  
    colorDialog1.AllowFullOpen = true;  
    colorDialog1.AnyColor = true;  
    colorDialog1.SolidColorOnly = false;  
    colorDialog1.ShowHelp = true;  
    ```  
  
    ```cpp  
    colorDialog1->AllowFullOpen = true;  
    colorDialog1->AnyColor = true;  
    colorDialog1->SolidColorOnly = false;  
    colorDialog1->ShowHelp = true;  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ColorDialog>
- [ColorDialog コンポーネント](colordialog-component-windows-forms.md)
- [ColorDialog コンポーネントの概要](colordialog-component-overview-windows-forms.md)
