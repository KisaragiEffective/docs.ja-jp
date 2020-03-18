---
title: DomainUpDown コントロール
ms.date: 03/30/2017
helpviewer_keywords:
- DomainUpDown control [Windows Forms]
- spin button control [Windows Forms], up-down controls
- up-down controls
- spin button control
- up-down controls [Windows Forms], spin button controls
ms.assetid: fb7cf017-e931-4a95-9d21-8caee4ee122a
ms.openlocfilehash: b538350a84e341d6b2759a7db28f8799f3777a86
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745829"
---
# <a name="domainupdown-control-windows-forms"></a>DomainUpDown コントロール (Windows フォーム)
Windows フォームの <xref:System.Windows.Forms.DomainUpDown> コントロールは、テキストボックスと、リストを上または下に移動するためのボタンのペアの組み合わせに似ています。 コントロールは、選択肢の一覧からテキスト文字列を表示して設定します。 ユーザーは上下のボタンをクリックして一覧内を移動し、上下の方向キーを押すか、リスト内の項目に一致する文字列を入力することで、文字列を選択できます。 このコントロールの1つの使用方法は、アルファベット順に並べ替えられた名前のリストから項目を選択することです。 (一覧を並べ替えるには、[<xref:System.Windows.Forms.DomainUpDown.Sorted%2A>] プロパティを `true`に設定します)。このコントロールの機能は、リストボックスまたはコンボボックスとよく似ていますが、スペースが非常に小さくなります。  
  
 コントロールのキープロパティは、<xref:System.Windows.Forms.DomainUpDown.Items%2A>、<xref:System.Windows.Forms.UpDownBase.ReadOnly%2A>、および <xref:System.Windows.Forms.DomainUpDown.Wrap%2A>です。 <xref:System.Windows.Forms.DomainUpDown.Items%2A> プロパティには、コントロールにテキスト値が表示されるオブジェクトの一覧が含まれています。 <xref:System.Windows.Forms.UpDownBase.ReadOnly%2A> が `false`に設定されている場合、コントロールは、ユーザーが入力してリスト内の値と照合するテキストを自動的に完了します。 <xref:System.Windows.Forms.DomainUpDown.Wrap%2A> が `true`に設定されている場合、最後の項目を超えてスクロールすると、リストの最初の項目に移動します (その逆も同様)。 コントロールの主要なメソッドは <xref:System.Windows.Forms.DomainUpDown.UpButton%2A> と <xref:System.Windows.Forms.DomainUpDown.DownButton%2A>です。  
  
 このコントロールは、テキスト文字列のみを表示します。 数値を表示するコントロールが必要な場合は、<xref:System.Windows.Forms.NumericUpDown> コントロールを使用します。 詳細については、「 [NumericUpDown Control](numericupdown-control-windows-forms.md) 」を参照してください。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [DomainUpDown コントロールの概要](domainupdown-control-overview-windows-forms.md)  
 <xref:System.Windows.Forms.DomainUpDown> コントロールの一般的な概念について説明します。これにより、ユーザーがテキスト文字列の一覧から参照したり選択したりできるようになります。  
  
 [方法: Windows フォーム DomainUpDown コントロールにプログラムで項目を追加する](how-to-add-items-to-windows-forms-domainupdown-controls-programmatically.md)  
 <xref:System.Windows.Forms.DomainUpDown> コントロールに表示するテキスト文字列を指定する方法について説明します。  
  
 [方法: Windows フォームの DomainUpDown コントロールから項目を削除する](how-to-remove-items-from-windows-forms-domainupdown-controls.md)  
 コード内の <xref:System.Windows.Forms.DomainUpDown> コントロールから項目を削除する方法について説明します。  
  
## <a name="reference"></a>リファレンス  
 <xref:System.Windows.Forms.DomainUpDown>  
 このクラスについて説明し、すべてのメンバーへのリンクの一覧を示します。  
  
 <xref:System.Windows.Forms.NumericUpDown>  
 このクラスについて説明し、すべてのメンバーへのリンクを示します。「」をご紹介します。  
  
## <a name="related-sections"></a>関連項目  
 [Windows フォームで使用するコントロール](controls-to-use-on-windows-forms.md)  
 Windows フォーム コントロールの完全な一覧を、使用に関する情報リンクと共に提供します。
