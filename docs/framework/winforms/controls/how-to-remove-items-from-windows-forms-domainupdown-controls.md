---
title: DomainUpDown コントロールからの項目の削除
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- DomainUpDown control [Windows Forms], removing items from
- spin button control [Windows Forms], removing items
ms.assetid: e70f5cbc-b497-41a9-975a-344c00e56ed2
ms.openlocfilehash: e52af5c5add4fda93e2b51c8afdb90c92e8d2f62
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76735770"
---
# <a name="how-to-remove-items-from-windows-forms-domainupdown-controls"></a>方法 : Windows フォームの DomainUpDown コントロールから項目を削除する
<xref:System.Windows.Forms.DomainUpDown.DomainUpDownItemCollection> クラスの <xref:System.Windows.Forms.DomainUpDown.DomainUpDownItemCollection.Remove%2A> または <xref:System.Windows.Forms.DomainUpDown.DomainUpDownItemCollection.RemoveAt%2A> メソッドを呼び出すことによって、Windows フォーム <xref:System.Windows.Forms.DomainUpDown> コントロールから項目を削除できます。 <xref:System.Windows.Forms.DomainUpDown.DomainUpDownItemCollection.Remove%2A> メソッドは、特定の項目を削除し、<xref:System.Windows.Forms.DomainUpDown.DomainUpDownItemCollection.RemoveAt%2A> メソッドはその位置によって項目を削除します。  
  
### <a name="to-remove-an-item"></a>アイテムを削除するには  
  
- <xref:System.Windows.Forms.DomainUpDown.DomainUpDownItemCollection> クラスの <xref:System.Windows.Forms.DomainUpDown.DomainUpDownItemCollection.Remove%2A> メソッドを使用して、名前を指定して項目を削除します。  
  
    ```vb  
    DomainUpDown1.Items.Remove("noodles")  
    ```  
  
    ```csharp  
    domainUpDown1.Items.Remove("noodles");  
    ```  
  
    ```cpp  
    domainUpDown1->Items->Remove("noodles");  
    ```  
  
     または  
  
- 位置によって項目を削除するには、<xref:System.Windows.Forms.DomainUpDown.DomainUpDownItemCollection.RemoveAt%2A> メソッドを使用します。  
  
    ```vb  
    ' Removes the first item in the list.  
    DomainUpDown1.Items.RemoveAt(0)  
    ```  
  
    ```csharp  
    // Removes the first item in the list.  
    domainUpDown1.Items.RemoveAt(0);  
    ```  
  
    ```cpp  
    // Removes the first item in the list.  
    domainUpDown1->Items->RemoveAt(0);  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DomainUpDown>
- <xref:System.Windows.Forms.DomainUpDown.DomainUpDownItemCollection.Remove%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DomainUpDown.DomainUpDownItemCollection.RemoveAt%2A?displayProperty=nameWithType>
- [DomainUpDown コントロール](domainupdown-control-windows-forms.md)
- [DomainUpDown コントロールの概要](domainupdown-control-overview-windows-forms.md)
