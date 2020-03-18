---
title: デザイナーを使用して TabControl のタブを追加および削除する
ms.date: 03/30/2017
helpviewer_keywords:
- tabs [Windows Forms], removing from pages
- TabPage control
- TabPage control [Windows Forms], adding and removing tabs
- tabs [Windows Forms], adding to pages
- tab pages
ms.assetid: 480633db-413a-45d2-9c8f-0427cc13adbe
ms.openlocfilehash: 445342ffb3b53c880ac38da52076e0c0cb0a9f23
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746287"
---
# <a name="how-to-add-and-remove-tabs-with-the-windows-forms-tabcontrol-using-the-designer"></a>方法 : デザイナーで Windows フォーム TabControl を使ってタブを追加および削除する
フォームに <xref:System.Windows.Forms.TabControl> コントロールを配置すると、既定で2つのタブが表示されます。 デザイナーを使用して、タブを追加または削除できます。

 次の手順では、<xref:System.Windows.Forms.TabControl> コントロールを含むフォームを含む**Windows アプリケーション**プロジェクトが必要です。 このようなプロジェクトの設定の詳細については、「[方法: Windows フォームアプリケーションプロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)する」および「[方法: Windows フォームにコントロールを追加](how-to-add-controls-to-windows-forms.md)する」を参照してください。

## <a name="to-add-or-remove-a-tab-using-the-designer"></a>デザイナーを使用してタブを追加または削除するには

- コントロールのスマートタグで、 **[タブの追加]** または **[タブの削除]** をクリックします。

     または

     **[プロパティ]** ウィンドウで、<xref:System.Windows.Forms.TabControl.TabPages%2A> プロパティの横にある省略記号ボタン ([...]) をクリックして、[](./media/visual-studio-ellipsis-button.png)プロパティウィンドウ] をクリックし、[ **TabPage コレクションエディター** **![] を**開きます。 **[追加]** または **[削除]** ボタンをクリックします。

## <a name="see-also"></a>参照

- [TabControl コントロール](tabcontrol-control-windows-forms.md)
- [TabControl コントロールの概要](tabcontrol-control-overview-windows-forms.md)
- [方法: タブ ページにコントロールを追加する](how-to-add-a-control-to-a-tab-page.md)
- [方法: タブ ページを無効化する](how-to-disable-tab-pages.md)
- [方法: Windows フォーム TabControl の表示形式を変更する](how-to-change-the-appearance-of-the-windows-forms-tabcontrol.md)
