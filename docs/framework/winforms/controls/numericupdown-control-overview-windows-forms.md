---
title: NumericUpDown コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- NumericUpDown
helpviewer_keywords:
- numeric spin button control [Windows Forms], Windows Forms
- NumericUpDown control [Windows Forms], about NumericUpDown control
- spin button control [Windows Forms], Windows Forms
ms.assetid: cff3cf30-4d46-4381-87df-37bfe83c71c5
ms.openlocfilehash: 3e24fa543df9028e9866d91ec60c3cf88578ac56
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76740805"
---
# <a name="numericupdown-control-overview-windows-forms"></a>NumericUpDown コントロールの概要 (Windows フォーム)
<xref:System.Windows.Forms.NumericUpDown> コントロールは、テキストボックスと、ユーザーがクリックして値を調整できる矢印の組み合わせのように見えます。 コントロールは、固定された数値値の選択肢の一覧から1つの数値を表示および設定します。 ユーザーは、上矢印と下矢印をクリックするか、上方向キーまたは下方向キーを押すか、コントロールのテキストボックスの部分に数値を入力することによって、数値を増減できます。 上方向キーをクリックすると、数値が最大値に移動します。下方向キーをクリックすると、数値が最小値に移動します。  
  
 このコントロールは、さまざまな用途に対応しているため、音楽プレーヤーアプリケーションのボリュームコントロールを作成する場合などに、このコントロールを選択することをお勧めします。 <xref:System.Windows.Forms.NumericUpDown> コントロールは、多くの Windows コントロールパネルアプリケーションで使用されます。  
  
## <a name="key-properties-and-methods"></a>キーのプロパティとメソッド  
 コントロールのテキストボックスに表示される数値は、16進数などのさまざまな形式にすることができます。 詳細については、「[方法: Windows フォーム NumericUpDown コントロールの形式を設定する](how-to-set-the-format-for-the-windows-forms-numericupdown-control.md)」を参照してください。 コントロールのキープロパティは、<xref:System.Windows.Forms.NumericUpDown.Value%2A>、<xref:System.Windows.Forms.NumericUpDown.Maximum%2A> (既定値は 100)、<xref:System.Windows.Forms.NumericUpDown.Minimum%2A> (既定値は 0)、および <xref:System.Windows.Forms.NumericUpDown.Increment%2A> (既定値 1) です。 <xref:System.Windows.Forms.NumericUpDown.Value%2A> プロパティは、コントロールで選択されている現在の数値を設定します。 <xref:System.Windows.Forms.NumericUpDown.Increment%2A> プロパティは、ユーザーが上矢印または下矢印をクリックしたときに、数値が調整される量を設定します。 フォーカスがコントロールから移動すると、型指定された入力は、最小値と最大値に対して検証されます。 ユーザーが <xref:System.Windows.Forms.NumericUpDown.Accelerations%2A> プロパティを使用して上矢印または下矢印を連続して押すと、コントロールが数値を移動する速度を上げることができます。 コントロールの主要なメソッドは <xref:System.Windows.Forms.NumericUpDown.UpButton%2A> と <xref:System.Windows.Forms.NumericUpDown.DownButton%2A>です。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.NumericUpDown>
- [NumericUpDown コントロール](numericupdown-control-windows-forms.md)
- [方法: Windows フォームの NumericUpDown コントロールの書式を設定する](how-to-set-the-format-for-the-windows-forms-numericupdown-control.md)
- [TextBox コントロール](textbox-control-windows-forms.md)
