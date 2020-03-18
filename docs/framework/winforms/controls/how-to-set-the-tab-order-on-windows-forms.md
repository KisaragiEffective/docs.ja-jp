---
title: コントロールのタブオーダーを設定する
ms.date: 03/30/2017
f1_keywords:
- TabStop
- TabIndex
helpviewer_keywords:
- tab order [Windows Forms], controls on Windows forms
- Windows Forms controls, setting tab order
- controls [Windows Forms], setting tab order
- Windows Forms, setting tab order
ms.assetid: 71fa8e76-0472-414b-ad3c-0f90166e0ad7
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 5d53e411bda0279271e4f73e1842c52fd6d9b3a9
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746836"
---
# <a name="how-to-set-the-tab-order-on-windows-forms"></a>方法: Windows フォームのタブオーダーを設定する

タブオーダーは、Tab キーを押すことによって、ユーザーがあるコントロールから別のコントロールにフォーカスを移動する順序です。 各フォームには、独自のタブオーダーがあります。 既定では、タブオーダーは、コントロールを作成した順序と同じです。 タブオーダー番号は0から始まります。

## <a name="to-set-the-tab-order-of-a-control"></a>コントロールのタブオーダーを設定するには

1. Visual Studio の **[表示]** メニューで、 **[タブオーダー]** を選択します。

   これにより、フォームのタブオーダー選択モードがアクティブになります。 各コントロールの左上隅に、(<xref:System.Windows.Forms.Control.TabIndex%2A> のプロパティを表す) 数値が表示されます。

2. コントロールを順番にクリックして、目的のタブオーダーを設定します。

   > [!NOTE]
   > タブオーダー内のコントロールの位置は、0以上の任意の値に設定できます。 重複が発生すると、2つのコントロールの z オーダーが評価され、上のコントロールが最初にタブになります。 (Z オーダーは、フォームの z 軸の [奥行] に沿ったフォーム上のコントロールの視覚的なレイヤーです。 Z オーダーは、他のコントロールの前にあるコントロールを決定します。)Z オーダーの詳細については、「 [Windows フォームのオブジェクトのレイヤー](how-to-layer-objects-on-windows-forms.md)化」を参照してください。

3. 終了したら、 **[表示]** メニューの **[タブオーダー]** をもう一度選択して、タブオーダーモードを終了します。

   > [!NOTE]
   > フォーカスを取得できないコントロール、および無効になったコントロールと非表示のコントロールには、<xref:System.Windows.Forms.Control.TabIndex%2A> プロパティはなく、タブオーダーには含まれません。 ユーザーが Tab キーを押すと、これらのコントロールはスキップされます。

または、[<xref:System.Windows.Forms.Control.TabIndex%2A>] プロパティを使用して、プロパティウィンドウにタブオーダーを設定することもできます。 コントロールの <xref:System.Windows.Forms.Control.TabIndex%2A> プロパティは、タブオーダー内での位置を決定します。 既定では、描画された最初のコントロールの <xref:System.Windows.Forms.Control.TabIndex%2A> 値は0、2番目のコントロールの <xref:System.Windows.Forms.Control.TabIndex%2A> は1のようになります。

また、既定では、<xref:System.Windows.Forms.GroupBox> コントロールには、整数である独自の <xref:System.Windows.Forms.Control.TabIndex%2A> 値があります。 <xref:System.Windows.Forms.GroupBox> コントロール自体には、実行時にフォーカスを設定することはできません。 したがって、<xref:System.Windows.Forms.GroupBox> 内の各コントロールには、0から始まる独自の decimal <xref:System.Windows.Forms.Control.TabIndex%2A> 値があります。 当然ながら、<xref:System.Windows.Forms.GroupBox> コントロールの <xref:System.Windows.Forms.Control.TabIndex%2A> がインクリメントされると、そのコントロール内のコントロールがそれに応じてインクリメントされます。 <xref:System.Windows.Forms.Control.TabIndex%2A> 値を5から6に変更した場合、そのグループの最初のコントロールの <xref:System.Windows.Forms.Control.TabIndex%2A> 値は自動的に6.0 に変更されます。

最後に、フォーム上の多くのコントロールは、タブオーダーではスキップできます。 通常、実行時に Tab キーを連続して押すと、各コントロールがタブオーダーで選択されます。 <xref:System.Windows.Forms.Control.TabStop%2A> プロパティをオフにすると、フォームのタブオーダーでコントロールを渡すことができます。

## <a name="to-remove-a-control-from-the-tab-order"></a>タブオーダーからコントロールを削除するには

**[プロパティ]** ウィンドウで、コントロールの <xref:System.Windows.Forms.Control.TabStop%2A> プロパティを**false**に設定します。

Tab キーを使用してコントロールを巡回するときにコントロールがスキップされた場合でも、<xref:System.Windows.Forms.Control.TabStop%2A> プロパティが `false` に設定されているコントロールは、タブオーダー内でその位置を維持します。

> [!NOTE]
> ラジオボタングループには、実行時に1つのタブストップがあります。 選択したボタン ([<xref:System.Windows.Forms.RadioButton.Checked%2A>] プロパティが [`true`] に設定されているボタン) では、[<xref:System.Windows.Forms.Control.TabStop%2A>] プロパティが [`true`] に自動的に設定されます。一方、他のボタンでは <xref:System.Windows.Forms.Control.TabStop%2A> プロパティが `false`に設定されています。 <xref:System.Windows.Forms.RadioButton> コントロールのグループ化の詳細については、「 [Set Windows フォーム RadioButton コントロールをセットとして機能させる](how-to-group-windows-forms-radiobutton-controls-to-function-as-a-set.md)」を参照してください。

## <a name="see-also"></a>参照

- [Windows フォーム コントロール](index.md)
- [Windows フォームで使用するコントロール](controls-to-use-on-windows-forms.md)
- [Windows フォーム コントロールの機能別一覧](windows-forms-controls-by-function.md)
