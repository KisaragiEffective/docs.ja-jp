---
title: Button コントロールの概要
ms.date: 03/30/2017
f1_keywords:
- Button
helpviewer_keywords:
- Button control [Windows Forms], about Button control
- buttons [Windows Forms], about buttons
ms.assetid: 255b291b-51a9-4a92-a1a4-2400cd82443f
ms.openlocfilehash: 4ba7b333e5414efb4814d64ce50c55e08b27f859
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76747081"
---
# <a name="button-control-overview-windows-forms"></a>Button コントロールの概要 (Windows フォーム)
Windows フォームの <xref:System.Windows.Forms.Button> コントロールを使用すると、ユーザーはそれをクリックしてアクションを実行できます。 ボタンをクリックすると、ボタンを実際に押して離したかのように表示されます。 ユーザーがボタンをクリックするたびに、<xref:System.Windows.Forms.Control.Click> イベントハンドラーが呼び出されます。 <xref:System.Windows.Forms.Control.Click> イベントハンドラーにコードを配置して、選択した任意のアクションを実行します。  
  
 ボタンに表示されるテキストは、<xref:System.Windows.Forms.Control.Text%2A> プロパティに含まれています。 テキストがボタンの幅を超えている場合は、次の行に折り返されます。 ただし、コントロールが全体的な高さに対応できない場合はクリップされます。 詳細については、「[方法: Windows フォームコントロールによって表示されるテキストを設定する](how-to-set-the-text-displayed-by-a-windows-forms-control.md)」を参照してください。 <xref:System.Windows.Forms.Control.Text%2A> プロパティにはアクセスキーを含めることができます。これにより、ユーザーは ALT キーを押しながらアクセスキーを押してコントロールを "クリック" できます。 詳細については、「[方法: Windows フォームコントロールのアクセスキーを作成する](how-to-create-access-keys-for-windows-forms-controls.md)」を参照してください。 テキストの外観は、<xref:System.Windows.Forms.Control.Font%2A> プロパティと <xref:System.Windows.Forms.ButtonBase.TextAlign%2A> プロパティによって制御されます。  
  
 <xref:System.Windows.Forms.Button> コントロールは、<xref:System.Windows.Forms.ButtonBase.Image%2A> と <xref:System.Windows.Forms.ButtonBase.ImageList%2A> プロパティを使用してイメージを表示することもできます。 詳細については、「[方法: Windows フォームコントロールによって表示されるイメージを設定する](how-to-set-the-image-displayed-by-a-windows-forms-control.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Button>
- [方法: Windows フォームのボタンのクリックに応答する](how-to-respond-to-windows-forms-button-clicks.md)
- [Windows フォームの Button コントロールを選択する方法](ways-to-select-a-windows-forms-button-control.md)
- [方法: デザイナーを使用して Windows フォームの Button コントロールを承認ボタンとして指定する](designate-a-wf-button-as-the-accept-button-using-the-designer.md)
- [方法: デザイナーを使用して Windows フォームの Button コントロールをキャンセル ボタンとして指定する](designate-a-wf-button-as-the-cancel-button-using-the-designer.md)
- [Button コントロール](button-control-windows-forms.md)
