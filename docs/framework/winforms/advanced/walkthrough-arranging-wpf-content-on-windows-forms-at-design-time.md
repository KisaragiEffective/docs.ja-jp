---
title: デザイン時に Windows フォームに WPF コンテンツを配置する
titleSuffix: ''
ms.date: 03/30/2017
helpviewer_keywords:
- WPF user control [Windows Forms], hosting in a layout panel
- WPF content [Windows Forms], arranging at design time
- Windows Forms, arranging WPF content at design time
- WPF content [Windows Forms], hosting in Windows Forms
- Windows Forms, anchoring and docking WPF content
- interoperability [WPF]
ms.assetid: 5efb1c53-1484-43d6-aa8a-f4861b99bb8a
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 5a6b12def45052e117fb149555946ea42d6cd3c2
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746822"
---
# <a name="walkthrough-arrange-wpf-content-on-windows-forms-at-design-time"></a>チュートリアル: デザイン時の Windows フォームでの WPF コンテンツの配置

この記事では、アンカーやスナップ線などの Windows フォームのレイアウト機能を使用して Windows Presentation Foundation (WPF) コントロールを配置する方法について説明します。

## <a name="prerequisites"></a>前提条件

このチュートリアルを完了するには Visual Studio が必要です。

## <a name="create-the-project"></a>プロジェクトを作成する

Visual Studio を開き、Visual Basic または `ArrangeElementHost`C#という名前の新しい Windows フォームアプリケーションプロジェクトを作成します。

> [!NOTE]
> WPF コンテンツをホストする場合は、C# プロジェクトと Visual Basic プロジェクトのみがサポートされます。

## <a name="create-the-wpf-control"></a>WPF コントロールの作成

プロジェクトに WPF コントロール型を追加したら、フォーム状に配置できます。

1. 新しい WPF <xref:System.Windows.Controls.UserControl> をプロジェクトに追加します。 コントロール型の既定の名前である `UserControl1.xaml` を使用します。 詳細については、「[チュートリアル: デザイン時の Windows フォームでの新しい WPF コンテンツの作成](walkthrough-creating-new-wpf-content-on-windows-forms-at-design-time.md)」を参照してください。

2. デザイン ビューで `UserControl1` が選択されていることを確認します。

3. **[プロパティ]** ウィンドウで、<xref:System.Windows.FrameworkElement.Width%2A> と <xref:System.Windows.FrameworkElement.Height%2A> のプロパティの値を**200**に設定します。

4. <xref:System.Windows.Controls.Control.Background%2A> プロパティの値を**Blue**に設定します。

5. プロジェクトをビルドします。

## <a name="host-wpf-controls-in-a-layout-panel"></a>レイアウトパネルで WPF コントロールをホストする

その他の Windows フォーム コントロールを使用するのと同じ方法で、レイアウト パネルで WPF コントロールを使用できます。

1. Windows フォーム デザイナーで `Form1` を開きます。

2. **[ツールボックス]** で、<xref:System.Windows.Forms.TableLayoutPanel> コントロールをフォームにドラッグします。

3. <xref:System.Windows.Forms.TableLayoutPanel> コントロールのスマートタグパネルで、 **[最終行の削除]** を選択します。

4. 幅と高さが大きくなるよう <xref:System.Windows.Forms.TableLayoutPanel> コントロールのサイズを変更します。

5. **ツールボックス**で、[`UserControl1`] をダブルクリックして、<xref:System.Windows.Forms.TableLayoutPanel> コントロールの最初のセルに `UserControl1` のインスタンスを作成します。

   `UserControl1` のインスタンスは、<xref:System.Windows.Forms.Integration.ElementHost> という名前の新しい `elementHost1` コントロールでホストされます。

6. **ツールボックス**で、[`UserControl1`] をダブルクリックして、<xref:System.Windows.Forms.TableLayoutPanel> コントロールの2番目のセルに別のインスタンスを作成します。

7. **[ドキュメントアウトライン]** ウィンドウで、[`tableLayoutPanel1`] を選択します。

8. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.Control.Padding%2A>] プロパティの値を**10、** 10、10、10に設定します。

   両方の <xref:System.Windows.Forms.Integration.ElementHost> コントロールが、新しいレイアウトに収まるようにサイズ変更されました。

## <a name="use-snaplines-to-align-wpf-controls"></a>スナップ線を使用した WPF コントロールの配置

スナップ線により、フォームのコントロールの配置を簡単に調整できます。 スナップ線を使用して、WPF コントロールも配置することができます。 詳細については、「[チュートリアル: スナップ線を使用した Windows フォームでのコントロールの配置](../controls/walkthrough-arranging-controls-on-windows-forms-using-snaplines.md)」を参照してください。

1. **ツールボックス**から `UserControl1` のインスタンスをフォームにドラッグし、<xref:System.Windows.Forms.TableLayoutPanel> コントロールの下の領域に配置します。

   `UserControl1` のインスタンスは、<xref:System.Windows.Forms.Integration.ElementHost> という名前の新しい `elementHost3` コントロールでホストされます。

2. スナップ線を使用して、`elementHost3` の左端を <xref:System.Windows.Forms.TableLayoutPanel> コントロールの左端に揃えます。

3. スナップ線を使用して、`elementHost3` のサイズを <xref:System.Windows.Forms.TableLayoutPanel> コントロールと同じ幅にします。

4. コントロール間に中央揃えのスナップ線が表示されるまで`elementHost3` を <xref:System.Windows.Forms.TableLayoutPanel> コントロールの方へ移動します。

5. **[プロパティ]** ウィンドウで、Margin プロパティの値を20、20、20、20に設定**します。**

6. 中央揃えのスナップ線がもう一度表示されるまで、`elementHost3` を <xref:System.Windows.Forms.TableLayoutPanel> コントロールから移動します。 中央揃えのスナップ線が、余白 20 を示すようになりました。

7. 左端が `elementHost1`の左端に揃うまで `elementHost3` を右に移動します。

8. 右端が `elementHost3` の右端に配置されるまで、`elementHost2` の幅を変更します。

## <a name="anchor-and-dock-wpf-controls"></a>WPF コントロールのアンカーとドッキング

フォームでホストされている WPF コントロールは、他の Windows フォーム コントロールと同じ固定とドッキングの動作を持ちます。

1. [`elementHost1`] を選択します。

2. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.Control.Anchor%2A>] プロパティを [**上]、[下]、[左]、[右**] に設定します。

3. <xref:System.Windows.Forms.TableLayoutPanel> コントロールを大きなサイズに変更します。

   `elementHost1` コントロールがセルを満たすようサイズ変更されます。

4. [`elementHost2`] を選択します。

5. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.Control.Dock%2A>] プロパティの値を <xref:System.Windows.Forms.DockStyle.Fill>に設定します。

   `elementHost2` コントロールがセルを満たすようサイズ変更されます。

6. <xref:System.Windows.Forms.TableLayoutPanel> コントロールを選択します。

7. <xref:System.Windows.Forms.Control.Dock%2A> プロパティの値を <xref:System.Windows.Forms.DockStyle.Top> に設定します。

8. [`elementHost3`] を選択します。

9. <xref:System.Windows.Forms.Control.Dock%2A> プロパティの値を <xref:System.Windows.Forms.DockStyle.Fill> に設定します。

   `elementHost3` コントロールが、フォームの残りの領域を満たすようサイズ変更されます。

10. フォームのサイズを変更します。

    3 つすべての <xref:System.Windows.Forms.Integration.ElementHost> コントロールのサイズを適切に変更します。

    詳細については、「[方法: TableLayoutPanel コントロールで子コントロールを固定およびドッキング](../controls/how-to-anchor-and-dock-child-controls-in-a-tablelayoutpanel-control.md)する」を参照してください。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [方法: TableLayoutPanel コントロールで子コントロールを固定およびドッキングする](../controls/how-to-anchor-and-dock-child-controls-in-a-tablelayoutpanel-control.md)
- [方法: デザイン時にフォームの端に合わせてコントロールを配置する](../controls/how-to-align-a-control-to-the-edges-of-forms-at-design-time.md)
- [チュートリアル: スナップ線を使用した Windows フォーム上のコントロールの配置](../controls/walkthrough-arranging-controls-on-windows-forms-using-snaplines.md)
- [移行と相互運用性](../../wpf/advanced/migration-and-interoperability.md)
- [WPF コントロールの使用](using-wpf-controls.md)
- [Visual Studio で XAML をデザインする](/visualstudio/xaml-tools/designing-xaml-in-visual-studio)
