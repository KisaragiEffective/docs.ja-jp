---
title: ColorDialog コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- ColorDialog
helpviewer_keywords:
- color dialog box [Windows Forms], about color dialog box
- ColorDialog component [Windows Forms], about ColorDialog
ms.assetid: 6dbdd8f0-f697-4728-bb09-7ea156f6d800
ms.openlocfilehash: 48d9d5072335908f85e65933dadafb1b69f28528
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736966"
---
# <a name="colordialog-component-overview-windows-forms"></a>ColorDialog コンポーネントの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.ColorDialog> コンポーネントは、ユーザーがパレットから色を選択し、そのパレットにカスタムの色を追加できるようにする、事前に構成されたダイアログボックスです。 このダイアログ ボックスは、他の Windows ベースのアプリケーションで表示される色を選択するためのダイアログ ボックスと同じです。 このコントロールは、独自のダイアログ ボックスを使用しない簡易ソリューションとして、Windows ベースのアプリケーションの中で使用します。  
  
 ダイアログボックスで選択した色が <xref:System.Windows.Forms.ColorDialog.Color%2A> プロパティに返されます。 <xref:System.Windows.Forms.ColorDialog.AllowFullOpen%2A> プロパティが `false`に設定されている場合、[ユーザー設定の色の定義] ボタンは無効になり、ユーザーはパレットの定義済みの色に制限されます。 <xref:System.Windows.Forms.ColorDialog.SolidColorOnly%2A> プロパティが `true`に設定されている場合、ユーザーはディザーカラーを選択できません。 ダイアログボックスを表示するには、<xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> メソッドを呼び出す必要があります。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ColorDialog>
- [ColorDialog コンポーネント](colordialog-component-windows-forms.md)
- [ダイアログ ボックス コントロールおよびコンポーネント](dialog-box-controls-and-components-windows-forms.md)
- [方法: Windows フォーム ColorDialog コンポーネントの表示形式を変更する](how-to-change-the-appearance-of-the-windows-forms-colordialog-component.md)
