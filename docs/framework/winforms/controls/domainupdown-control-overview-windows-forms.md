---
title: DomainUpDown コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- DomainUpDown
helpviewer_keywords:
- spin button control [Windows Forms], about spin button
- DomainUpDown control [Windows Forms], about DomainUpDown control
ms.assetid: 3f40f9c1-20ad-4331-b9b5-b0127eb36eb3
ms.openlocfilehash: 86703a96d4845414d043161e0efa67bfde9278db
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745863"
---
# <a name="domainupdown-control-overview-windows-forms"></a>DomainUpDown コントロールの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.DomainUpDown> コントロールは、基本的には、テキストボックスと、リストを上または下に移動するためのボタンの組み合わせで構成されます。 コントロールは、選択肢の一覧からテキスト文字列を表示して設定します。 ユーザーは上下のボタンをクリックして一覧内を移動し、上下の方向キーを押すか、リスト内の項目に一致する文字列を入力することで、文字列を選択できます。 このコントロールの1つの使用方法は、アルファベット順に並べ替えられた名前のリストから項目を選択することです。  
  
> [!NOTE]
> リストを並べ替えるには、<xref:System.Windows.Forms.DomainUpDown.Sorted%2A> プロパティを `true`に設定します。  
  
 このコントロールの機能は、リストボックスまたはコンボボックスとよく似ていますが、スペースが非常に小さくなります。  
  
## <a name="key-properties"></a>キー プロパティ  
 コントロールのキープロパティは、<xref:System.Windows.Forms.DomainUpDown.Items%2A>、<xref:System.Windows.Forms.UpDownBase.ReadOnly%2A>、および <xref:System.Windows.Forms.DomainUpDown.Wrap%2A>です。 <xref:System.Windows.Forms.DomainUpDown.Items%2A> プロパティには、コントロールにテキスト値が表示されるオブジェクトの一覧が含まれています。 <xref:System.Windows.Forms.UpDownBase.ReadOnly%2A> が `false`に設定されている場合、コントロールは、ユーザーが入力してリスト内の値と照合するテキストを自動的に完了します。 <xref:System.Windows.Forms.DomainUpDown.Wrap%2A> が `true`に設定されている場合、最後の項目を超えてスクロールすると、リストの最初の項目に移動します (その逆も同様)。 コントロールの主要なメソッドは <xref:System.Windows.Forms.DomainUpDown.UpButton%2A> と <xref:System.Windows.Forms.DomainUpDown.DownButton%2A>です。  
  
 このコントロールは、テキスト文字列のみを表示します。 数値を表示するコントロールが必要な場合は、<xref:System.Windows.Forms.NumericUpDown> コントロールを使用します。 詳細については、「 [NumericUpDown Control の概要](numericupdown-control-overview-windows-forms.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DomainUpDown>
- [DomainUpDown コントロール](domainupdown-control-windows-forms.md)
