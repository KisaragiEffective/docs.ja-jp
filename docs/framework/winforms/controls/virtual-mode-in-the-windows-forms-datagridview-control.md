---
title: DataGridView コントロールの仮想モード
ms.date: 03/30/2017
helpviewer_keywords:
- DataGridView control [Windows Forms], virtual mode
ms.assetid: feae5d43-2848-4b1a-8ea7-77085dc415b5
ms.openlocfilehash: 0d82f0fc9946e5b61ea171f2f5d2ab5690db0c71
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745436"
---
# <a name="virtual-mode-in-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロールでの仮想モード
仮想モードでは、<xref:System.Windows.Forms.DataGridView> コントロールとカスタムデータキャッシュとの間の相互作用を管理できます。 仮想モードを実装するには、<xref:System.Windows.Forms.DataGridView.VirtualMode%2A> プロパティを `true` に設定し、このトピックで説明する1つ以上のイベントを処理します。 通常は、少なくとも `CellValueNeeded` イベントを処理します。これにより、コントロールがデータキャッシュ内の値を検索できるようになります。  
  
## <a name="bound-mode-and-virtual-mode"></a>バインドモードと仮想モード  
 仮想モードは、バインドモードを補完または置き換える必要がある場合にのみ必要です。 バインドモードでは、<xref:System.Windows.Forms.DataGridView.DataSource%2A> プロパティを設定します。コントロールは、指定されたソースからデータを自動的に読み込み、ユーザーの変更を元に戻します。 どのバインド列を表示するかを制御できます。また、データソース自体は通常、並べ替えなどの操作を処理します。  
  
## <a name="supplementing-bound-mode"></a>バインドモードの補完  
 バインドされた列とバインドされた列を表示することにより、バインドモードを補うことができます。 これは、"混合モード" と呼ばれることもあります。これは、計算値やユーザーインターフェイス (UI) コントロールなどを表示する場合に便利です。  
  
 非バインド列はデータソースの外部にあるため、データソースの並べ替え操作では無視されます。 そのため、混在モードでの並べ替えを有効にする場合は、ローカルキャッシュ内のバインドされていないデータを管理し、仮想モードを実装して、<xref:System.Windows.Forms.DataGridView> コントロールと対話できるようにする必要があります。  
  
 仮想モードを使用してバインドされていない列の値を維持する方法の詳細については、<xref:System.Windows.Forms.DataGridViewCheckBoxColumn.ThreeState%2A?displayProperty=nameWithType> プロパティと <xref:System.Windows.Forms.DataGridViewComboBoxColumn?displayProperty=nameWithType> クラスのリファレンスに関するトピックの例を参照してください。  
  
## <a name="replacing-bound-mode"></a>バインドモードの置換  
 バインドモードがパフォーマンスのニーズを満たしていない場合は、仮想モードのイベントハンドラーを使用して、カスタムキャッシュ内のすべてのデータを管理できます。 たとえば、仮想モードを使用すると、最適化されたパフォーマンスを得るために必要なだけのデータをネットワークデータベースから取得する、ジャストインタイムのデータ読み込みメカニズムを実装できます。 このシナリオは、低速のネットワーク接続で大量のデータを使用する場合や、RAM または記憶域スペースの容量が限られているクライアントコンピューターを使用する場合に特に便利です。  
  
 ジャストインタイムシナリオでの仮想モードの使用の詳細については、「 [Windows フォーム DataGridView コントロールでの Just-in-time データ読み込みによる仮想モードの実装](implementing-virtual-mode-jit-data-loading-in-the-datagrid.md)」を参照してください。  
  
## <a name="virtual-mode-events"></a>仮想モードイベント  
 データが読み取り専用の場合、処理する必要があるのは `CellValueNeeded` イベントだけである可能性があります。 追加の仮想モードイベントを使用すると、ユーザーの編集、行の追加と削除、行レベルのトランザクションなど、特定の機能を有効にすることができます。  
  
 一部の標準 <xref:System.Windows.Forms.DataGridView> イベント (ユーザーが行を追加または削除したとき、またはセル値の編集、解析、検証、または書式設定が行われたときに発生するイベントなど) は、仮想モードでも便利です。 また、セルのツールヒントテキスト、セルと行のエラーテキスト、セルと行のショートカットメニューデータ、行の高さデータなど、バインドされたデータソースに格納されていない値を維持するためのイベントを処理することもできます。  
  
 行レベルのコミットスコープで読み取り/書き込みデータを管理するための仮想モードの実装の詳細については、「[チュートリアル: Windows フォーム DataGridView コントロールでの仮想モードの実装](implementing-virtual-mode-wf-datagridview-control.md)」を参照してください。  
  
 セルレベルのコミットスコープで仮想モードを実装する例については、「<xref:System.Windows.Forms.DataGridView.VirtualMode%2A> プロパティのリファレンス」を参照してください。  
  
 次のイベントは、<xref:System.Windows.Forms.DataGridView.VirtualMode%2A> プロパティが `true`に設定されている場合にのみ発生します。  
  
|Event|[説明]|  
|-----------|-----------------|  
|<xref:System.Windows.Forms.DataGridView.CellValueNeeded>|データキャッシュからセル値を取得して表示するために、コントロールによって使用されます。 このイベントは、バインドされていない列のセルに対してのみ発生します。|  
|<xref:System.Windows.Forms.DataGridView.CellValuePushed>|セルのユーザー入力をデータキャッシュにコミットするために、コントロールによって使用されます。 このイベントは、バインドされていない列のセルに対してのみ発生します。<br /><br /> <xref:System.Windows.Forms.DataGridView.CellValuePushed> イベントハンドラーの外部でキャッシュされた値を変更する場合は、<xref:System.Windows.Forms.DataGridView.UpdateCellValue%2A> メソッドを呼び出して、現在の値がコントロールに表示されることを確認し、現在有効になっている自動サイズ変更モードを適用します。|  
|<xref:System.Windows.Forms.DataGridView.NewRowNeeded>|データキャッシュ内の新しい行が必要であることを示すために、コントロールによって使用されます。|  
|<xref:System.Windows.Forms.DataGridView.RowDirtyStateNeeded>|コミットされていない変更が行にあるかどうかを判断するために、コントロールによって使用されます。|  
|<xref:System.Windows.Forms.DataGridView.CancelRowEdit>|行をキャッシュされた値に戻す必要があることを示すために、コントロールによって使用されます。|  
  
 次のイベントは、仮想モードでは便利ですが、<xref:System.Windows.Forms.DataGridView.VirtualMode%2A> プロパティの設定に関係なく使用できます。  
  
|events|[説明]|  
|------------|-----------------|  
|<xref:System.Windows.Forms.DataGridView.UserDeletingRow><br /><br /> <xref:System.Windows.Forms.DataGridView.UserDeletedRow><br /><br /> <xref:System.Windows.Forms.DataGridView.RowsRemoved><br /><br /> <xref:System.Windows.Forms.DataGridView.RowsAdded>|コントロールによって使用され、行が削除または追加されたことを示します。これにより、データキャッシュを適宜更新できます。|  
|<xref:System.Windows.Forms.DataGridView.CellFormatting><br /><br /> <xref:System.Windows.Forms.DataGridView.CellParsing><br /><br /> <xref:System.Windows.Forms.DataGridView.CellValidating><br /><br /> <xref:System.Windows.Forms.DataGridView.CellValidated><br /><br /> <xref:System.Windows.Forms.DataGridView.RowValidating><br /><br /> <xref:System.Windows.Forms.DataGridView.RowValidated>|コントロールが、表示するセルの値を書式設定し、ユーザー入力を解析および検証するために使用します。|  
|<xref:System.Windows.Forms.DataGridView.CellToolTipTextNeeded>|<xref:System.Windows.Forms.DataGridView.DataSource%2A> プロパティが設定されている場合、または <xref:System.Windows.Forms.DataGridView.VirtualMode%2A> プロパティが `true`場合に、コントロールによってセルのツールヒントテキストを取得するために使用されます。<br /><br /> セルのツールヒントは、<xref:System.Windows.Forms.DataGridView.ShowCellToolTips%2A> プロパティの値が `true`場合にのみ表示されます。|  
|<xref:System.Windows.Forms.DataGridView.CellErrorTextNeeded><br /><br /> <xref:System.Windows.Forms.DataGridView.RowErrorTextNeeded>|<xref:System.Windows.Forms.DataGridView.DataSource%2A> プロパティが設定されている場合、または <xref:System.Windows.Forms.DataGridView.VirtualMode%2A> プロパティが `true`場合に、コントロールによってセルまたは行のエラーテキストを取得するために使用されます。<br /><br /> セルまたは行のエラーテキストを変更して、コントロールに現在の値が表示されるようにするには、<xref:System.Windows.Forms.DataGridView.UpdateCellErrorText%2A> メソッドまたは <xref:System.Windows.Forms.DataGridView.UpdateRowErrorText%2A> メソッドを呼び出します。<br /><br /> <xref:System.Windows.Forms.DataGridView.ShowCellErrors%2A> と <xref:System.Windows.Forms.DataGridView.ShowRowErrors%2A> のプロパティ値が `true`場合、セルおよび行のエラーグリフが表示されます。|  
|<xref:System.Windows.Forms.DataGridView.CellContextMenuStripNeeded><br /><br /> <xref:System.Windows.Forms.DataGridView.RowContextMenuStripNeeded>|コントロール <xref:System.Windows.Forms.DataGridView.DataSource%2A> プロパティが設定されている場合、または <xref:System.Windows.Forms.DataGridView.VirtualMode%2A> プロパティが `true`場合に、コントロールによってセルまたは <xref:System.Windows.Forms.ContextMenuStrip> 行を取得するために使用されます。|  
|<xref:System.Windows.Forms.DataGridView.RowHeightInfoNeeded><br /><br /> <xref:System.Windows.Forms.DataGridView.RowHeightInfoPushed>|データキャッシュ内の行の高さ情報を取得または格納するために、コントロールによって使用されます。 現在の値がコントロールの表示で使用されるように、<xref:System.Windows.Forms.DataGridView.RowHeightInfoPushed> イベントハンドラーの外部でキャッシュされた行の高さ情報を変更する場合は、<xref:System.Windows.Forms.DataGridView.UpdateRowHeightInfo%2A> メソッドを呼び出します。|  
  
## <a name="best-practices-in-virtual-mode"></a>仮想モードのベストプラクティス  
 大量のデータを効率的に処理するために仮想モードを実装する場合は、<xref:System.Windows.Forms.DataGridView> コントロール自体を使用して効率的に作業していることも確認する必要があります。 セルスタイル、自動サイズ変更、選択、および行共有の効率的な使用方法の詳細については、「 [Windows フォーム DataGridView コントロールのスケーリングに関するベストプラクティス](best-practices-for-scaling-the-windows-forms-datagridview-control.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.VirtualMode%2A>
- [Windows フォーム DataGridView コントロールでのパフォーマンス チューニング](performance-tuning-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールを拡張するための推奨される手順](best-practices-for-scaling-the-windows-forms-datagridview-control.md)
- [チュートリアル: Windows フォーム DataGridView コントロールでの仮想モードの実装](implementing-virtual-mode-wf-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでの Just-In-Time データ読み込みによる仮想モードの実装](implementing-virtual-mode-jit-data-loading-in-the-datagrid.md)
