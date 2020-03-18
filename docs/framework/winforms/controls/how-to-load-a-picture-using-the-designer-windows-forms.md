---
title: '方法 : デザイナーを使用してピクチャを読み込む'
ms.date: 03/30/2017
helpviewer_keywords:
- picture formats
- images [Windows Forms], displaying on Windows Forms
- pictures [Windows Forms], displaying
- forms [Windows Forms], displaying images
- PictureBox control [Windows Forms], adding pictures
ms.assetid: 4dc7b973-afb1-4276-8322-20825af96655
ms.openlocfilehash: 12b90d561a18fcffaafb9c45b7fa6be6dd060215
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736330"
---
# <a name="how-to-load-a-picture-using-the-designer-windows-forms"></a>方法 : デザイナーを使用してピクチャを読み込む (Windows フォーム)

Windows フォーム <xref:System.Windows.Forms.PictureBox> コントロールを使用すると、<xref:System.Windows.Forms.PictureBox.Image%2A> プロパティを有効な画像に設定することによって、デザイン時にフォームの画像を読み込んで表示することができます。 次の表に、許容されるファイルの種類を示します。

|種類|ファイル名拡張子|
|---|---|
|Bitmap|.bmp|
|アイコン|.ico|
|GIF|.gif|
|Metafile|.wmf|
|JPEG|.jpg|

## <a name="to-display-a-picture-at-design-time"></a>デザイン時に画像を表示するには

1. フォームに <xref:System.Windows.Forms.PictureBox> コントロールを描画します。

2. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.PictureBox.Image%2A>] プロパティを選択し、省略記号ボタンを選択して **[開く]** ダイアログボックスを表示します。

3. 特定のファイルの種類 (.gif ファイルなど) を探している場合は、 **[ファイルの種類]** ボックスでそれを選択します。

4. 表示するファイルを選択します。

## <a name="to-clear-the-picture-at-design-time"></a>デザイン時に画像をクリアするには

1. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.PictureBox.Image%2A>] プロパティを選択します。 イメージオブジェクトの名前の左側に表示される小さいサムネイル画像を右クリックし、 **[リセット]** を選択します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.PictureBox>
- [PictureBox コントロールの概要](picturebox-control-overview-windows-forms.md)
- [方法: 実行時にピクチャのサイズまたは配置を変更する](how-to-modify-the-size-or-placement-of-a-picture-at-run-time-windows-forms.md)
- [方法: 実行時にピクチャを設定する](how-to-set-pictures-at-run-time-windows-forms.md)
- [PictureBox コントロール](picturebox-control-windows-forms.md)
