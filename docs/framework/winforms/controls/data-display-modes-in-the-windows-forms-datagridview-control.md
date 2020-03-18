---
title: DataGridView コントロールのデータ表示モード
ms.date: 03/30/2017
helpviewer_keywords:
- data [Windows Forms], display modes
- data grids [Windows Forms], display modes
- DataGridView control [Windows Forms], display modes
ms.assetid: 9755a030-3f3f-4705-a661-ba5a48a81875
ms.openlocfilehash: ad6be647e3a36904b055fd6771f539df28938fab
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744008"
---
# <a name="data-display-modes-in-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロールでのデータ表示モード
<xref:System.Windows.Forms.DataGridView> コントロールでは、バインド、バインド解除、および仮想の3つの異なるモードでデータを表示できます。 要件に基づいて最適なモードを選択します。  
  
## <a name="unbound"></a>非バインド  
 非バインドモードは、プログラムで管理する比較的少量のデータを表示する場合に適しています。 <xref:System.Windows.Forms.DataGridView> コントロールをバインドモードとしてデータソースに直接アタッチすることはありません。 代わりに、通常は <xref:System.Windows.Forms.DataGridViewRowCollection.Add%2A?displayProperty=nameWithType> メソッドを使用してコントロールにデータを設定する必要があります。  
  
 非バインドモードは、静的な読み取り専用のデータの場合や、外部データストアと対話する独自のコードを提供する場合に特に便利です。 ただし、ユーザーが外部データソースと対話できるようにする場合は、通常、バインドモードを使用します。  
  
 読み取り専用のバインドされていない <xref:System.Windows.Forms.DataGridView>を使用する例については、「方法: バインドされていない[Windows フォーム DataGridView コントロールを作成](how-to-create-an-unbound-windows-forms-datagridview-control.md)する」を参照してください。  
  
## <a name="bound"></a>Bound  
 バインドモードは、データストアとの自動操作を使用したデータの管理に適しています。 <xref:System.Windows.Forms.DataGridView.DataSource%2A> プロパティを設定して、<xref:System.Windows.Forms.DataGridView> コントロールをデータソースに直接アタッチできます。 コントロールがデータにバインドされると、データ行がプッシュされ、その部分を明示的に管理する必要がなくプルされます。 <xref:System.Windows.Forms.DataGridView.AutoGenerateColumns%2A> プロパティが `true`と、データソース内の各列によって、対応する列がコントロールに作成されます。 独自の列を作成する場合は、このプロパティを `false` に設定し、<xref:System.Windows.Forms.DataGridViewColumn.DataPropertyName%2A> プロパティを使用して、構成時に各列をバインドできます。 これは、既定で生成される型以外の列の型を使用する場合に便利です。 詳細については、「 [Windows フォーム DataGridView コントロールの列の型](column-types-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
 バインドされた <xref:System.Windows.Forms.DataGridView> コントロールを使用する例については、「[チュートリアル: Windows フォーム DataGridView コントロールでのデータの検証](walkthrough-validating-data-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
 バインドモードで <xref:System.Windows.Forms.DataGridView> コントロールに非バインド列を追加することもできます。 これは、ユーザーが特定の行に対して操作を実行できるようにするボタンまたはリンクの列を表示する場合に便利です。 また、バインドされた列から計算された値を含む列を表示する場合にも便利です。 <xref:System.Windows.Forms.DataGridView.CellFormatting> イベントのハンドラーで、計算列のセル値を設定できます。 ただし、<xref:System.Data.DataSet> または <xref:System.Data.DataTable> をデータソースとして使用している場合は、代わりに、<xref:System.Data.DataColumn.Expression%2A?displayProperty=nameWithType> プロパティを使用して計算列を作成することをお勧めします。 この場合、<xref:System.Windows.Forms.DataGridView> コントロールは、計算列を、データソース内の他の列と同様に扱います。  
  
 バインドモードでの非バインド列による並べ替えはサポートされていません。 バインドモードでユーザー編集可能な値を含む非バインド列を作成する場合は、バインドされた列によってコントロールが並べ替えられるときに、仮想モードを実装してこれらの値を維持する必要があります。  
  
## <a name="virtual"></a>仮想的  
 仮想モードを使用すると、独自のデータ管理操作を実装できます。 バインドされた列によってコントロールが並べ替えられている場合は、バインドモードのバインドされていない列の値を維持するために必要です。 ただし、仮想モードの主な用途は、大量のデータを操作するときのパフォーマンスを最適化することです。  
  
 <xref:System.Windows.Forms.DataGridView> コントロールを管理するキャッシュにアタッチすると、データ行をプッシュおよびプルするタイミングがコードによって制御されます。 メモリフットプリントを小さくするために、キャッシュのサイズは現在表示されている行の数と同じにする必要があります。 ユーザーが新しい行をスクロールして表示すると、コードはキャッシュから新しいデータを要求し、必要に応じて、メモリから古いデータをフラッシュします。  
  
 仮想モードを実装する場合は、データモデルで新しい行が必要になったタイミングと、新しい行の追加をロールバックするタイミングを追跡する必要があります。 この機能の正確な実装は、データモデルの実装とデータモデルのトランザクションセマンティクスによって異なります。コミットスコープがセルレベルまたは行レベルのどちらであるかを示します。  
  
 仮想モードの詳細については、「 [Windows フォーム DataGridView コントロールでの仮想モード](virtual-mode-in-the-windows-forms-datagridview-control.md)」を参照してください。 仮想モードイベントの使用方法を示す例については、「[チュートリアル: Windows フォーム DataGridView コントロールでの仮想モードの実装](implementing-virtual-mode-wf-datagridview-control.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.DataSource%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.VirtualMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.BindingSource>
- <xref:System.Windows.Forms.DataGridViewColumn.DataPropertyName%2A?displayProperty=nameWithType>
- [Windows フォーム DataGridView コントロールでのデータの表示](displaying-data-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールの列型](column-types-in-the-windows-forms-datagridview-control.md)
- [チュートリアル: バインドされていない Windows フォーム DataGridView コントロールの作成](walkthrough-creating-an-unbound-windows-forms-datagridview-control.md)
- [方法: データを Windows フォーム DataGridView コントロールにバインドする](how-to-bind-data-to-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでの仮想モード](virtual-mode-in-the-windows-forms-datagridview-control.md)
- [チュートリアル: Windows フォーム DataGridView コントロールでの仮想モードの実装](implementing-virtual-mode-wf-datagridview-control.md)
