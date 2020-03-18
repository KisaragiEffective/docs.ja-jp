---
title: CheckBox コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- CheckBox
helpviewer_keywords:
- CheckBox control [Windows Forms], about CheckBox control
- data binding [Windows Forms], checkbox controls
- check boxes [Windows Forms], about check boxes
ms.assetid: 085a4e0b-9046-473f-b141-d0edddfb2ebb
ms.openlocfilehash: b42816cf1bc0ce1ab6db0a2a436b17b0d4370d59
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76737093"
---
# <a name="checkbox-control-overview-windows-forms"></a>CheckBox コントロールの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.CheckBox> コントロールは、特定の条件がオンかオフかを示します。 一般的に、はい/いいえ、または True/False の選択肢をユーザーに提示するために使用されます。 ユーザーが 1 つ以上選択可能な複数の選択肢を表示するために、チェック ボックス コントロールをグループの中で使用できます。  
  
 チェックボックスコントロールは、ユーザーが選択した項目を示すために使用されるという点で、オプションボタンコントロールに似ています。 グループ内のラジオボタンは一度に1つしか選択できないという点で異なります。 ただし、チェックボックスコントロールを使用すると、任意の数のチェックボックスをオンにできます。  
  
 チェックボックスは、単純なデータバインディングを使用してデータベース内の要素に接続されている場合があります。 複数のチェックボックスは、<xref:System.Windows.Forms.GroupBox> コントロールを使用してグループ化することができます。 これは、視覚的な外観やユーザーインターフェイスのデザインに役立ちます。グループ化されたコントロールはフォームデザイナー上で一緒に移動できるためです。 詳細については、「[データバインディング](../windows-forms-data-binding.md)と[GroupBox コントロール](groupbox-control-windows-forms.md)の Windows フォーム」を参照してください。  
  
 <xref:System.Windows.Forms.CheckBox> コントロールには、<xref:System.Windows.Forms.CheckBox.Checked%2A> と <xref:System.Windows.Forms.CheckBox.CheckState%2A>という2つの重要なプロパティがあります。 <xref:System.Windows.Forms.CheckBox.Checked%2A> プロパティは、`true` または `false`のいずれかを返します。 <xref:System.Windows.Forms.CheckBox.CheckState%2A> プロパティは、<xref:System.Windows.Forms.CheckState.Checked> または <xref:System.Windows.Forms.CheckState.Unchecked>のいずれかを返します。または、<xref:System.Windows.Forms.CheckBox.ThreeState%2A> プロパティが `true`に設定されている場合、<xref:System.Windows.Forms.CheckBox.CheckState%2A> は <xref:System.Windows.Forms.CheckState.Indeterminate>も返すことがあります。 不確定状態では、オプションが使用できないことを示す淡色表示のボックスが表示されます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.CheckBox>
- [方法: Windows フォームの CheckBox コントロールでオプションを設定する](how-to-set-options-with-windows-forms-checkbox-controls.md)
- [方法: Windows フォームの CheckBox のクリックに応答する](how-to-respond-to-windows-forms-checkbox-clicks.md)
- [CheckBox コントロール](checkbox-control-windows-forms.md)
