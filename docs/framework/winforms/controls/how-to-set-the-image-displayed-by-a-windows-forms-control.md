---
title: コントロールによって表示されるイメージを設定する
ms.date: 08/20/2019
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Button control [Windows Forms], images
- Windows Forms controls, images
- controls [Windows Forms], images
- images [Windows Forms], Windows Forms controls
- examples [Windows Forms], controls
ms.assetid: 9445af8f-4f62-48b0-a3f6-068058964b9f
ms.openlocfilehash: 5df0068c8462bbaab0cb0135de1dd1b91ababe06
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746870"
---
# <a name="how-to-set-the-image-displayed-by-a-windows-forms-control"></a>方法: Windows フォームコントロールによって表示されるイメージを設定する

いくつかの Windows フォームコントロールでイメージを表示できます。 これらのイメージは、[保存] コマンドを示すボタン上のフロッピーディスクアイコンなど、コントロールの目的を明確にするアイコンにすることができます。 または、アイコンに背景画像を使用して、コントロールの外観と動作を指定することもできます。

## <a name="programmatic"></a>プログラムによる

コントロールの `Image` または `BackgroundImage` プロパティを <xref:System.Drawing.Image>型のオブジェクトに設定します。 通常は、<xref:System.Drawing.Image.FromFile%2A> メソッドを使用して、ファイルからイメージを読み込みます。

次のコード例では、イメージの場所に設定されているパスが **[マイピクチャ**] フォルダーです。 Windows オペレーティングシステムを実行しているほとんどのコンピューターには、このディレクトリが含まれています。 これにより、最小限のシステムアクセスレベルを持つユーザーも、アプリケーションを安全に実行できます。 次のコード例では、<xref:System.Windows.Forms.PictureBox> コントロールが追加されたフォームが既に存在している必要があります。

```vb
' Replace the image named below with your own icon.
PictureBox1.Image = Image.FromFile _
   (System.Environment.GetFolderPath _
   (System.Environment.SpecialFolder.MyPictures) _
   & "\Image.gif")
```

```csharp
// Replace the image named below with your own icon.
// Note the escape character used (@) when specifying the path.
pictureBox1.Image = Image.FromFile
   (System.Environment.GetFolderPath
   (System.Environment.SpecialFolder.MyPictures)
   + @"\Image.gif");
```

```cpp
// Replace the image named below with your own icon.
pictureBox1->Image = Image::FromFile(String::Concat
   (System::Environment::GetFolderPath
   (System::Environment::SpecialFolder::MyPictures),
   "\\Image.gif"));
```

## <a name="designer"></a>デザイナー

1. Visual Studio の **[プロパティ]** ウィンドウで、コントロールの**Image**または**BackgroundImage**プロパティを選択し、省略記号![(visual studio](./media/visual-studio-ellipsis-button.png)の省略記号ボタン) を選択して **[リソースの選択]** ダイアログボックスを表示します。

2. 表示するイメージを選択します。

## <a name="see-also"></a>参照

- <xref:System.Drawing.Image.FromFile%2A>
- <xref:System.Drawing.Image>
- <xref:System.Windows.Forms.Control.BackgroundImage%2A>
