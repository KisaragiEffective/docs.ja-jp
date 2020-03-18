---
title: デザイナーを使用してパネルの背景を設定する
ms.date: 03/30/2017
helpviewer_keywords:
- background colors [Windows Forms], Windows Forms Panel controls
- background images [Windows Forms], Windows Forms Panel controls
- Panel control [Windows Forms], background
- colors [Windows Forms], Windows Forms Panel controls
ms.assetid: db83cf54-3c69-4b08-ac6c-25b9b5abb1b0
ms.openlocfilehash: 8bdefba433632f7ba02f549a549c52c7aa56c2d7
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76731920"
---
# <a name="how-to-set-the-background-of-a-windows-forms-panel-using-the-designer"></a>方法: デザイナーを使用して Windows フォームパネルの背景を設定する

Windows フォーム <xref:System.Windows.Forms.Panel> コントロールには、背景色と背景画像の両方を表示できます。 <xref:System.Windows.Forms.Control.BackColor%2A> プロパティは、ラベルやラジオボタンなど、パネルに含まれるコントロールの背景色を設定します。 <xref:System.Windows.Forms.Control.BackgroundImage%2A> プロパティが設定されていない場合、<xref:System.Windows.Forms.Control.BackColor%2A> 選択によってパネル全体が表示されます。 <xref:System.Windows.Forms.Control.BackgroundImage%2A> プロパティが設定されている場合、パネルに含まれているコントロールの背後に画像が表示されます。

次の手順では、<xref:System.Windows.Forms.Panel> コントロールを含むフォームを含む**Windows アプリケーション**プロジェクトが必要です。 Visual Studio でこのようなプロジェクトを設定する方法の詳細については、「[方法: Windows フォームアプリケーションプロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)する」および「[方法: Windows フォームにコントロールを追加](how-to-add-controls-to-windows-forms.md)する」を参照してください。

## <a name="set-the-background-in-the-windows-forms-designer"></a>Windows フォームデザイナーの背景を設定する

1. Visual Studio でプロジェクトを開き、[<xref:System.Windows.Forms.Panel>] コントロールを選択します。

2. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.Control.BackColor%2A>] プロパティの横にある矢印ボタンをクリックして、3つのタブを含むウィンドウを表示します。

3. **[カスタム]** タブを選択して、色のパレットを表示します。

4. **[Web]** または **[システム]** タブを選択して、色の定義済みの名前の一覧を表示し、色を選択します。

5. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.Control.BackgroundImage%2A>] プロパティの横にある矢印ボタンをクリックします。

6. **[開く]** ダイアログボックスで、表示するファイルを選択します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Control.BackColor%2A>
- <xref:System.Windows.Forms.Control.BackgroundImage%2A>
- [Panel コントロール](panel-control-windows-forms.md)
- [Panel コントロールの概要](panel-control-overview-windows-forms.md)
- [方法: デザイナーを使用して Windows フォーム Panel コントロールでコントロールをグループ化する](group-controls-with-wf-panel-control-using-the-designer.md)
