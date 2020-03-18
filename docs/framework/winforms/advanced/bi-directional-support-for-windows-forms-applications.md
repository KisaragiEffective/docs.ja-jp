---
title: 双方向サポート
ms.date: 09/30/2017
helpviewer_keywords:
- globalization [Windows Forms], bi-directional support in Windows
- Windows Forms, international
- localization [Windows Forms], bi-directional support in Windows
- bi-directional language support [Windows Forms], Windows applications
- Windows Forms, bi-directional support
ms.openlocfilehash: 8b2e842fc08be78b74cede85927352fafca7bc8f
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742079"
---
# <a name="bi-directional-support-for-windows-forms-applications"></a>Windows フォーム アプリケーションの双方向サポート
Visual Studio を使用して、アラビア語やヘブライ語などの双方向 (右から左) の言語をサポートする Windows ベースのアプリケーションを作成できます。 これには、標準的なフォーム、ダイアログ ボックス、MDI フォームや、これらのフォームで操作できるすべてのコントロール、まり、<xref:System.Windows.Forms.Control> 名前空間のすべてのオブジェクトが含まれます。

## <a name="culture-support"></a>カルチャのサポート
 カルチャと UI カルチャの設定は、アプリケーションが日付、時刻、通貨、およびその他の情報をどのように操作するかを決定します。 双方向言語のカルチャおよび UI カルチャのサポートは、その他の言語と同じです。 詳細については、「[グローバル Windows フォームおよび web フォームのカルチャ固有のクラス](/visualstudio/ide/globalizing-and-localizing-applications)」を参照してください。

## <a name="righttoleft-and-righttoleftlayout-properties"></a>RightToLeft プロパティと RightToLeftLayout プロパティ
 フォームの派生元となる基底 <xref:System.Windows.Forms.Control> クラスには、フォームとコントロールの読み取り順序を変更するよう設定できる <xref:System.Windows.Forms.Control.RightToLeft%2A> プロパティが含まれます。 フォームの <xref:System.Windows.Forms.Control.RightToLeft%2A> プロパティを設定する場合、既定ではフォームのコントロールがこの設定を継承します。 ただし、ほとんどのコントロールで <xref:System.Windows.Forms.Control.RightToLeft%2A> プロパティを個別に設定することもできます。 「[方法 : グローバリゼーション用に Windows フォームで右から左の方向でテキストを表示する](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/7d3337xw(v=vs.100))」も参照してください。

 <xref:System.Windows.Forms.Control.RightToLeft%2A> プロパティの影響は、コントロールごとに異なります。 一部のコントロールで、<xref:System.Windows.Forms.Button>、<xref:System.Windows.Forms.TreeView>、および <xref:System.Windows.Forms.ToolTip> の各コントロールの場合のように、プロパティでは読み取り順序のみを設定します。 その他のコントロールでは、<xref:System.Windows.Forms.Control.RightToLeft%2A> プロパティが読み取り順序とレイアウトの両方を変更します。 これには、<xref:System.Windows.Forms.RadioButton>、<xref:System.Windows.Forms.ComboBox>、および <xref:System.Windows.Forms.CheckBox> の各コントロールが含まれます。 その他のコントロールには、右から左のレイアウトのミラーに適用される <xref:System.Windows.Forms.Form.RightToLeftLayout%2A> プロパティが必要です。 次の表は、<xref:System.Windows.Forms.Control.RightToLeft%2A> プロパティと <xref:System.Windows.Forms.Form.RightToLeftLayout%2A> プロパティが個々 の Windows フォーム コントロールに影響する方法の詳細を示します。

|コントロールとコンポーネント|RightToLeft プロパティの効果|RightToLeftLayout プロパティの効果|ミラー化が必要か|
|------------------------|------------------------------------|------------------------------------------|-------------------------|
|<xref:System.Windows.Forms.Button>|RTL の読み取り順序を設定します。 <xref:System.Windows.Forms.ButtonBase.TextAlign%2A>、<xref:System.Windows.Forms.ButtonBase.ImageAlign%2A>、および <xref:System.Windows.Forms.ButtonBase.TextImageRelation%2A> を反転する|効果なし|いいえ|
|<xref:System.Windows.Forms.CheckBox>|テキストの右側にチェック ボックスが表示されます。|効果なし|いいえ|
|<xref:System.Windows.Forms.CheckedListBox>|テキストの右側にすべてのチェック ボックスが表示されます。|効果なし|いいえ|
|<xref:System.Windows.Forms.ColorDialog>|影響を受けません。オペレーティング システムの言語によって異なります|効果なし|いいえ|
|<xref:System.Windows.Forms.ComboBox>|コンボ ボックス コントロール内のアイテムが右揃え|効果なし|いいえ|
|<xref:System.Windows.Forms.ContextMenu>|RTL の読み取り順序で右揃えで表示|効果なし|いいえ|
|<xref:System.Windows.Forms.DataGrid>|RTL の読み取り順序で右揃えで表示|効果なし|いいえ|
|<xref:System.Windows.Forms.DataGridView>|RTL の読み取り順序とコントロールのレイアウトの両方に影響を与える|効果なし|いいえ|
|<xref:System.Windows.Forms.DateTimePicker>|影響を受けません。オペレーティング システムの言語によって異なります|コントロールをミラーリングする|はい|
|<xref:System.Windows.Forms.DomainUpDown>|上矢印と下矢印ボタンを左揃え|効果なし|いいえ|
|<xref:System.Windows.Forms.ErrorProvider>|サポートされていません|効果なし|いいえ|
|<xref:System.Windows.Forms.FontDialog>|オペレーティング システムの言語によって異なります|効果なし|いいえ|
|<xref:System.Windows.Forms.Form>|RTL の読み取り順序を設定し、スクロール バーを反転させる|フォームをミラー化する|はい|
|<xref:System.Windows.Forms.GroupBox>|キャプションが右揃えで表示されます。 子コントロールは、このプロパティを継承できます。|コントロール内で <xref:System.Windows.Forms.TableLayoutPanel> を使用して、右から左へのミラーリングをサポートする|いいえ|
|<xref:System.Windows.Forms.HScrollBar>|右揃えのスクロール ボックス (つまみ) で始まる|効果なし|いいえ|
|<xref:System.Windows.Forms.ImageList>|必要なし|効果なし|いいえ|
|<xref:System.Windows.Forms.Label>|右揃えに表示されます。 <xref:System.Windows.Forms.Label.TextAlign%2A> および <xref:System.Windows.Forms.Label.ImageAlign%2A> を反転する|効果なし|いいえ|
|<xref:System.Windows.Forms.LinkLabel>|右揃えに表示されます。 <xref:System.Windows.Forms.Label.TextAlign%2A> および <xref:System.Windows.Forms.Label.ImageAlign%2A> を反転する|効果なし|いいえ|
|<xref:System.Windows.Forms.ListBox>|アイテムが右揃え|効果なし|いいえ|
|<xref:System.Windows.Forms.ListView>|読み取り順序を RTL に設定し、要素は左揃えを維持する|コントロールをミラーリングする|はい|
|<xref:System.Windows.Forms.MainMenu>|(デザイン時ではなく) 実行時に RTL の読み取り順序で右揃えの表示|効果なし|いいえ|
|<xref:System.Windows.Forms.MaskedTextBox>|テキストが右から左へ表示されます。|効果なし|いいえ|
|<xref:System.Windows.Forms.MonthCalendar>|影響を受けません。オペレーティング システムの言語によって異なります|コントロールをミラーリングする|はい|
|<xref:System.Windows.Forms.NotifyIcon>|サポートされていません|サポートされていません|いいえ|
|<xref:System.Windows.Forms.NumericUpDown>|上矢印と下矢印ボタンを左揃え|効果なし|いいえ|
|<xref:System.Windows.Forms.OpenFileDialog>|右から左に記述するオペレーティングシステムで、含まれるフォームの <xref:System.Windows.Forms.Control.RightToLeft> プロパティをに設定して、ダイアログをローカライズ <xref:System.Windows.Forms.RightToLeft.Yes?displayProperty=nameWithType> |効果なし|いいえ|
|<xref:System.Windows.Forms.PageSetupDialog>|影響を受けません。オペレーティング システムの言語によって異なります|効果なし|いいえ|
|<xref:System.Windows.Forms.Panel>|子コントロールは、このプロパティを継承できます。|コントロール内で <xref:System.Windows.Forms.TableLayoutPanel> を使用して、右から左をサポートする|はい|
|<xref:System.Windows.Forms.PictureBox>|サポートされていません|効果なし|いいえ|
|<xref:System.Windows.Forms.PrintDialog>|影響を受けません。オペレーティング システムの言語によって異なります|効果なし|いいえ|
|<xref:System.Drawing.Printing.PrintDocument>|垂直スクロール バーが左揃えになり、水平スクロール バーが左から開始|効果なし|いいえ|
|<xref:System.Windows.Forms.PrintPreviewDialog>|サポートされていません|サポートされていません|いいえ|
|<xref:System.Windows.Forms.ProgressBar>|このプロパティによる影響はなし|コントロールをミラーリングする|はい|
|<xref:System.Windows.Forms.RadioButton>|テキストの右側にラジオ ボタンが表示|効果なし|いいえ|
|<xref:System.Windows.Forms.RichTextBox>|テキストを含むコントロールの要素が RTL の読み取り順序で右から左に表示される|効果なし|いいえ|
|<xref:System.Windows.Forms.SaveFileDialog>|影響を受けません。オペレーティング システムの言語によって異なります|効果なし|いいえ|
|<xref:System.Windows.Forms.SplitContainer>|パネルのレイアウトが反転され、垂直スクロール バーは左側に表示され、水平スクロール バーは右から始まる|<xref:System.Windows.Forms.TableLayoutPanel> を使用して、子コントロールの順序をミラーリングする|いいえ|
|<xref:System.Windows.Forms.Splitter>|サポートされていません|効果なし|いいえ|
|<xref:System.Windows.Forms.StatusBar>|サポートされていません。代わりに <xref:System.Windows.Forms.StatusStrip> を使用|効果はありません。代わりに <xref:System.Windows.Forms.StatusStrip> を使用|いいえ|
|<xref:System.Windows.Forms.TabControl>|このプロパティによる影響はなし|コントロールをミラーリングする|はい|
|<xref:System.Windows.Forms.TextBox>|RTL の読み取り順序で右から左へテキストが表示されます。|効果なし|いいえ|
|<xref:System.Windows.Forms.Timer>|必要なし|必要なし|いいえ|
|<xref:System.Windows.Forms.ToolBar>|このプロパティの夜影響はなし。代わりに <xref:System.Windows.Forms.ToolStrip> を使用|効果はありません。代わりに <xref:System.Windows.Forms.ToolStrip> を使用|はい|
|<xref:System.Windows.Forms.ToolTip>|RTL の読み取り順序を設定|効果なし|いいえ|
|<xref:System.Windows.Forms.TrackBar>|スクロールやトラックは右から始まります。<xref:System.Windows.Forms.TrackBar.Orientation%2A> が垂直方向の場合、タイマー刻みは右から発生します。|効果なし|いいえ|
|<xref:System.Windows.Forms.TreeView>|RTL の読み取り順序のみを設定|コントロールをミラーリングする|はい|
|<xref:System.Windows.Forms.UserControl>|左側に垂直スクロール バーが表示され、水平スクロール バーは右側につまみがあります|直接サポートはありません。<xref:System.Windows.Forms.TableLayoutPanel> を使用します。|いいえ|
|<xref:System.Windows.Forms.VScrollBar>|右側のスクロール可能なコントロールの代わりに左側に表示されます。|効果なし|いいえ|

## <a name="encoding"></a>エンコード
 Windows フォームは Unicode をサポートするので、双方向のアプリケーションを作成するときに、任意の文字セットを含めることができます。 ただし、すべての Windows フォーム コントロールですべてのプラットフォームの Unicode をサポートするわけではありません。

## <a name="gdi"></a>GDI+
 GDI + を使用して、右から左への読み取り順序でテキストを描画できます。 <xref:System.Drawing.Graphics.DrawString%2A> メソッドは、テキストの描画に使用され、テキストの原点を反転させるために、`StringFormat` 列挙の <xref:System.Drawing.StringFormatFlags.DirectionRightToLeft> のメンバーを設定できる <xref:System.Drawing.StringFormatFlags> パラメーターをサポートします。

## <a name="common-dialog-boxes"></a>コモン ダイアログ ボックス
 Windows の制御下にある [ファイルを開く] ダイアログ ボックスなどのシステム ツールです。 これらは、オペレーティング システムから言語要素を継承します。 適切な言語設定のバージョンの Windows を使用している場合は、これらのダイアログ ボックスは双方向言語で正しく動作します。

 同様に、メッセージ ボックスはオペレーティング システムを通過して、双方向のテキストをサポートします。 メッセージ ボックスのボタンのキャプションは、現在の言語設定に基づいています。 既定では、メッセージ ボックスは右から左への読み取り順序を使用せず、メッセージ ボックスが表示される場合に、読み取り順序を変更するパラメーターを指定することができます。

## <a name="righttoleft-scrollbars-and-scrollablecontrol"></a>RightToLeft、Scrollbars、および ScrollableControl
 Windows フォームには現在制限があり、両方の <xref:System.Windows.Forms.ScrollableControl> が有効になり、<xref:System.Windows.Forms.Control.RightToLeft%2A> が <xref:System.Windows.Forms.ScrollableControl.AutoScroll%2A> に設定されている場合に、<xref:System.Windows.Forms.RightToLeft.Yes> から派生したすべてのクラスが適切に動作しされません。 たとえば、フォームに <xref:System.Windows.Forms.Panel> のようなコントロールや、<xref:System.Windows.Forms.Panel> (<xref:System.Windows.Forms.FlowLayoutPanel> または <xref:System.Windows.Forms.TableLayoutPanel> など) から派生したコンテナー クラスを配置するとします。 コンテナーの <xref:System.Windows.Forms.ScrollableControl.AutoScroll%2A> を <xref:System.Windows.Forms.RightToLeft.Yes> に設定し、コンテナー内の 1 つ以上のコントロールの <xref:System.Windows.Forms.Control.Anchor%2A> プロパティを <xref:System.Windows.Forms.AnchorStyles.Right> に設定する場合、スクロール バーが表示されません。 <xref:System.Windows.Forms.ScrollableControl> から派生するクラスは、<xref:System.Windows.Forms.ScrollableControl.AutoScroll%2A> が <xref:System.Windows.Forms.RightToLeft.No> に設定された場合のように動作します。

 現在、唯一の回避策は、別の <xref:System.Windows.Forms.ScrollableControl> 内側に <xref:System.Windows.Forms.ScrollableControl> をネストすることです。 たとえば、この状況で <xref:System.Windows.Forms.TableLayoutPanel> が動作することが必要な場合は、<xref:System.Windows.Forms.Panel> コントロールの内側に配置して、<xref:System.Windows.Forms.ScrollableControl.AutoScroll%2A> 上の <xref:System.Windows.Forms.Panel> を <xref:System.Windows.Forms.RightToLeft.Yes> に設定します。

## <a name="mirroring"></a>ミラーリング
 *ミラーリング*とは、右から左にフローするように UI 要素のレイアウトを反転させることを指します。 たとえば、ミラーリングされた Windows フォームでは、最小化、最大化、および閉じるボタンはタイトル バーの右端ではなく左端に表示されます。

 フォームまたはコントロールの <xref:System.Windows.Forms.Control.RightToLeft%2A> プロパティを `true` に設定すると、フォームの要素の読み取り順序が反転しますが、この設定は、右から左のレイアウトを反転しません。つまり、ミラーリングが発生しません。 たとえば、このプロパティを設定しても、フォームのタイトル バーにある **[最小化]** 、 **[最大化]** 、 **[閉じる]** の各ボタンがフォームの左側に移動することはありません。 同様に、<xref:System.Windows.Forms.TreeView> コントロールなどのいくつかコントロールは、アラビア語またはヘブライ語に適した表示に変更するには、ミラーリングが必要です。 <xref:System.Windows.Forms.Form.RightToLeftLayout%2A> プロパティを設定することで、これらのコントロールをミラーリングできます。

 次のコントロールのミラーリングされたバージョンを作成できます。

- <xref:System.Windows.Forms.ColumnHeader.ListView%2A>

- <xref:System.Windows.Forms.Panel>

- <xref:System.Windows.Forms.StatusBar>

- <xref:System.Windows.Forms.TabControl>

- <xref:System.Windows.Forms.TabPage>

- <xref:System.Windows.Forms.ToolBar>

- <xref:System.Windows.Forms.TreeView>

 一部のコントロールはシールされています。 そのため、そこから新しいコントロールを派生できません。 これには、<xref:System.Windows.Forms.ImageList> コントロールや <xref:System.Windows.Forms.ProgressBar> コントロールが含まれます。

## <a name="see-also"></a>参照

- [ASP.NET Web アプリケーションに対する双方向サポート](https://docs.microsoft.com/previous-versions/aspnet/6eedwbtt(v=vs.100))
