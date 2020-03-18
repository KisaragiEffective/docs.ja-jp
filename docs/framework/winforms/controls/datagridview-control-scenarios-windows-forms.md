---
title: DataGridView コントロールのシナリオ
ms.date: 03/30/2017
helpviewer_keywords:
- data [Windows Forms], displaying in tabular format
- data grids [Windows Forms], about data grids
- DataGridView control [Windows Forms], scenarios
ms.assetid: 09a5fd05-3447-47ec-a4ec-6082a2b7f0dd
ms.openlocfilehash: 160d967c6445fb753cb6c73babfb02a734a07e28
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742461"
---
# <a name="datagridview-control-scenarios-windows-forms"></a>DataGridView コントロールのシナリオ (Windows フォーム)
<xref:System.Windows.Forms.DataGridView> コントロールを使用すると、さまざまなデータソースの表形式のデータを表示できます。 単純な用途では、手動で <xref:System.Windows.Forms.DataGridView> にデータを入力し、コントロールを介して直接データを操作できます。 ただし、通常は、外部データソースにデータを格納し、<xref:System.Windows.Forms.BindingSource> コンポーネントを通じてコントロールをバインドします。  
  
 このトピックでは、<xref:System.Windows.Forms.DataGridView> コントロールに関連する一般的なシナリオのいくつかについて説明します。  
  
## <a name="scenario-1-displaying-small-amounts-of-data"></a>シナリオ 1: 少量のデータを表示する  
 <xref:System.Windows.Forms.DataGridView> コントロールにデータを表示するために、外部データソースにデータを保存する必要はありません。 少量のデータを処理している場合は、コントロールを自分で設定し、コントロールを使用してデータを操作できます。 これは*非バインドモード*と呼ばれます。 詳細については、「方法: バインドされていない[Windows フォーム DataGridView コントロールを作成](how-to-create-an-unbound-windows-forms-datagridview-control.md)する」を参照してください。  
  
### <a name="scenario-key-points"></a>シナリオのキーポイント  
  
- 非バインドモードでは、コントロールを手動で設定します。  
  
- 非バインドモードは、読み取り専用の少量のデータに特に適しています。  
  
- 非バインドモードは、スプレッドシートに似た、または少ないに設定されたテーブルにも適しています。  
  
## <a name="scenario-2-viewing-and-updating-data-stored-in-an-external-data-source"></a>シナリオ 2: 外部データソースに格納されているデータの表示と更新  
 <xref:System.Windows.Forms.DataGridView> コントロールをユーザーインターフェイス (UI) として使用して、ユーザーがデータベーステーブルやビジネスオブジェクトのコレクションなどのデータソースに保存されているデータにアクセスできるようにすることができます。 詳細については、「[方法: データを Windows フォーム DataGridView コントロールにバインドする](how-to-bind-data-to-the-windows-forms-datagridview-control.md)」を参照してください。  
  
### <a name="scenario-key-points"></a>シナリオのキーポイント  
  
- バインドモードを使用すると、データソースに接続したり、データソースのプロパティまたはデータベースの列に基づいて自動的に列を生成したり、コントロールを自動的に設定したりできます。  
  
- バインドモードは、ユーザーがデータを操作する場合に適しています。 データは、表示用に書式設定できます。ユーザー指定のデータは、データソースによって予期される形式に解析できます。 データ入力のフォーマットエラーとデータベース制約エラーを検出すると、ユーザーに警告が表示され、間違ったセルを修正できるようになります。  
  
- 列の並べ替え、固定、並べ替えなどの追加機能を使用すると、ユーザーはワークフローにとって最も便利な方法でデータを表示できます。  
  
- クリップボードのサポートにより、ユーザーはアプリケーションから他のアプリケーションにデータをコピーできます。  
  
## <a name="scenario-3-advanced-data"></a>シナリオ 3: 高度なデータ  
 標準のデータバインディングモデルでは対応できない特別なニーズがある場合は、*仮想モード*を実装することで、コントロールとデータの間の相互作用を管理できます。 仮想モードの実装とは、情報が必要なときにコントロールがセルに関する情報を要求できるようにする1つ以上のイベントハンドラーを実装することを意味します。  
  
 たとえば、大量のデータを処理する場合は、最適な効率を得るために仮想モードを実装することができます。 仮想モードは、別のデータソースから取得した列と共に表示される非バインド列の値を維持する場合にも役立ちます。  
  
 仮想モードの詳細については、「[チュートリアル: Windows フォーム DataGridView コントロールでの仮想モードの実装](implementing-virtual-mode-wf-datagridview-control.md)」を参照してください。  
  
### <a name="scenario-key-points"></a>シナリオのキーポイント  
  
- 仮想モードは、パフォーマンスを微調整する必要がある場合に非常に大量のデータを表示するために適しています。  
  
## <a name="scenario-4-automatically-resizing-rows-and-columns"></a>シナリオ 4: 行と列のサイズを自動的に変更する  
 定期的に更新されるデータを表示すると、行と列のサイズを自動的に変更して、すべてのコンテンツが表示されるようにすることができます。 <xref:System.Windows.Forms.DataGridView> コントロールには、手動でサイズ変更を有効または無効にしたり、特定の時刻にプログラムによってサイズを変更したり、コンテンツが変更されるたびに自動的にサイズを変更したりできるようにするオプション 詳細については、「 [Windows フォーム DataGridView コントロールのサイズ変更オプション](sizing-options-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
### <a name="scenario-key-points"></a>シナリオのキーポイント  
  
- 手動でサイズ変更することで、ユーザーはセルの高さと幅を調整できます。  
  
- 自動サイズ変更を使用すると、セルのサイズを維持して、セルの内容がクリップされないようにすることができます。  
  
- プログラムによるサイズ変更では、連続する自動サイズ変更のパフォーマンスが低下しないように、特定の時間にセルのサイズを変更することができます。  
  
## <a name="scenario-5-simple-customization"></a>シナリオ 5: 簡単なカスタマイズ  
 <xref:System.Windows.Forms.DataGridView> コントロールには、基本的な外観と動作を変更するためのさまざまな方法が用意されています。 詳細については、「 [Windows フォーム DataGridView コントロールのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
### <a name="scenario-key-points"></a>シナリオのキーポイント  
  
- <xref:System.Windows.Forms.DataGridViewCellStyle> オブジェクトを使用すると、色、フォント、書式設定、位置情報を、コントロールの複数のレベルや個々の要素に対して行うことができます。  
  
- セルスタイルを複数の要素で重ねて共有して、コードを再利用することができます。  
  
## <a name="scenario-6-advanced-customization"></a>シナリオ 6: 高度なカスタマイズ  
 <xref:System.Windows.Forms.DataGridView> コントロールには、外観と動作をカスタマイズするためのさまざまな方法が用意されています。  
  
### <a name="scenario-key-points"></a>シナリオのキーポイント  
  
- 独自のセルの描画コードを指定できます。 詳細については、「[方法: Windows フォーム DataGridView コントロールのセルの外観をカスタマイズする](customize-the-appearance-of-cells-in-the-datagrid.md)」を参照してください。  
  
- 独自の行の描画を提供できます。 これは、複数の列にまたがるコンテンツを含む行を作成する場合などに便利です。 詳細については、「[方法: Windows フォーム DataGridView コントロールの行の外観をカスタマイズする](customize-the-appearance-of-rows-in-the-datagrid.md)」を参照してください。  
  
- 独自のセルおよび列クラスを実装して、セルの外観をカスタマイズできます。 詳細については、「[方法: 動作と外観を拡張して Windows フォーム DataGridView コントロールのセルと列をカスタマイズする](customize-cells-and-columns-in-the-datagrid-by-extending-behavior.md)」を参照してください。  
  
- 組み込みの列型によって提供されるもの以外のコントロールをホストするために、独自のセルクラスおよび列クラスを実装できます。 詳細については、「[方法: Windows フォーム DataGridView セルのコントロールをホストする](how-to-host-controls-in-windows-forms-datagridview-cells.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- [DataGridView コントロールの概要](datagridview-control-overview-windows-forms.md)
