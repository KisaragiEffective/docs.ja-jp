---
title: デザイナーを使用して Panel コントロールでコントロールをグループ化する
ms.date: 03/30/2017
helpviewer_keywords:
- Panel control [Windows Forms], grouping controls
- controls [Windows Forms], grouping
- Windows Forms controls, grouping
ms.assetid: 7e1cd708-fdb1-49d8-9ca2-5640b276bf2e
ms.openlocfilehash: 927aeb5b51bc1ac4e22a45e487b49424b4e67b71
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745655"
---
# <a name="how-to-group-controls-with-the-windows-forms-panel-control-using-the-designer"></a>方法 : デザイナーを使用して Windows フォーム Panel コントロールでコントロールをグループ化する
Windows フォーム <xref:System.Windows.Forms.Panel> コントロールは、他のコントロールをグループ化するために使用されます。 コントロールをグループ化する理由は3つあります。 1つは、明確なユーザーインターフェイスに関連するフォーム要素を視覚的にグループ化することです。また、オプションボタンのプログラムによるグループ化もあります。次に例を示します。最後に、デザイン時にコントロールを1つの単位として移動します。

## <a name="to-create-a-group-of-controls"></a>コントロールのグループを作成するには

1. ツールボックスの **[Windows フォーム]** タブから <xref:System.Windows.Forms.Panel> コントロールをフォームにドラッグします。

2. パネルに他のコントロールを追加し、各コントロールをパネル内に描画します。

     パネルに表示する既存のコントロールがある場合は、すべてのコントロールを選択してクリップボードに貼り付け、<xref:System.Windows.Forms.Panel> コントロールを選択して、パネルに貼り付けることができます。 また、パネルにドラッグすることもできます。

3. Optionalパネルに罫線を追加する場合は、その <xref:System.Windows.Forms.BorderStyle> プロパティを設定します。 <xref:System.Windows.Forms.BorderStyle.Fixed3D>、<xref:System.Windows.Forms.BorderStyle.FixedSingle>、<xref:System.Windows.Forms.BorderStyle.None>の3つの選択肢があります。

## <a name="see-also"></a>参照

- [Panel コントロール](panel-control-windows-forms.md)
- [Panel コントロールの概要](panel-control-overview-windows-forms.md)
- [方法: パネルの背景を設定する](how-to-set-the-background-of-a-windows-forms-panel.md)
