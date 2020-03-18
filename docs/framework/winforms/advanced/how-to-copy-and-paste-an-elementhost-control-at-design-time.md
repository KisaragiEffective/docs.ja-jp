---
title: '方法 : デザイン時に ElementHost コントロールをコピーして貼り付ける'
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Forms, content copying and pasting
- interoperability [WPF]
- ElementHost control [Windows Forms], copying and pasting at design time
- WPF user control [Windows Forms], hosting in Windows Forms
ms.assetid: e570375d-2a68-44ba-b4f7-c781af2d20e8
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: e89510558274558e560bf810afe746e250ff26a4
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459231"
---
# <a name="how-to-copy-and-paste-an-elementhost-control"></a>方法: エンティティコントロールをコピーして貼り付ける

この手順では、Visual Studio の Windows フォームで Windows Presentation Foundation (WPF) コントロールをコピーする方法について説明します。

1. Visual Studio で、新しい WPF <xref:System.Windows.Controls.UserControl> を Windows フォームプロジェクトに追加します。 コントロール型の既定の名前である `UserControl1.xaml` を使用します。 詳細については、「[チュートリアル: デザイン時の Windows フォームでの新しい WPF コンテンツの作成](walkthrough-creating-new-wpf-content-on-windows-forms-at-design-time.md)」を参照してください。

2. **[プロパティ]** ウィンドウで、`UserControl1` の <xref:System.Windows.FrameworkElement.Width%2A> と <xref:System.Windows.FrameworkElement.Height%2A> のプロパティの値を**200**に設定します。

3. <xref:System.Windows.Controls.Control.Background%2A> プロパティの値を**Blue**に設定します。

4. プロジェクトをビルドします。

5. Windows フォーム デザイナーで `Form1` を開きます。

6. **[ツールボックス]** から `UserControl1` のインスタンスをフォームにドラッグします。

   `UserControl1` のインスタンスは、`elementHost1` という名前の新しい <xref:System.Windows.Forms.Integration.ElementHost> コントロールでホストされます。

7. `elementHost1` 選択した状態で、 **Ctrl**+**C**キーを押してクリップボードにコピーします。

8. **Ctrl**+**V**キーを押して、コピーしたコントロールをフォームに貼り付けます。

   `elementHost2` という名前の新しい <xref:System.Windows.Forms.Integration.ElementHost> コントロールがフォーム上に作成されます。

## <a name="see-also"></a>関連項目

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [移行と相互運用性](../../wpf/advanced/migration-and-interoperability.md)
- [WPF コントロールの使用](using-wpf-controls.md)
- [Visual Studio で XAML をデザインする](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)
