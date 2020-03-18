---
title: デザイナーを使用してボタンを [キャンセル] ボタンとして指定する
ms.date: 03/30/2017
helpviewer_keywords:
- buttons [Windows Forms], cancel buttons
- Button control [Windows Forms], designating as cancel button
ms.assetid: 30e77d9c-d565-4ab5-a84a-62c043af8822
ms.openlocfilehash: 95d1139b6e339055f944ad0b0967a6d91283d49e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746051"
---
# <a name="how-to-designate-a-windows-forms-button-as-the-cancel-button-using-the-designer"></a>方法 : デザイナーを使用して Windows フォームの Button コントロールをキャンセル ボタンとして指定する
任意の Windows フォームで、[キャンセル] ボタンとして <xref:System.Windows.Forms.Button> コントロールを指定できます。 フォーム上の他のコントロールにフォーカスがあるかどうかに関係なく、ユーザーが ESC キーを押すたびに [キャンセル] ボタンがクリックされます。 このようなボタンは、通常、ユーザーがアクションをコミットせずに操作をすばやく終了できるようにプログラミングされています。

## <a name="to-designate-the-cancel-button"></a>[キャンセル] ボタンを指定するには

1. ボタンが存在するフォームを選択します。

2. **[プロパティ]** ウィンドウで、フォームの <xref:System.Windows.Forms.Form.CancelButton%2A> プロパティを <xref:System.Windows.Forms.Button> コントロールの名前に設定します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Form.CancelButton%2A>
- [Button コントロールの概要](button-control-overview-windows-forms.md)
- [Windows フォームの Button コントロールを選択する方法](ways-to-select-a-windows-forms-button-control.md)
- [方法: Windows フォームのボタンのクリックに応答する](how-to-respond-to-windows-forms-button-clicks.md)
- [方法: デザイナーを使用して Windows フォームの Button コントロールを承認ボタンとして指定する](designate-a-wf-button-as-the-accept-button-using-the-designer.md)
- [Button コントロール](button-control-windows-forms.md)
