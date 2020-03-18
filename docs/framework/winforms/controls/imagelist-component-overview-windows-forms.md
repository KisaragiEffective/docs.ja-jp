---
title: ImageList コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- ImageList
helpviewer_keywords:
- collection controls [Windows Forms], images
- icon list control
- ImageList component [Windows Forms], about ImageList component
ms.assetid: 7e25d89b-5633-40c1-afc3-82e0e301ffa2
ms.openlocfilehash: b46204375cb046d637f4c9e1d888f37d10ea1f57
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76728103"
---
# <a name="imagelist-component-overview-windows-forms"></a>ImageList コンポーネントの概要 (Windows フォーム)

Windows フォーム <xref:System.Windows.Forms.ImageList> コンポーネントは、コントロールで表示するイメージの保存に使用します。 イメージ リストでは、一貫性のある 1 つのイメージのカタログのコードを記述することができます。 たとえば、ボタンの <xref:System.Windows.Forms.Button> プロパティまたは <xref:System.Windows.Forms.ButtonBase.ImageIndex%2A> プロパティを変更するだけで、<xref:System.Windows.Forms.ButtonBase.ImageKey%2A> コントロールによって表示されるイメージを回転できます。 同じイメージのリストを複数のコントロールに関連付けることもできます。 たとえば、<xref:System.Windows.Forms.ListView> コントロールと <xref:System.Windows.Forms.TreeView> コントロールの両方を使用して同じファイルのリストを表示する場合、イメージのリストでファイルのアイコンを変更すると、新しいアイコンが両方のビューに表示されます。

## <a name="using-imagelist-with-controls"></a>コントロールでの ImageList の使用

`ImageList` プロパティを持つコントロール (<xref:System.Windows.Forms.ListView> コントロールの場合は <xref:System.Windows.Forms.ListView.SmallImageList%2A> プロパティと <xref:System.Windows.Forms.ListView.LargeImageList%2A> プロパティ) でイメージ リストを使用することができます。 イメージ リストに関連付けることができるコントロールは、<xref:System.Windows.Forms.ListView>、<xref:System.Windows.Forms.TreeView>、<xref:System.Windows.Forms.ToolBar>、<xref:System.Windows.Forms.TabControl>、<xref:System.Windows.Forms.Button>、<xref:System.Windows.Forms.CheckBox>、<xref:System.Windows.Forms.RadioButton>、および <xref:System.Windows.Forms.Label> の各コントロールです。 イメージ リストをコントロールに関連付けるには、コントロールの `ImageList` プロパティを <xref:System.Windows.Forms.ImageList> コンポーネントの名前に設定します。

## <a name="key-properties"></a>キー プロパティ

<xref:System.Windows.Forms.ImageList> コンポーネントのキー プロパティは <xref:System.Windows.Forms.ImageList.Images%2A> で、関連付けられたコントロールで使用される画像が含まれています。 各イメージは、そのインデックス値、またはそのキーを使用してアクセスできます。 <xref:System.Windows.Forms.ImageList.ColorDepth%2A> プロパティは、イメージをレンダリングする際の色の数を決定します。 イメージはすべて同じサイズで表示され、<xref:System.Windows.Forms.ImageList.ImageSize%2A> プロパティによって設定されます。 大きなイメージはサイズが合うように縮小されます。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ImageList>
- [方法: Windows フォームの ImageList コンポーネントにイメージを追加または削除する](how-to-add-or-remove-images-with-the-windows-forms-imagelist-component.md)
