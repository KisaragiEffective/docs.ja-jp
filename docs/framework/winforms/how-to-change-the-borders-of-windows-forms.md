---
title: フォームの境界線の変更
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Windows Forms, changing the borders
ms.assetid: b3d5fa56-80c6-4b10-b505-f9672307ed55
ms.openlocfilehash: 2c8eb25b44c7406e4312f432f2d69524346f94d6
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76739570"
---
# <a name="how-to-change-the-borders-of-windows-forms"></a>方法 : Windows フォームの境界線を変更する
Windows フォームの外観や動作を決定する際にはさまざまな境界線スタイルを選択できます。 <xref:System.Windows.Forms.Form.FormBorderStyle%2A> プロパティを変更して、フォームのサイズ変更動作を制御できます。 また、<xref:System.Windows.Forms.Form.FormBorderStyle%2A> を設定すると、キャプション バーの表示方法や、キャプション バーに表示されるボタンを変更できます。 詳細については、<xref:System.Windows.Forms.FormBorderStyle> を参照してください。  
  
 Visual Studio では、このタスクに対する広範なサポートが用意されています。  
  
 「[方法: デザイナーを使用して Windows フォームの境界線を変更する](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/yettzh3e(v=vs.100))」も参照してください。  
  
### <a name="to-set-the-border-style-of-windows-forms-programmatically"></a>プログラムで Windows フォームの境界線スタイルを設定するには  
  
- <xref:System.Windows.Forms.Form.FormBorderStyle%2A> プロパティを任意のスタイルに設定します。 次のコード例では、`DlgBx1` フォームの境界線スタイルを <xref:System.Windows.Forms.FormBorderStyle.FixedDialog>に設定します。  
  
    ```vb  
    DlgBx1.FormBorderStyle = System.Windows.Forms.FormBorderStyle.FixedDialog  
    ```  
  
    ```csharp  
    DlgBx1.FormBorderStyle = System.Windows.Forms.FormBorderStyle.FixedDialog;  
    ```  
  
    ```cpp  
    DlgBx1->FormBorderStyle =  
       System::Windows::Forms::FormBorderStyle::FixedDialog;  
    ```  
  
     「[方法: デザイン時にダイアログボックスを作成する](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/55cz5x2c(v=vs.100))」も参照してください。  
  
     また、オプションの **[最小化]** ボタンと **[最大化]** ボタンを提供するフォームの境界線スタイルを選択した場合は、これらのボタンのいずれかまたは両方を機能させるかどうかを指定できます。 これらのボタンは、ユーザーの操作感を細かく調節する場合に便利です。 既定では、 **[最小化]** ボタンと **[最大化]** ボタンが有効になり、それらの機能は **[プロパティ]** ウィンドウで操作されます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.FormBorderStyle>
- <xref:System.Windows.Forms.FormBorderStyle.FixedDialog>
- [Windows フォームについて](getting-started-with-windows-forms.md)
