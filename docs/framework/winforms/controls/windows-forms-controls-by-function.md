---
title: 関数によるコントロール
ms.date: 03/30/2017
helpviewer_keywords:
- controls [Windows Forms], by function
- Windows Forms, controls by function
- Windows Forms controls, list of
ms.assetid: 5e65a6c3-5d6f-480d-beb8-b28f865f07e3
ms.openlocfilehash: f2341d49513b418c244ee7ab93c0858899502487
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741575"
---
# <a name="windows-forms-controls-by-function"></a>Windows フォーム コントロールの機能別一覧
Windows フォームには、多数の関数を実行するコントロールとコンポーネントが用意されています。 次の表に、一般的な関数に従った Windows フォームコントロールとコンポーネントの一覧を示します。 また、同じ機能を提供する複数のコントロールが存在する場合、推奨されるコントロールは、置き換えられたコントロールに関するメモと共に一覧表示されます。 別の後続の表では、置き換えられるコントロールが、推奨される置換と共に一覧表示されます。  
  
> [!NOTE]
> 次の表は、Windows フォームで使用できるすべてのコントロールまたはコンポーネントの一覧を示していません。より包括的な一覧については、「 [Windows フォームで使用するコントロール](controls-to-use-on-windows-forms.md)」を参照してください。  
  
## <a name="recommended-controls-and-components-by-function"></a>関数別の推奨されるコントロールとコンポーネント  
  
|Function|コントロール|[説明]|  
|--------------|-------------|-----------------|  
|データ表示|<xref:System.Windows.Forms.DataGridView> コントロール|<xref:System.Windows.Forms.DataGridView> コントロールは、データを表示するためのカスタマイズ可能なテーブルを提供します。 <xref:System.Windows.Forms.DataGridView> クラスを使用すると、セル、行、列、および罫線をカスタマイズできます。 **注:** <xref:System.Windows.Forms.DataGridView> コントロールには、<xref:System.Windows.Forms.DataGrid> コントロールに不足している基本的な機能と高度な機能が多数用意されています。 詳細については、「 [Windows フォーム DataGridView コントロールと DataGrid コントロールの違い](differences-between-the-windows-forms-datagridview-and-datagrid-controls.md)」を参照してください。|  
|データバインディングとナビゲーション|<xref:System.Windows.Forms.BindingSource> コンポーネント|通貨管理、変更通知、およびその他のサービスを提供することにより、フォーム上のコントロールを簡単にデータにバインドできます。|  
||<xref:System.Windows.Forms.BindingNavigator> コントロール|フォーム上のデータを移動および操作するためのツールバー型インターフェイスを提供します。|  
|テキスト編集|<xref:System.Windows.Forms.TextBox> コントロール|実行時にユーザーが編集できる、またはプログラムによって変更される、デザイン時に入力されたテキストを表示します。|  
||<xref:System.Windows.Forms.RichTextBox> コントロール|プレーンテキストまたはリッチテキスト形式 (RTF) での書式設定を使用してテキストを表示できるようにします。|  
||<xref:System.Windows.Forms.MaskedTextBox> コントロール|ユーザー入力の形式を制限します|  
|情報の表示 (読み取り専用)|<xref:System.Windows.Forms.Label> コントロール|ユーザーが直接編集できないテキストを表示します。|  
||<xref:System.Windows.Forms.LinkLabel> コントロール|テキストを Web スタイルのリンクとして表示し、ユーザーが特別なテキストをクリックしたときにイベントをトリガーします。 通常、テキストは別のウィンドウまたは Web サイトへのリンクです。|  
||<xref:System.Windows.Forms.StatusStrip> コントロール|通常は親フォームの下部にある、フレーム領域を使用して、アプリケーションの現在の状態に関する情報を表示します。|  
||<xref:System.Windows.Forms.ProgressBar> コントロール|操作の現在の進行状況をユーザーに表示します。|  
|Web ページの表示|<xref:System.Windows.Forms.WebBrowser> コントロール|ユーザーがフォーム内で Web ページをナビゲートできるようにします。|  
|リストからの選択|<xref:System.Windows.Forms.CheckedListBox> コントロール|項目のスクロール可能な一覧を表示します。各項目にはチェックボックスが付属しています。|  
||<xref:System.Windows.Forms.ComboBox> コントロール|項目のドロップダウンリストを表示します。|  
||<xref:System.Windows.Forms.DomainUpDown> コントロール|ユーザーがスクロールボタンを使用してスクロールできるテキスト項目の一覧を表示します。|  
||<xref:System.Windows.Forms.ListBox> コントロール|テキストとグラフィカルな項目 (アイコン) の一覧を表示します。|  
||<xref:System.Windows.Forms.ListView> コントロール|4つの異なるビューのいずれかに項目を表示します。 表示には、テキストのみ、小さいアイコンのテキスト、大きいアイコンを含むテキスト、および詳細ビューが含まれます。|  
||<xref:System.Windows.Forms.NumericUpDown> コントロール|ユーザーが上下にスクロールできる数字の一覧を表示します。|  
||<xref:System.Windows.Forms.TreeView> コントロール|オプションのチェックボックスまたはアイコンを含むテキストで構成できるノードオブジェクトの階層コレクションを表示します。|  
|グラフィックスの表示|<xref:System.Windows.Forms.PictureBox> コントロール|ビットマップやアイコンなどのグラフィックファイルをフレームに表示します。|  
|グラフィックスストレージ|<xref:System.Windows.Forms.ImageList> コントロール|イメージのリポジトリとして機能します。 <xref:System.Windows.Forms.ImageList> コントロールとそれに含まれるイメージは、1つのアプリケーションから次のアプリケーションに再利用できます。|  
|値の設定|<xref:System.Windows.Forms.CheckBox> コントロール|テキストのチェックボックスとラベルを表示します。 一般に、オプションの設定に使用されます。|  
||<xref:System.Windows.Forms.CheckedListBox> コントロール|項目のスクロール可能な一覧を表示します。各項目にはチェックボックスが付属しています。|  
||<xref:System.Windows.Forms.RadioButton> コントロール|オンまたはオフにできるボタンを表示します。|  
||<xref:System.Windows.Forms.TrackBar> コントロール|スケールに沿って "thumb" を移動して、ユーザーがスケールの値を設定できるようにします。|  
|日付の設定|<xref:System.Windows.Forms.DateTimePicker> コントロール|ユーザーが日付または時刻を選択できるようにするためのグラフィカルなカレンダーを表示します。|  
||<xref:System.Windows.Forms.MonthCalendar> コントロール|ユーザーが日付の範囲を選択できるようにするためのグラフィカルなカレンダーを表示します。|  
|ダイアログ ボックス|<xref:System.Windows.Forms.ColorDialog> コントロール|ユーザーがインターフェイス要素の色を設定できる [カラーピッカー] ダイアログボックスを表示します。|  
||<xref:System.Windows.Forms.FontDialog> コントロール|ユーザーがフォントとその属性を設定できるダイアログボックスを表示します。|  
||<xref:System.Windows.Forms.OpenFileDialog> コントロール|ユーザーがファイルに移動して選択できるダイアログボックスを表示します。|  
||<xref:System.Windows.Forms.PrintDialog> コントロール|ユーザーがプリンターを選択し、その属性を設定できるダイアログボックスを表示します。|  
||<xref:System.Windows.Forms.PrintPreviewDialog> コントロール|印刷時にコントロール <xref:System.Drawing.Printing.PrintDocument> コンポーネントがどのように表示されるかを表示するダイアログボックスを表示します。|  
||<xref:System.Windows.Forms.FolderBrowserDialog> コントロール|ユーザーがフォルダーを参照して作成し、最終的に選択できるダイアログを表示します。|  
||<xref:System.Windows.Forms.SaveFileDialog> コントロール|ユーザーがファイルを保存するためのダイアログボックスを表示します。|  
|メニューコントロール|<xref:System.Windows.Forms.MenuStrip> コントロール|カスタムメニューを作成します。 **注:** <xref:System.Windows.Forms.MenuStrip> は、<xref:System.Windows.Forms.MainMenu> コントロールを置き換えるように設計されています。|  
||<xref:System.Windows.Forms.ContextMenuStrip> コントロール|カスタムコンテキストメニューを作成します。 **注:** <xref:System.Windows.Forms.ContextMenuStrip> は、<xref:System.Windows.Forms.ContextMenu> コントロールを置き換えるように設計されています。|  
|コマンド|<xref:System.Windows.Forms.Button> コントロール|プロセスを開始、停止、または中断します。|  
||<xref:System.Windows.Forms.LinkLabel> コントロール|テキストを Web スタイルのリンクとして表示し、ユーザーが特別なテキストをクリックしたときにイベントをトリガーします。 通常、テキストは別のウィンドウまたは Web サイトへのリンクです。|  
||<xref:System.Windows.Forms.NotifyIcon> コントロール|バックグラウンドで実行されているアプリケーションを表すタスクバーの状態通知領域にアイコンを表示します。|  
||<xref:System.Windows.Forms.ToolStrip> コントロール|テーマの有無にかかわらず、Microsoft Windows XP、Microsoft Office、Microsoft Internet Explorer、またはカスタムのルックアンドフィールを備えたツールバーを作成します。また、テーマを使用するかどうか、およびオーバーフローと実行時の項目の並べ替えをサポートします。 **注:** <xref:System.Windows.Forms.ToolStrip> コントロールは、<xref:System.Windows.Forms.ToolBar> コントロールを置き換えるように設計されています。|  
|ユーザー ヘルプ|<xref:System.Windows.Forms.HelpProvider> コンポーネント|コントロールのポップアップ ヘルプまたはオンライン ヘルプを提供します。|  
||<xref:System.Windows.Forms.ToolTip> コンポーネント|ユーザーがポインターをコントロールの上に置いたときに、コントロールの目的の簡単な説明を表示するポップアップウィンドウを提供します。|  
|その他のコントロールのグループ化|<xref:System.Windows.Forms.Panel> コントロール|ラベル付けされていないスクロール可能なフレーム上のコントロールのセットをグループ化します。|  
||<xref:System.Windows.Forms.GroupBox> コントロール|ラベル付けされた nonscrollable frame で、一連のコントロール (ラジオボタンなど) をグループ化します。|  
||<xref:System.Windows.Forms.TabControl> コントロール|グループ化されたオブジェクトを効率的に整理およびアクセスするためのタブ付きページを提供します。|  
||<xref:System.Windows.Forms.SplitContainer> コントロール|移動可能なバーで区切られた2つのパネルを提供します。 **注:** <xref:System.Windows.Forms.SplitContainer> コントロールは、<xref:System.Windows.Forms.Splitter> コントロールを置き換えるように設計されています。|  
||<xref:System.Windows.Forms.TableLayoutPanel> コントロール|内容を行と列から成るグリッドに動的にレイアウトするパネルを表します。|  
||<xref:System.Windows.Forms.FlowLayoutPanel> コントロール|水平方向または垂直方向に内容を動的にレイアウトするパネルを表します。|  
|オーディオ|<xref:System.Media.SoundPlayer> コントロール|.Wav 形式でサウンドファイルを再生します。 サウンドは、非同期に読み込んだり、再生したりすることができます。|  
  
## <a name="superseded-controls-and-components-by-function"></a>置き換えられたコントロールとコンポーネント (関数別)  
  
|Function|置き換えられるコントロール|推奨代替|  
|--------------|------------------------|-----------------------------|  
|データ表示|<xref:System.Windows.Forms.DataGrid>|<xref:System.Windows.Forms.DataGridView>|  
|情報の表示 (読み取り専用コントロール)|<xref:System.Windows.Forms.StatusBar>|<xref:System.Windows.Forms.StatusStrip>|  
|メニューコントロール|<xref:System.Windows.Forms.ContextMenu>|<xref:System.Windows.Forms.ContextMenuStrip>|  
||<xref:System.Windows.Forms.MainMenu>|<xref:System.Windows.Forms.MenuStrip>|  
|コマンド|<xref:System.Windows.Forms.ToolBar>|<xref:System.Windows.Forms.ToolStrip>|  
||<xref:System.Windows.Forms.StatusBar>|<xref:System.Windows.Forms.StatusStrip>|  
|フォームのレイアウト|<xref:System.Windows.Forms.Splitter>|<xref:System.Windows.Forms.SplitContainer>|  
  
## <a name="see-also"></a>参照

- [Windows フォームで使用するコントロール](controls-to-use-on-windows-forms.md)
- [.NET Framework を使用したカスタム Windows フォーム コントロールの開発](developing-custom-windows-forms-controls.md)
