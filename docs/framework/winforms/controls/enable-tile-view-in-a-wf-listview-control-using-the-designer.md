---
title: デザイナーを使用して ListView コントロールのタイルビューを有効にする
ms.date: 03/30/2017
helpviewer_keywords:
- tile view feature
- ListView control [Windows Forms], tile view
- tiling [Windows Forms], Windows Forms, controls
ms.assetid: 12f0816a-52b8-41ee-a6d9-ded3a8a5817a
ms.openlocfilehash: a0429efaab14995ab1e3f3b0dfd91db61de72fbf
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745802"
---
# <a name="how-to-enable-tile-view-in-a-windows-forms-listview-control-using-the-designer"></a>方法 : デザイナーを使って Windows フォーム ListView コントロールの "並べて表示" ビューを有効にする
<xref:System.Windows.Forms.ListView> コントロールのタイルビュー機能を使用すると、グラフィカルな情報とテキスト情報の間で視覚的なバランスを取ることができます。 並べて表示ビューの項目で表示されるテキスト情報は、詳細ビュー用に定義されている列情報と同じ情報です。 タイルビューは、<xref:System.Windows.Forms.ListView> コントロールのグループ化機能または挿入マーク機能と組み合わせて機能します。

 タイルビューでは、次の図に示すように、32 x 32 アイコンと数行のテキストが使用されます。

 ![ListView コントロール内の並べて表示ビュー](./media/enable-tile-view-in-a-wf-listview-control-using-the-designer/tile-view-in-listview-control.gif "並べて表示ビューのアイコンとテキスト")

 タイルビューのプロパティとメソッドを使用すると、各項目に対して表示する列フィールドを指定したり、タイルビューウィンドウ内のすべての項目のサイズと外観をまとめて制御したりすることができます。 わかりやすくするために、タイルのテキストの最初の行は常に項目の名前です。変更することはできません。

 次の手順では、<xref:System.Windows.Forms.ListView> コントロールを含むフォームを含む**Windows アプリケーション**プロジェクトが必要です。 このようなプロジェクトの設定の詳細については、「[方法: Windows フォームアプリケーションプロジェクトを作成](/visualstudio/ide/step-1-create-a-windows-forms-application-project)する」および「[方法: Windows フォームにコントロールを追加](how-to-add-controls-to-windows-forms.md)する」を参照してください。

## <a name="to-set-tile-view-in-the-designer"></a>デザイナーでタイルビューを設定するには

1. Visual Studio で、フォームの [<xref:System.Windows.Forms.ListView>] コントロールを選択します。

2. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.ListView.View%2A>] プロパティを選択し、 **[タイル]** を選択します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ListView.TileSize%2A>
- [ListView コントロールの概要](listview-control-overview-windows-forms.md)
