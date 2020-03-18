---
title: LinkLabel コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- LinkLabel
helpviewer_keywords:
- links [Windows Forms], LinkLabel control
- Label control [Windows Forms], about Label control
- LinkLabel control [Windows Forms], about LinkLabel control
ms.assetid: 9e248549-10ca-43a3-bb5e-60f583d369f1
ms.openlocfilehash: 9837902bf56a402d623adbcf41558dcc568b7105
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745225"
---
# <a name="linklabel-control-overview-windows-forms"></a>LinkLabel コントロールの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.LinkLabel> コントロールを使用すると、Windows フォームアプリケーションに Web スタイルのリンクを追加できます。 <xref:System.Windows.Forms.LinkLabel> コントロールを使用して、の <xref:System.Windows.Forms.Label> コントロールを使用できるすべての操作を行うことができます。また、テキストの一部を、ファイル、フォルダー、または Web ページへのリンクとして設定することもできます。  
  
## <a name="what-you-can-do-with-the-linklabel-control"></a>LinkLabel コントロールでできること  
 <xref:System.Windows.Forms.LinkLabel> コントロールには、<xref:System.Windows.Forms.Label> コントロールのすべてのプロパティ、メソッド、およびイベントに加え、ハイパーリンクおよびリンクの色のプロパティがあります。 <xref:System.Windows.Forms.LinkLabel.LinkArea%2A> プロパティは、リンクをアクティブにするテキストの領域を設定します。 <xref:System.Windows.Forms.LinkLabel.LinkColor%2A>、<xref:System.Windows.Forms.LinkLabel.VisitedLinkColor%2A>、および <xref:System.Windows.Forms.LinkLabel.ActiveLinkColor%2A> の各プロパティは、リンクの色を設定します。 <xref:System.Windows.Forms.LinkLabel.LinkClicked> イベントによって、リンクテキストが選択されたときの動作が決まります。  
  
 <xref:System.Windows.Forms.LinkLabel> コントロールの最も簡単な用途は、<xref:System.Windows.Forms.LinkLabel.LinkArea%2A> プロパティを使用して1つのリンクを表示することですが、<xref:System.Windows.Forms.LinkLabel.Links%2A> プロパティを使用して複数のハイパーリンクを表示することもできます。 <xref:System.Windows.Forms.LinkLabel.Links%2A> プロパティを使用すると、リンクのコレクションにアクセスできます。 個々の <xref:System.Windows.Forms.LinkLabel.Link> オブジェクトの <xref:System.Windows.Forms.LinkLabel.Link.LinkData%2A> プロパティでデータを指定することもできます。 <xref:System.Windows.Forms.LinkLabel.Link.LinkData%2A> プロパティの値は、表示するファイルの場所または Web サイトのアドレスを格納するために使用できます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.LinkLabel>
- [Label コントロールの概要](label-control-overview-windows-forms.md)
- [方法: Windows フォーム LinkLabel コントロールでオブジェクトまたは Web ページにリンクする](link-to-an-object-or-web-page-with-wf-linklabel-control.md)
- [方法: Windows フォーム LinkLabel コントロールの表示形式を変更する](how-to-change-the-appearance-of-the-windows-forms-linklabel-control.md)
