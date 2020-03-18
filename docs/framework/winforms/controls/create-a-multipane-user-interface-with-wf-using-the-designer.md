---
title: デザイナーを使用してマルチペインユーザーインターフェイスを作成する
ms.date: 03/30/2017
helpviewer_keywords:
- user interface [Windows Forms], multipane
- SplitContainer control [Windows Forms], using the designer
- multipane user interface
ms.assetid: c3f9294d-a26c-4198-9242-f237f55f7573
ms.openlocfilehash: 6b41d538f1cd1633cec51c89e27adf3c5b47f9a6
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76737277"
---
# <a name="how-to-create-a-multipane-user-interface-with-windows-forms-using-the-designer"></a>方法 : デザイナーを使用して Windows フォームでマルチペイン ユーザー インターフェイスを作成する
次の手順では、Microsoft Outlook で使用されているものと同様のマルチペインユーザーインターフェイスを作成します。これは、**フォルダー**リスト、**メッセージ**ペイン、および**プレビュー**ウィンドウで使用します。 この配置は、フォームを使用してドッキングコントロールを主することで実現されます。

 コントロールをドッキングすると、コントロールが固定されている親コンテナーの端が決まります。 したがって、[<xref:System.Windows.Forms.SplitContainer.Dock%2A>] プロパティを [<xref:System.Windows.Forms.DockStyle.Right>] に設定すると、コントロールの右端が親コントロールの右端にドッキングされます。 さらに、ドッキングしたコントロールの端は、コンテナーコントロールの端に合わせて変更されます。 <xref:System.Windows.Forms.SplitContainer.Dock%2A> プロパティの動作の詳細については、「[方法: Windows フォームにコントロールをドッキング](how-to-dock-controls-on-windows-forms.md)する」を参照してください。

 この手順では、アプリケーションが Microsoft Outlook を模倣するように機能を追加するのではなく、フォーム上の <xref:System.Windows.Forms.SplitContainer> とその他のコントロールを配置する方法に焦点を当てます。

 このユーザーインターフェイスを作成するには、すべてのコントロールを <xref:System.Windows.Forms.SplitContainer> コントロール内に配置します。このコントロールには、左側のパネルに <xref:System.Windows.Forms.TreeView> コントロールが含まれています。 <xref:System.Windows.Forms.SplitContainer> コントロールの右側のパネルには、<xref:System.Windows.Forms.RichTextBox> コントロールの上に <xref:System.Windows.Forms.ListView> コントロールを持つ2つ目の <xref:System.Windows.Forms.SplitContainer> コントロールが含まれています。 これらの <xref:System.Windows.Forms.SplitContainer> コントロールを使用すると、フォーム上の他のコントロールのサイズを個別に変更できます。 この手順の手法を調整して、独自のカスタムユーザーインターフェイスを作成することができます。

## <a name="to-create-an-outlook-style-user-interface-at-design-time"></a>デザイン時に Outlook スタイルのユーザーインターフェイスを作成するには

1. 新しい Windows アプリケーションプロジェクトを作成します (**ファイル** > **新しい** > **プロジェクト** >  **C# Visual**または**Visual Basic** > **クラシックデスクトップ** > **Windows フォームアプリケーション**)。

2. <xref:System.Windows.Forms.SplitContainer> コントロールを**ツールボックス**からフォームにドラッグします。 **[プロパティ]** ウィンドウで、 <xref:System.Windows.Forms.SplitContainer.Dock%2A> プロパティを <xref:System.Windows.Forms.DockStyle.Fill>に設定します。

3. **[ツールボックス]** から <xref:System.Windows.Forms.SplitContainer> コントロールの左側のパネルに <xref:System.Windows.Forms.TreeView> コントロールをドラッグします。 **[プロパティ]** ウィンドウで、下矢印がクリックされたときに表示される値エディターで左側のパネルをクリックして、[<xref:System.Windows.Forms.SplitContainer.Dock%2A>] プロパティを [<xref:System.Windows.Forms.DockStyle.Left>] に設定します。

4. **ツールボックス**から別の <xref:System.Windows.Forms.SplitContainer> コントロールをドラッグします。フォームに追加した <xref:System.Windows.Forms.SplitContainer> コントロールの右側のパネルに配置します。 **プロパティ** ウィンドウで、<xref:System.Windows.Forms.SplitContainer.Dock%2A> プロパティを <xref:System.Windows.Forms.DockStyle.Fill> に設定し、<xref:System.Windows.Forms.SplitContainer.Orientation%2A> プロパティを <xref:System.Windows.Forms.Orientation.Horizontal>に設定します。

5. <xref:System.Windows.Forms.ListView> コントロールを**ツールボックス**から、フォームに追加した2つ目の <xref:System.Windows.Forms.SplitContainer> コントロールの上のパネルにドラッグします。 <xref:System.Windows.Forms.SplitContainer.Dock%2A> プロパティの <xref:System.Windows.Forms.ListView> コントロールを <xref:System.Windows.Forms.DockStyle.Fill>に設定します。

6. **[ツールボックス]** から <xref:System.Windows.Forms.RichTextBox> コントロールを2番目の <xref:System.Windows.Forms.SplitContainer> コントロールの下のパネルにドラッグします。 <xref:System.Windows.Forms.SplitContainer.Dock%2A> プロパティの <xref:System.Windows.Forms.RichTextBox> コントロールを <xref:System.Windows.Forms.DockStyle.Fill>に設定します。

     この時点で、F5 キーを押してアプリケーションを実行すると、Microsoft Outlook のような3部構成のユーザーインターフェイスがフォームに表示されます。

    > [!NOTE]
    > <xref:System.Windows.Forms.SplitContainer> コントロール内のいずれかのスプリッターの上にマウスポインターを置くと、内部ディメンションのサイズを変更できます。

アプリケーション開発のこの時点で、洗練されたユーザーインターフェイスを作成しました。 次の手順では、アプリケーション自体のプログラミングに進みます。たとえば、<xref:System.Windows.Forms.TreeView> コントロールと <xref:System.Windows.Forms.ListView> コントロールを何らかの種類のデータソースに接続します。 コントロールをデータに接続する方法の詳細については、「[データバインディングと Windows フォーム](../data-binding-and-windows-forms.md)」を参照してください。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.SplitContainer>
- [SplitContainer コントロール](splitcontainer-control-windows-forms.md)
