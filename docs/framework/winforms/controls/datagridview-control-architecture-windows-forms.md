---
title: DataGridView コントロールのアーキテクチャ
ms.date: 03/30/2017
helpviewer_keywords:
- DataGridView control [Windows Forms], architecture
ms.assetid: 1c6cabf0-02ee-4bbc-9574-b54bb7f5b19e
ms.openlocfilehash: 2e1884383cca87f8d4ff84f486e2b29761a0c55d
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742527"
---
# <a name="datagridview-control-architecture-windows-forms"></a>DataGridView コントロールのアーキテクチャ (Windows フォーム)
<xref:System.Windows.Forms.DataGridView> コントロールとその関連クラスは、表形式のデータを表示および編集するための柔軟で拡張可能なシステムとして設計されています。 これらのクラスはすべて、<xref:System.Windows.Forms?displayProperty=nameWithType> 名前空間に含まれており、すべて "DataGridView" プレフィックスで名前が付けられています。  
  
## <a name="architecture-elements"></a>アーキテクチャの要素  
 プライマリ <xref:System.Windows.Forms.DataGridView> コンパニオンクラスは <xref:System.Windows.Forms.DataGridViewElement>から派生します。 次のオブジェクトモデルは、<xref:System.Windows.Forms.DataGridViewElement> 継承階層を示しています。  
  
 ![DataGridViewElement オブジェクトモデル階層を示す図。](./media/datagridview-control-architecture-windows-forms/datagridviewelement-object-model.gif)  
  
 <xref:System.Windows.Forms.DataGridViewElement> クラスは、親 <xref:System.Windows.Forms.DataGridView> コントロールへの参照を提供し、<xref:System.Windows.Forms.DataGridViewElementStates> 列挙体の値の組み合わせを表す値を保持する <xref:System.Windows.Forms.DataGridViewElement.State%2A> プロパティを持ちます。  
  
 以下のセクションでは、<xref:System.Windows.Forms.DataGridView> コンパニオンクラスについて詳しく説明します。  
  
### <a name="datagridviewelementstates"></a>DataGridViewElementStates  
 <xref:System.Windows.Forms.DataGridViewElementStates> 列挙体には次の値が含まれます。  
  
- <xref:System.Windows.Forms.DataGridViewElementStates.None>  
  
- <xref:System.Windows.Forms.DataGridViewElementStates.Frozen>  
  
- <xref:System.Windows.Forms.DataGridViewElementStates.ReadOnly>  
  
- <xref:System.Windows.Forms.DataGridViewElementStates.Resizable>  
  
- <xref:System.Windows.Forms.DataGridViewElementStates.ResizableSet>  
  
- <xref:System.Windows.Forms.DataGridViewElementStates.Selected>  
  
- <xref:System.Windows.Forms.DataGridViewElementStates.Visible>  
  
 この列挙体の値はビットごとの論理演算子と組み合わせることができるため、<xref:System.Windows.Forms.DataGridViewElement.State%2A> プロパティは一度に複数の状態を表すことができます。 たとえば、<xref:System.Windows.Forms.DataGridViewElement> は、同時に <xref:System.Windows.Forms.DataGridViewElementStates.Frozen>、<xref:System.Windows.Forms.DataGridViewElementStates.Selected>、および <xref:System.Windows.Forms.DataGridViewElementStates.Visible>にすることができます。  
  
### <a name="cells-and-bands"></a>セルとバンド  
 <xref:System.Windows.Forms.DataGridView> コントロールは、セルとバンドの2つの基本的な種類のオブジェクトで構成されています。 すべてのセルは <xref:System.Windows.Forms.DataGridViewCell> 基底クラスから派生します。 2種類のバンド (<xref:System.Windows.Forms.DataGridViewColumn> と <xref:System.Windows.Forms.DataGridViewRow>) は、どちらも <xref:System.Windows.Forms.DataGridViewBand> 基底クラスから派生します。  
  
 <xref:System.Windows.Forms.DataGridView> コントロールは複数のクラスと相互運用しますが、最も一般的に発生するのは、<xref:System.Windows.Forms.DataGridViewCell>、<xref:System.Windows.Forms.DataGridViewColumn>、および <xref:System.Windows.Forms.DataGridViewRow>です。  
  
### <a name="datagridviewcell"></a>DataGridViewCell  
 セルは、<xref:System.Windows.Forms.DataGridView>の相互作用の基本単位です。 表示はセルの中央に配置され、多くの場合、データ入力はセルを通じて実行されます。 <xref:System.Windows.Forms.DataGridViewRow> クラスの <xref:System.Windows.Forms.DataGridViewRow.Cells%2A> コレクションを使用してセルにアクセスできます。また、<xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Windows.Forms.DataGridView.SelectedCells%2A> コレクションを使用して、選択したセルにアクセスできます。 次のオブジェクトモデルは、この使用方法を示し、<xref:System.Windows.Forms.DataGridViewCell> 継承階層を示しています。  
  
 ![DataGridViewCell オブジェクトモデル階層を示す図。](./media/datagridview-control-architecture-windows-forms/datagridviewcell-object-model.gif)  
  
 <xref:System.Windows.Forms.DataGridViewCell> 型は抽象基本クラスであり、すべてのセル型が派生します。 <xref:System.Windows.Forms.DataGridViewCell> とその派生型は Windows フォームコントロールではなく、一部のホスト Windows フォームコントロールです。 セルでサポートされる編集機能は、通常、ホストされるコントロールによって処理されます。  
  
 <xref:System.Windows.Forms.DataGridViewCell> オブジェクトは、Windows フォームコントロールと同じように、独自の外観と描画機能を制御しません。 代わりに、<xref:System.Windows.Forms.DataGridView> は <xref:System.Windows.Forms.DataGridViewCell> オブジェクトの外観を担います。 <xref:System.Windows.Forms.DataGridView> コントロールのプロパティとイベントを操作することによって、セルの外観と動作に大きな影響を与えることができます。 <xref:System.Windows.Forms.DataGridView> コントロールの機能を超えるカスタマイズに特別な要件がある場合は、<xref:System.Windows.Forms.DataGridViewCell> またはその子クラスの1つから派生する独自のクラスを実装できます。  
  
 <xref:System.Windows.Forms.DataGridViewCell>から派生したクラスを次に示します。  
  
- <xref:System.Windows.Forms.DataGridViewTextBoxCell>  
  
- <xref:System.Windows.Forms.DataGridViewButtonCell>  
  
- <xref:System.Windows.Forms.DataGridViewLinkCell>  
  
- <xref:System.Windows.Forms.DataGridViewCheckBoxCell>  
  
- <xref:System.Windows.Forms.DataGridViewComboBoxCell>  
  
- <xref:System.Windows.Forms.DataGridViewImageCell>  
  
- <xref:System.Windows.Forms.DataGridViewHeaderCell>  
  
- <xref:System.Windows.Forms.DataGridViewRowHeaderCell>  
  
- <xref:System.Windows.Forms.DataGridViewColumnHeaderCell>  
  
- <xref:System.Windows.Forms.DataGridViewTopLeftHeaderCell>  
  
- カスタムセルの種類  
  
### <a name="datagridviewcolumn"></a>DataGridViewColumn  
 <xref:System.Windows.Forms.DataGridView> コントロールのアタッチされたデータストアのスキーマは、<xref:System.Windows.Forms.DataGridView> コントロールの列で表されます。 <xref:System.Windows.Forms.DataGridView> コントロールの列にアクセスするには、<xref:System.Windows.Forms.DataGridView.Columns%2A> コレクションを使用します。 選択した列にアクセスするには、<xref:System.Windows.Forms.DataGridView.SelectedColumns%2A> コレクションを使用します。 次のオブジェクトモデルは、この使用方法を示し、<xref:System.Windows.Forms.DataGridViewColumn> 継承階層を示しています。  
  
 ![DataGridViewColumn オブジェクトモデル階層を示す図。](./media/datagridview-control-architecture-windows-forms/datagridviewcolumn-object-model.gif)  
  
 主なセル型には、対応する列の型があります。 これらは、<xref:System.Windows.Forms.DataGridViewColumn> の基本クラスから派生します。  
  
 <xref:System.Windows.Forms.DataGridViewColumn>から派生したクラスを次に示します。  
  
- <xref:System.Windows.Forms.DataGridViewButtonColumn>  
  
- <xref:System.Windows.Forms.DataGridViewCheckBoxColumn>  
  
- <xref:System.Windows.Forms.DataGridViewComboBoxColumn>  
  
- <xref:System.Windows.Forms.DataGridViewImageColumn>  
  
- <xref:System.Windows.Forms.DataGridViewTextBoxColumn>  
  
- <xref:System.Windows.Forms.DataGridViewLinkColumn>  
  
- カスタム列の型  
  
### <a name="datagridview-editing-controls"></a>DataGridView 編集コントロール  
 高度な編集機能をサポートするセルは、通常、Windows フォームコントロールから派生したホストされたコントロールを使用します。 これらのコントロールは、<xref:System.Windows.Forms.IDataGridViewEditingControl> インターフェイスも実装します。 次のオブジェクトモデルは、これらのコントロールの使用方法を示しています。  
  
 ![DataGridView 編集コントロールオブジェクトモデルの階層を示す図。](./media/datagridview-control-architecture-windows-forms/datagridviewediting-object-model.gif)  
  
 <xref:System.Windows.Forms.DataGridView> コントロールには、次の編集コントロールが用意されています。  
  
- <xref:System.Windows.Forms.DataGridViewComboBoxEditingControl>  
  
- <xref:System.Windows.Forms.DataGridViewTextBoxEditingControl>  
  
 独自の編集コントロールを作成する方法については、「[方法: Windows フォーム DataGridView セルのコントロールをホスト](how-to-host-controls-in-windows-forms-datagridview-cells.md)する」を参照してください。  
  
 次の表は、セルの種類、列の種類、および編集コントロールの関係を示しています。  
  
|セルの種類|ホストされるコントロール|列の型|  
|---------------|--------------------|-----------------|  
|<xref:System.Windows.Forms.DataGridViewButtonCell>|該当なし|<xref:System.Windows.Forms.DataGridViewButtonColumn>|  
|<xref:System.Windows.Forms.DataGridViewCheckBoxCell>|該当なし|<xref:System.Windows.Forms.DataGridViewCheckBoxColumn>|  
|<xref:System.Windows.Forms.DataGridViewComboBoxCell>|<xref:System.Windows.Forms.DataGridViewComboBoxEditingControl>|<xref:System.Windows.Forms.DataGridViewComboBoxColumn>|  
|<xref:System.Windows.Forms.DataGridViewImageCell>|該当なし|<xref:System.Windows.Forms.DataGridViewImageColumn>|  
|<xref:System.Windows.Forms.DataGridViewLinkCell>|該当なし|<xref:System.Windows.Forms.DataGridViewLinkColumn>|  
|<xref:System.Windows.Forms.DataGridViewTextBoxCell>|<xref:System.Windows.Forms.DataGridViewTextBoxEditingControl>|<xref:System.Windows.Forms.DataGridViewTextBoxColumn>|  
  
### <a name="datagridviewrow"></a>DataGridViewRow  
 <xref:System.Windows.Forms.DataGridViewRow> クラスは、<xref:System.Windows.Forms.DataGridView> コントロールがアタッチされているデータストアのレコードのデータフィールドを表示します。 <xref:System.Windows.Forms.DataGridView.Rows%2A> コレクションを使用して、<xref:System.Windows.Forms.DataGridView> コントロールの行にアクセスできます。 <xref:System.Windows.Forms.DataGridView.SelectedRows%2A> コレクションを使用して、選択した行にアクセスできます。 次のオブジェクトモデルは、この使用方法を示し、<xref:System.Windows.Forms.DataGridViewRow> 継承階層を示しています。  
  
 ![DataGridViewRow オブジェクトモデル階層を示す図。](./media/datagridview-control-architecture-windows-forms/datagridviewrow-object-model.gif)
  
 <xref:System.Windows.Forms.DataGridViewRow> クラスから独自の型を派生させることもできますが、通常は必要ありません。 <xref:System.Windows.Forms.DataGridView> コントロールには、<xref:System.Windows.Forms.DataGridViewRow> オブジェクトの動作をカスタマイズするための行関連のイベントとプロパティがいくつかあります。  
  
 <xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Windows.Forms.DataGridView.AllowUserToAddRows%2A> プロパティを有効にすると、新しい行を追加するための特殊な行が最後の行として表示されます。 この行は <xref:System.Windows.Forms.DataGridView.Rows%2A> コレクションの一部ですが、注意が必要な特別な機能を備えています。 詳細については、「 [Windows フォーム DataGridView コントロールでの新しいレコードの行の使用](using-the-row-for-new-records-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [DataGridView コントロールの概要](datagridview-control-overview-windows-forms.md)
- [Windows フォーム DataGridView コントロールのカスタマイズ](customizing-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールにおける新規レコード行の使用](using-the-row-for-new-records-in-the-windows-forms-datagridview-control.md)
