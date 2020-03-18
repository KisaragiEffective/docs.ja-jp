---
title: '方法 : MenuStrip にオプション ボタンを表示する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- MenuStrip [Windows Forms], displaying option buttons
- displaying option buttons [Windows Forms], MenuStrip [Windows Forms]
- option buttons [Windows Forms], displaying in MenuStrip
ms.assetid: 8b596af2-9ff8-4f7b-93d7-cba830e167f4
ms.openlocfilehash: f3010e71396ce562e9dbdefd4b75b36f3a81ffb3
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76735525"
---
# <a name="how-to-display-option-buttons-in-a-menustrip-windows-forms"></a>方法 : MenuStrip にオプション ボタンを表示する (Windows フォーム)

オプションボタンは、オプションボタンとも呼ばれ、ユーザーが一度に1つだけ選択できる点を除いて、チェックボックスに似ています。 既定では <xref:System.Windows.Forms.ToolStripMenuItem> クラスはオプションボタンの動作を提供しませんが、クラスには、<xref:System.Windows.Forms.MenuStrip> コントロールのメニュー項目に対してオプションボタンの動作を実装するようにカスタマイズできるチェックボックスの動作が用意されています。

メニュー項目の [<xref:System.Windows.Forms.ToolStripMenuItem.CheckOnClick%2A>] プロパティが `true`場合、ユーザーは項目をクリックしてチェックマークの表示を切り替えることができます。 <xref:System.Windows.Forms.ToolStripMenuItem.Checked%2A> プロパティは、項目の現在の状態を示します。 基本的なオプションボタンの動作を実装するには、項目が選択されているときに、前に選択した項目の <xref:System.Windows.Forms.ToolStripMenuItem.Checked%2A> プロパティを `false`に設定する必要があります。

次の手順では、<xref:System.Windows.Forms.ToolStripMenuItem> クラスを継承するクラスにこの機能と追加機能を実装する方法について説明します。 `ToolStripRadioButtonMenuItem` クラスは、<xref:System.Windows.Forms.ToolStripMenuItem.OnCheckedChanged%2A> や <xref:System.Windows.Forms.ToolStripMenuItem.OnPaint%2A> などのメンバーをオーバーライドして、オプションボタンの選択動作と外観を提供します。 また、このクラスは、親項目が選択されていない場合にサブメニューのオプションを無効にするために、<xref:System.Windows.Forms.ToolStripMenuItem.Enabled%2A> プロパティをオーバーライドします。

## <a name="to-implement-option-button-selection-behavior"></a>オプションボタンの選択動作を実装するには

1. 項目の選択を有効にするには、<xref:System.Windows.Forms.ToolStripMenuItem.CheckOnClick%2A> プロパティを `true` に初期化します。

     [!code-csharp[ToolStripRadioButtonMenuItem#110](~/samples/snippets/csharp/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/cs/ToolStripRadioButtonMenuItem.cs#110)]
     [!code-vb[ToolStripRadioButtonMenuItem#110](~/samples/snippets/visualbasic/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/vb/ToolStripRadioButtonMenuItem.vb#110)]

2. <xref:System.Windows.Forms.ToolStripMenuItem.OnCheckedChanged%2A> メソッドをオーバーライドして、新しい項目を選択したときに選択した項目の選択を解除します。

     [!code-csharp[ToolStripRadioButtonMenuItem#120](~/samples/snippets/csharp/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/cs/ToolStripRadioButtonMenuItem.cs#120)]
     [!code-vb[ToolStripRadioButtonMenuItem#120](~/samples/snippets/visualbasic/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/vb/ToolStripRadioButtonMenuItem.vb#120)]

3. <xref:System.Windows.Forms.ToolStripMenuItem.OnClick%2A> メソッドをオーバーライドして、既に選択されている項目をクリックしても選択内容がクリアされないようにします。

     [!code-csharp[ToolStripRadioButtonMenuItem#130](~/samples/snippets/csharp/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/cs/ToolStripRadioButtonMenuItem.cs#130)]
     [!code-vb[ToolStripRadioButtonMenuItem#130](~/samples/snippets/visualbasic/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/vb/ToolStripRadioButtonMenuItem.vb#130)]

## <a name="to-modify-the-appearance-of-the-option-button-items"></a>オプションボタンの項目の外観を変更するには

1. <xref:System.Windows.Forms.RadioButtonRenderer> クラスを使用して、既定のチェックマークをオプションボタンに置き換えるには、<xref:System.Windows.Forms.ToolStripMenuItem.OnPaint%2A> メソッドをオーバーライドします。

     [!code-csharp[ToolStripRadioButtonMenuItem#140](~/samples/snippets/csharp/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/cs/ToolStripRadioButtonMenuItem.cs#140)]
     [!code-vb[ToolStripRadioButtonMenuItem#140](~/samples/snippets/visualbasic/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/vb/ToolStripRadioButtonMenuItem.vb#140)]

2. <xref:System.Windows.Forms.ToolStripMenuItem.OnMouseEnter%2A>、<xref:System.Windows.Forms.ToolStripMenuItem.OnMouseLeave%2A>、<xref:System.Windows.Forms.ToolStripMenuItem.OnMouseDown%2A>、および <xref:System.Windows.Forms.ToolStripMenuItem.OnMouseUp%2A> の各メソッドをオーバーライドしてマウスの状態を追跡し、<xref:System.Windows.Forms.ToolStripMenuItem.OnPaint%2A> メソッドが正しいオプションボタンの状態を描画することを確認します。

     [!code-csharp[ToolStripRadioButtonMenuItem#150](~/samples/snippets/csharp/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/cs/ToolStripRadioButtonMenuItem.cs#150)]
     [!code-vb[ToolStripRadioButtonMenuItem#150](~/samples/snippets/visualbasic/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/vb/ToolStripRadioButtonMenuItem.vb#150)]

## <a name="to-disable-options-on-a-submenu-when-the-parent-item-is-not-selected"></a>親項目が選択されていないときにサブメニューのオプションを無効にするには

1. <xref:System.Windows.Forms.ToolStripMenuItem.CheckOnClick%2A> 値が `true` で、<xref:System.Windows.Forms.ToolStripMenuItem.Checked%2A> 値が `false`の親項目がある場合に、項目が無効になるように <xref:System.Windows.Forms.ToolStripMenuItem.Enabled%2A> プロパティをオーバーライドします。

     [!code-csharp[ToolStripRadioButtonMenuItem#160](~/samples/snippets/csharp/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/cs/ToolStripRadioButtonMenuItem.cs#160)]
     [!code-vb[ToolStripRadioButtonMenuItem#160](~/samples/snippets/visualbasic/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/vb/ToolStripRadioButtonMenuItem.vb#160)]

2. <xref:System.Windows.Forms.ToolStripMenuItem.OnOwnerChanged%2A> メソッドをオーバーライドして、親アイテムの <xref:System.Windows.Forms.ToolStripMenuItem.CheckedChanged> イベントをサブスクライブします。

     [!code-csharp[ToolStripRadioButtonMenuItem#170](~/samples/snippets/csharp/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/cs/ToolStripRadioButtonMenuItem.cs#170)]
     [!code-vb[ToolStripRadioButtonMenuItem#170](~/samples/snippets/visualbasic/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/vb/ToolStripRadioButtonMenuItem.vb#170)]

3. 親項目の <xref:System.Windows.Forms.ToolStripMenuItem.CheckedChanged> イベントのハンドラーで、項目を無効にして、新しい有効な状態で表示を更新します。

     [!code-csharp[ToolStripRadioButtonMenuItem#180](~/samples/snippets/csharp/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/cs/ToolStripRadioButtonMenuItem.cs#180)]
     [!code-vb[ToolStripRadioButtonMenuItem#180](~/samples/snippets/visualbasic/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/vb/ToolStripRadioButtonMenuItem.vb#180)]

## <a name="example"></a>例

次のコード例は、完全な `ToolStripRadioButtonMenuItem` クラスを提供し、<xref:System.Windows.Forms.Form> クラスと `Program` クラスを使用して、オプションボタンの動作を示します。

[!code-csharp[ToolStripRadioButtonMenuItem#000](~/samples/snippets/csharp/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/cs/ToolStripRadioButtonMenuItem.cs#000)]
[!code-vb[ToolStripRadioButtonMenuItem#000](~/samples/snippets/visualbasic/VS_Snippets_Winforms/ToolStripRadioButtonMenuItem/vb/ToolStripRadioButtonMenuItem.vb#000)]

## <a name="compiling-the-code"></a>コードのコンパイル

この例で必要な要素は次のとおりです。

- System、System.Drawing、および System.Windows.Forms の各アセンブリへの参照。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.MenuStrip>
- <xref:System.Windows.Forms.ToolStripMenuItem>
- <xref:System.Windows.Forms.ToolStripMenuItem.CheckOnClick%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.ToolStripMenuItem.Checked%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.ToolStripMenuItem.OnCheckedChanged%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.ToolStripMenuItem.OnPaint%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.ToolStripMenuItem.Enabled%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.RadioButtonRenderer>
- [MenuStrip コントロール](menustrip-control-windows-forms.md)
- [方法: カスタムの ToolStripRenderer を実装する](how-to-implement-a-custom-toolstriprenderer.md)
