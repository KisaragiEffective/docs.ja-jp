---
title: GroupBox コントロールを使用したコントロールのグループ化
ms.date: 03/30/2017
helpviewer_keywords:
- controls [Windows Forms], grouping
- GroupBox control [Windows Forms], grouping controls
- Windows Forms controls, grouping
ms.assetid: 0bda316d-bd2a-43aa-ac73-342453303169
ms.openlocfilehash: bb7476c410d2802b5d32cc9842a778f290765e32
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736657"
---
# <a name="how-to-group-controls-with-the-windows-forms-groupbox-control"></a>方法 : Windows フォーム GroupBox コントロールを使用してコントロールをグループ化する
Windows フォーム <xref:System.Windows.Forms.GroupBox> コントロールは、他のコントロールをグループ化するために使用されます。 コントロールをグループ化する理由は3つあります。  
  
- クリアユーザーインターフェイスに関連するフォーム要素の視覚的なグループ化を作成するには  
  
- プログラムによるグループ化 (オプションボタンなど) を作成する場合は。  
  
- デザイン時にコントロールを1つの単位として移動する場合は。  
  
### <a name="to-create-a-group-of-controls"></a>コントロールのグループを作成するには  
  
1. フォームに <xref:System.Windows.Forms.GroupBox> コントロールを描画します。  
  
2. グループボックスに他のコントロールを追加し、それぞれをグループボックス内に描画します。  
  
     グループボックスに含める既存のコントロールがある場合は、すべてのコントロールを選択してクリップボードに貼り付け、<xref:System.Windows.Forms.GroupBox> コントロールを選択して、グループボックスに貼り付けることができます。 また、[グループ] ボックスにドラッグすることもできます。  
  
3. グループボックスの [<xref:System.Windows.Forms.GroupBox.Text%2A>] プロパティを適切なキャプションに設定します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.GroupBox>
- [GroupBox コントロール](groupbox-control-windows-forms.md)
