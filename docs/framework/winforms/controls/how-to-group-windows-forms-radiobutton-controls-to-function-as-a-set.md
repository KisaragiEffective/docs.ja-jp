---
title: Set RadioButton コントロールをセットとして機能するようにグループ化する
ms.date: 03/30/2017
helpviewer_keywords:
- radio buttons [Windows Forms], grouping
- controls [Windows Forms], grouping
- Windows Forms controls, grouping
- RadioButton control [Windows Forms], grouping
ms.assetid: 58f8fe34-50b7-49d8-a2be-c271be3c6b32
ms.openlocfilehash: c4b37c84bf0f93837b91c743c7681d39fd83a659
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76732953"
---
# <a name="how-to-group-windows-forms-radiobutton-controls-to-function-as-a-set"></a>方法 : セットとして機能する Windows フォーム RadioButton コントロールをグループ化する
Windows フォーム <xref:System.Windows.Forms.RadioButton> コントロールは、1つのプロシージャまたはオブジェクトに割り当てることができる2つ以上の設定の中からユーザーを選択できるように設計されています。 たとえば、<xref:System.Windows.Forms.RadioButton> コントロールのグループは、注文に対して選択したパッケージキャリアを表示できますが、使用されるのは1つの配送業者だけです。 したがって、機能グループの一部であっても、一度に1つの <xref:System.Windows.Forms.RadioButton> のみ選択できます。  
  
 オプションボタンをグループ化するには、<xref:System.Windows.Forms.Panel> コントロール、<xref:System.Windows.Forms.GroupBox> コントロール、フォームなどのコンテナー内に描画します。 フォームに直接追加されたすべてのラジオボタンは、1つのグループになります。 個別のグループを追加するには、パネルまたはグループボックス内に配置する必要があります。 パネルまたはグループボックスの詳細については、「 [Panel コントロールの概要](panel-control-overview-windows-forms.md)」または「 [GroupBox コントロールの概要](groupbox-control-overview-windows-forms.md)」を参照してください。  
  
### <a name="to-group-radiobutton-controls-as-a-set-to-function-independently-of-other-sets"></a>RadioButton コントロールを他のセットとは独立して機能するようにグループ化するには  
  
1. **ツールボックス**の **[Windows フォーム]** タブから <xref:System.Windows.Forms.GroupBox> または <xref:System.Windows.Forms.Panel> コントロールをフォームにドラッグします。  
  
2. <xref:System.Windows.Forms.GroupBox> コントロールまたは <xref:System.Windows.Forms.Panel> コントロールに <xref:System.Windows.Forms.RadioButton> コントロールを描画します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.RadioButton>
- [RadioButton コントロールの概要](radiobutton-control-overview-windows-forms.md)
- [Panel コントロールの概要](panel-control-overview-windows-forms.md)
- [GroupBox コントロールの概要](groupbox-control-overview-windows-forms.md)
- [CheckBox コントロールの概要](checkbox-control-overview-windows-forms.md)
- [RadioButton コントロール](radiobutton-control-windows-forms.md)
