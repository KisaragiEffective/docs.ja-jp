---
title: ProgressBar コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- ProgressBar
helpviewer_keywords:
- ProgressBar control [Windows Forms], about ProgressBar control
- progress controls [Windows Forms], about progress controls
ms.assetid: a05d9cba-3a6a-4f8f-94b8-8ec12799fb80
ms.openlocfilehash: a0c4a0401d06614ab3ad148b932ebc64080c592c
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741279"
---
# <a name="progressbar-control-overview-windows-forms"></a>ProgressBar コントロールの概要 (Windows フォーム)
> [!IMPORTANT]
> <xref:System.Windows.Forms.ToolStripProgressBar> コントロールは、<xref:System.Windows.Forms.ProgressBar> コントロールに代わると共に追加の機能を提供します。ただし、<xref:System.Windows.Forms.ProgressBar> コントロールは、下位互換性を保つ目的および将来使用する目的で保持されます。  
  
 Windows フォーム <xref:System.Windows.Forms.ProgressBar> コントロールは、水平バーに配置された適切な数の四角形を表示することによって、プロセスの進行状況を示します。 プロセスが完了すると、バーは塗りつぶされます。 一般的に、進行状況バーは、プロセスが完了するまでの待機時間をユーザーに示すために使用されます。たとえば、大きなファイルが読み込まれている場合です。  
  
> [!NOTE]
> <xref:System.Windows.Forms.ProgressBar> コントロールは、フォーム上で水平方向にのみ配置できます。  
  
## <a name="key-properties-and-methods"></a>キーのプロパティとメソッド  
 <xref:System.Windows.Forms.ProgressBar> コントロールのキープロパティは、<xref:System.Windows.Forms.ProgressBar.Value%2A>、<xref:System.Windows.Forms.ProgressBar.Minimum%2A>、および <xref:System.Windows.Forms.ProgressBar.Maximum%2A>です。 <xref:System.Windows.Forms.ProgressBar.Minimum%2A> プロパティと <xref:System.Windows.Forms.ProgressBar.Maximum%2A> プロパティは、進行状況バーに表示できる最大値と最小値を設定します。 <xref:System.Windows.Forms.ProgressBar.Value%2A> プロパティは、操作の完了に向けた進行状況を表します。 コントロールに表示されるバーはブロックで構成されているため、<xref:System.Windows.Forms.ProgressBar> コントロールによって表示される値は、<xref:System.Windows.Forms.ProgressBar.Value%2A> プロパティの現在の値のみを概算します。 <xref:System.Windows.Forms.ProgressBar.Value%2A> プロパティは、<xref:System.Windows.Forms.ProgressBar> コントロールのサイズに基づいて、次のブロックを表示するタイミングを決定します。  
  
 現在の進行状況の値を更新する最も一般的な方法は、<xref:System.Windows.Forms.ProgressBar.Value%2A> プロパティを設定するコードを記述することです。 大きなファイルを読み込む例では、ファイルの最大サイズをキロバイト単位で設定できます。 たとえば、<xref:System.Windows.Forms.ProgressBar.Maximum%2A> プロパティが100に設定されている場合、<xref:System.Windows.Forms.ProgressBar.Minimum%2A> プロパティは10に設定され、<xref:System.Windows.Forms.ProgressBar.Value%2A> プロパティは50に設定され、5つの四角形が表示されます。 これは、表示できる数の半分です。  
  
 ただし、<xref:System.Windows.Forms.ProgressBar.Value%2A> プロパティを直接設定する以外に、<xref:System.Windows.Forms.ProgressBar> コントロールによって表示される値を変更する方法は他にもあります。 <xref:System.Windows.Forms.ProgressBar.Step%2A> プロパティを使用して、<xref:System.Windows.Forms.ProgressBar.Value%2A> プロパティをインクリメントする値を指定できます。 次に、<xref:System.Windows.Forms.ProgressBar.PerformStep%2A> メソッドを呼び出すと、値がインクリメントされます。 インクリメント値を変更するには、<xref:System.Windows.Forms.ProgressBar.Increment%2A> メソッドを使用し、<xref:System.Windows.Forms.ProgressBar.Value%2A> プロパティをインクリメントする値を指定します。  
  
 現在のアクションについてグラフィカルにユーザーに通知する別のコントロールは、<xref:System.Windows.Forms.StatusBar> コントロールです。  
  
> [!IMPORTANT]
> <xref:System.Windows.Forms.StatusStrip> コントロールと <xref:System.Windows.Forms.ToolStripStatusLabel> コントロールは、<xref:System.Windows.Forms.StatusBar> および <xref:System.Windows.Forms.StatusBarPanel> コントロールに対して機能を置き換え、追加します。ただし、<xref:System.Windows.Forms.StatusBar> および <xref:System.Windows.Forms.StatusBarPanel> のコントロールは、下位互換性と将来の使用の両方のために保持されます (選択した場合)。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ProgressBar>
- [ProgressBar コントロール](progressbar-control-windows-forms.md)
