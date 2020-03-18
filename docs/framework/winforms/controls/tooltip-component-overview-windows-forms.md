---
title: ToolTip コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- ToolTip
helpviewer_keywords:
- tooltips [Windows Forms], about tooltips
- ToolTip component [Windows Forms], about ToolTip component
ms.assetid: 3fbc6f08-c882-4acd-a960-a08efe3c7e6e
ms.openlocfilehash: 731f7ad0ce6670000d8c3d3e901e1506f7ef470a
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741907"
---
# <a name="tooltip-component-overview-windows-forms"></a>ToolTip コンポーネントの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.ToolTip> コンポーネントは、ユーザーがコントロールをポイントしたときにテキストを表示します。 ツールヒントは任意のコントロールに関連付けることができます。 このコンポーネントの使用例: フォーム上の領域を節約するには、ボタンに小さいアイコンを表示し、ツールヒントを使用してボタンの機能を説明します。  
  
## <a name="working-with-the-tooltip-component"></a>ツールヒントコンポーネントの操作  
 <xref:System.Windows.Forms.ToolTip> コンポーネントは、Windows フォームまたは他のコンテナーの複数のコントロールに `ToolTip` プロパティを提供します。 たとえば、フォームに1つの <xref:System.Windows.Forms.ToolTip> コンポーネントを配置した場合、<xref:System.Windows.Forms.TextBox> コントロールの "ここに名前を入力してください" と <xref:System.Windows.Forms.Button> コントロールの [ここをクリックして変更を保存します] を表示できます。  
  
 <xref:System.Windows.Forms.ToolTip> コンポーネントの主要なメソッドは <xref:System.Windows.Forms.ToolTip.SetToolTip%2A> と <xref:System.Windows.Forms.ToolTip.GetToolTip%2A>です。 <xref:System.Windows.Forms.ToolTip.SetToolTip%2A> メソッドを使用して、コントロールに表示されるツールヒントを設定できます。 詳細については、「[方法: デザイン時に Windows フォームのコントロールにツールヒントを設定する](how-to-set-tooltips-for-controls-on-a-windows-form-at-design-time.md)」を参照してください。 キーのプロパティは <xref:System.Windows.Forms.ToolTip.Active%2A>です。ツールヒントを表示するには、`true` に設定する必要があります。 <xref:System.Windows.Forms.ToolTip.AutomaticDelay%2A>また、ツールヒントの文字列を表示する時間の長さ、ツールヒントを表示するためにユーザーがコントロールをポイントする必要がある期間、およびその後のツールヒントウィンドウが表示されるまでの時間を設定します。 詳細については、「[方法: Windows フォーム ToolTip コンポーネントの遅延を変更する](how-to-change-the-delay-of-the-windows-forms-tooltip-component.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ToolTip>
- [方法: デザイン時に Windows フォームのコントロールにツールヒントを設定する](how-to-set-tooltips-for-controls-on-a-windows-form-at-design-time.md)
- [方法: Windows フォームの ToolTip コンポーネントの遅延時間を変更する](how-to-change-the-delay-of-the-windows-forms-tooltip-component.md)
