---
title: DataGridView コントロール テクノロジの概要
ms.date: 03/30/2017
helpviewer_keywords:
- DataGridView control [Windows Forms], about DataGridView control
- data grids [Windows Forms], about data grids
ms.assetid: 094498c3-a126-4a3f-83fe-f69e96c7717b
ms.openlocfilehash: 18eebd067df9768e14cc81258184551d00dd1402
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742475"
---
# <a name="datagridview-control-technology-summary-windows-forms"></a>DataGridView コントロール テクノロジの概要 (Windows フォーム)
ここでは、`DataGridView` コントロールおよびその使用をサポートしているクラスの概要について説明します。  
  
 表形式でのデータの表示は、頻繁に実行されるタスクです。 `DataGridView` コントロールは、グリッドにデータを表示するための完全なソリューションとして設計されています。  
  
## <a name="keywords"></a>Keywords  
 DataGridView、BindingSource、テーブル、セル、データバインディング、仮想モード  
  
## <a name="namespaces"></a>名前空間  
 <xref:System.Windows.Forms?displayProperty=nameWithType>  
  
 <xref:System.Data?displayProperty=nameWithType>  
  
## <a name="related-technologies"></a>関連技術情報  
 `BindingSource`  
  
## <a name="background"></a>バックグラウンド  
 ユーザーインターフェイス (UI) デザイナーでは、表形式のデータをユーザーに表示するために必要となることがよくあります。 .NET Framework には、テーブルまたはグリッドにデータを表示するいくつかの方法が用意されています。 `DataGridView` コントロールは、Windows フォームアプリケーションのこのテクノロジの最新の進化を表します。  
  
 `DataGridView` コントロールでは、データストアのデータ行を表示できます。 多くの種類のデータストアがサポートされています。 データストアは、1次元配列などの単純な型指定されていないデータを保持することも、<xref:System.Data.DataSet>などの型指定されたデータを保持することもできます。 詳細については、「[方法: データを Windows フォーム DataGridView コントロールにバインドする](how-to-bind-data-to-the-windows-forms-datagridview-control.md)」を参照してください。  
  
 `DataGridView` コントロールには、データを表形式で表示するための強力で柔軟な機能が用意されています。 コントロールを使用すると、サイズが非常に大きいデータセットの読み取り専用または編集可能なビューを表示できます。  
  
 複数の方法で `DataGridView` コントロールを拡張して、アプリケーションにカスタム動作を組み込むことができます。 たとえば、プログラムで独自の並べ替えアルゴリズムを指定したり、独自の種類のセルを作成したりできます。 `DataGridView` コントロールの外観は、いくつかのプロパティを選択することで簡単にカスタマイズできます。 多くの種類のデータストアをデータソースとして使用することも、データソースがバインドされていない状態で `DataGridView` コントロールを操作することもできます。  
  
## <a name="implementing-datagridview-classes"></a>DataGridView クラスの実装  
 `DataGridView` コントロールの機能拡張機能を利用するには、いくつかの方法があります。 イベントとプロパティを使用してコントロールのさまざまな要素をカスタマイズできますが、一部のカスタマイズでは、既存の `DataGridView` クラスから派生した新しいクラスを作成する必要があります。  
  
 最も一般的に使用される基底クラスは `DataGridViewCell` および `DataGridViewColumn`です。 `DataGridViewCell` またはその子クラスから独自の cell クラスを派生させることができます。 任意のセル型を任意の列に追加できますが、通常は、カスタムセル型のセルをホストする `DataGridViewColumn` から、既定でコンパニオン列クラスも派生します。  
  
 派生セルクラスに `IDataGridViewEditingCell` インターフェイスを実装して、編集機能を持つセル型を作成できますが、編集モードではコントロールをホストしません。 編集モードのセルでホストできるコントロールを作成するには、<xref:System.Windows.Forms.Control>から派生したクラスに `IDataGridViewEditingControl` インターフェイスを実装します。  
  
 詳細については、「[方法: 動作と外観を拡張して Windows フォーム Datagridview コントロールのセルと列をカスタマイズ](customize-cells-and-columns-in-the-datagrid-by-extending-behavior.md)する」および「[方法: Windows フォーム Datagridview セルのコントロールをホストする](how-to-host-controls-in-windows-forms-datagridview-cells.md)」を参照してください。  
  
## <a name="datagridview-classes-at-a-glance"></a>DataGridView クラスの概要  
 <xref:System.Windows.Forms>  
  
|テクノロジ領域|クラス/インターフェイス/構成要素|  
|---------------------|-------------------------------------------------|  
|データ バインディング|<xref:System.Windows.Forms.BindingSource>|  
|データプレゼンテーション|<xref:System.Windows.Forms.DataGridView><br /><br /> <xref:System.Windows.Forms.DataGridViewCell> と派生クラス<br /><br /> <xref:System.Windows.Forms.DataGridViewRow> と派生クラス<br /><br /> <xref:System.Windows.Forms.DataGridViewColumn> と派生クラス<br /><br /> <xref:System.Windows.Forms.DataGridViewCellStyle>|  
|<xref:System.Windows.Forms.DataGridView> の機能拡張|<xref:System.Windows.Forms.DataGridViewCell> と派生クラス<br /><br /> <xref:System.Windows.Forms.DataGridViewColumn> と派生クラス<br /><br /> <xref:System.Windows.Forms.IDataGridViewEditingCell><br /><br /> <xref:System.Windows.Forms.IDataGridViewEditingControl>|  
  
## <a name="whats-new"></a>新機能  
 <xref:System.Windows.Forms.DataGridView> コントロールは、Windows フォームで表形式のデータを表示するための完全なソリューションとして設計されています。 新しいアプリケーションを作成する場合は、<xref:System.Windows.Forms.DataGrid>などの他のソリューションを使用する前に、<xref:System.Windows.Forms.DataGridView> コントロールの使用を検討してください。 詳細については、「[Windows フォームの DataGridView コントロールと DataGrid コントロールの違いについて](differences-between-the-windows-forms-datagridview-and-datagrid-controls.md)」を参照してください。  
  
 <xref:System.Windows.Forms.DataGridView> コントロールは、<xref:System.Windows.Forms.BindingSource> コンポーネントと密接に連携して動作します。 このコンポーネントは、フォームのプライマリデータソースとして設計されています。 データソースの種類に関係なく、<xref:System.Windows.Forms.DataGridView> コントロールとそのデータソースとの間の相互作用を管理できます。  
  
## <a name="see-also"></a>参照

- [DataGridView コントロールの概要](datagridview-control-overview-windows-forms.md)
- [DataGridView コントロールのアーキテクチャ](datagridview-control-architecture-windows-forms.md)
- [接続情報の保護](../../data/adonet/protecting-connection-information.md)
