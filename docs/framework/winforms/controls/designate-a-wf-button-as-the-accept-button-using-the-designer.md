---
title: デザイナーを使用してボタンを [Accept] ボタンとして指定する
ms.date: 03/30/2017
helpviewer_keywords:
- buttons [Windows Forms], default on Windows Forms
- Accept button on Windows Forms
- Button control [Windows Forms], designating as default
- Windows Forms controls, default button on form
ms.assetid: a1da0590-755f-49f2-aca7-609fac6351bf
ms.openlocfilehash: 27a5de51f8c5b2c84cc205e09ac1a2179c315752
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746067"
---
# <a name="how-to-designate-a-windows-forms-button-as-the-accept-button-using-the-designer"></a>方法 : デザイナーを使用して Windows フォームの Button コントロールを承認ボタンとして指定する
任意の Windows フォームで、[accept] ボタンとして <xref:System.Windows.Forms.Button> コントロールを指定できます (既定のボタンとも呼ばれます)。 ユーザーが ENTER キーを押すたびに、フォーム上の他のコントロールにフォーカスがあるかどうかに関係なく、既定のボタンがクリックされます。 この例外は、フォーカスのあるコントロールが別のボタンである場合です。この場合、フォーカスがあるボタン (複数行のテキストボックス)、または ENTER キーをトラップするカスタムコントロールがクリックされます。

## <a name="to-designate-the-accept-button"></a>Accept ボタンを指定するには

1. ボタンが存在するフォームを選択します。

2. **[プロパティ]** ウィンドウで、フォームの <xref:System.Windows.Forms.Form.AcceptButton%2A> プロパティを <xref:System.Windows.Forms.Button> コントロールの名前に設定します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Form.AcceptButton%2A>
- [Button コントロールの概要](button-control-overview-windows-forms.md)
- [Windows フォームの Button コントロールを選択する方法](ways-to-select-a-windows-forms-button-control.md)
- [方法: Windows フォームのボタンのクリックに応答する](how-to-respond-to-windows-forms-button-clicks.md)
- [方法: デザイナーを使用して Windows フォームの Button コントロールをキャンセル ボタンとして指定する](designate-a-wf-button-as-the-cancel-button-using-the-designer.md)
- [Button コントロール](button-control-windows-forms.md)
