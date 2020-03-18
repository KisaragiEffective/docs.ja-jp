---
title: '方法 : デザイナーを使って ImageList イメージを追加または削除する'
ms.date: 03/30/2017
helpviewer_keywords:
- ImageList component [Windows Forms], adding images
- ImageList component [Windows Forms], removing images
- images [Windows Forms], adding to ImageList component
ms.assetid: 5699b244-e37c-4d20-bc35-7441e55c1e3a
ms.openlocfilehash: cdc7b563a0ee4f8779b99b4e9a6786e78f8d500f
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628723"
---
# <a name="how-to-add-or-remove-imagelist-images-with-the-designer"></a>方法 : デザイナーを使って ImageList イメージを追加または削除する

<xref:System.Windows.Forms.ImageList> コンポーネントに画像を追加するには、さまざまな方法があります。 <xref:System.Windows.Forms.ImageList>に関連付けられたスマートタグを使用すると、イメージをすばやく追加できます。また、<xref:System.Windows.Forms.ImageList>で他のプロパティをいくつか設定する場合は、プロパティウィンドウに画像を追加する方が便利な場合があります。 コードを使用してイメージを追加することもできます。 コードを使用してイメージを追加する方法の詳細については、「[方法: Windows フォーム ImageList コンポーネントを使用してイメージを追加または削除](how-to-add-or-remove-images-with-the-windows-forms-imagelist-component.md)する」を参照してください。 通常は、コントロールに関連付けられる前に <xref:System.Windows.Forms.ImageList> コンポーネントにイメージを設定しますが、これは必須ではありません。

### <a name="to-add-or-remove-images-by-using-the-properties-window"></a>プロパティウィンドウを使用してイメージを追加または削除するには

1. <xref:System.Windows.Forms.ImageList> コンポーネントを選択するか、フォームに追加します。

2. プロパティウィンドウで、<xref:System.Windows.Forms.ImageList.Images%2A> プロパティの横にある省略記号ボタン![([...]) をクリックします (Visual Studio のプロパティウィンドウの](./media/visual-studio-ellipsis-button.png))。

3. **イメージコレクションエディター**で、 **[追加]** または **[削除]** をクリックして、一覧からイメージを追加または削除します。

### <a name="to-add-or-remove-images-using-the-smart-tag"></a>スマートタグを使用してイメージを追加または削除するには

1. <xref:System.Windows.Forms.ImageList> コンポーネントを選択するか、フォームに追加します。

2. デザイナーアクショングリフをクリックします (![小さい黒い矢印](./media/designer-actions-glyph.gif))

3. **[ImageList Tasks]** ダイアログボックスで、 **[Images の選択]** を選択します。

4. **イメージコレクションエディター**で、 **[追加]** または **[削除]** をクリックして、一覧からイメージを追加または削除します。

## <a name="see-also"></a>参照

- [イメージ、ビットマップ、メタファイル](../advanced/images-bitmaps-and-metafiles.md)
- [チュートリアル: デザイナーアクションを使用した一般的なタスクの実行](perform-common-tasks-design-actions.md)
- [ImageList コンポーネント](imagelist-component-windows-forms.md)
