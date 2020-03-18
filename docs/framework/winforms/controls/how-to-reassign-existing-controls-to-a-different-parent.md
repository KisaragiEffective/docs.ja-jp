---
title: '方法 : 既存のコントロールを別の親に再配置する'
ms.date: 03/30/2017
helpviewer_keywords:
- container controls [Windows Forms], Windows Forms
- layout [Windows Forms], resizing
- layout [Windows Forms], child controls
ms.assetid: 5a5723ff-34e0-4b6f-a57b-be4ebe35cb34
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 1767fcff1742f4ad630b4b996c709b7ded53a129
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459200"
---
# <a name="how-to-reassign-existing-controls-to-a-different-parent"></a>方法: 既存のコントロールを別の親に再割り当てする

フォームに存在するコントロールを新しいコンテナー コントロールに割り当てることができます。

1. Visual Studio で、3つの <xref:System.Windows.Forms.Button> コントロールを**ツールボックス**からフォームにドラッグします。 これらを互いに近づけて配置しますが、整列はさせません。

2. **[ツールボックス]** で <xref:System.Windows.Forms.FlowLayoutPanel> コントロール アイコンをクリックします。 (アイコンをフォームにドラッグしないでください)。

3. マウス ポインターを 3 つの <xref:System.Windows.Forms.Button> コントロールに近づけます。

   ポインターが <xref:System.Windows.Forms.FlowLayoutPanel> コントロール アイコンが付いた十字カーソルに変わります。

4. マウス ボタンを押したままにします。

5. マウス ポインターをドラッグして、 <xref:System.Windows.Forms.FlowLayoutPanel> コントロールのアウトラインを描画します。

6. 3 つの <xref:System.Windows.Forms.Button> コントロールを囲むようにアウトラインを描画します。

7. マウスのボタンを離します。

   これで、3 つの <xref:System.Windows.Forms.Button> コントロールが <xref:System.Windows.Forms.FlowLayoutPanel> コントロールに挿入されました。

## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.FlowLayoutPanel>
- <xref:System.Windows.Forms.TableLayoutPanel>
- [チュートリアル: TableLayoutPanel を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md)
- [チュートリアル: スナップ線を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-snaplines.md)
