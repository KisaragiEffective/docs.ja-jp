---
title: DataGridView コントロールと DataGrid コントロールの違い
ms.date: 03/30/2017
helpviewer_keywords:
- data grids [Windows Forms
- DataGrid control [Windows Forms], DataGridView control compared
- DataGridView control [Windows Forms], DataGrid control compared
ms.assetid: d412c786-140e-4210-8a56-a68467530a55
ms.openlocfilehash: 3552abe55d8e2c1cb4ae372ca64c7ca21f1fed0e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745958"
---
# <a name="differences-between-the-windows-forms-datagridview-and-datagrid-controls"></a>Windows フォームの DataGridView コントロールと DataGrid コントロールの違いについて
<xref:System.Windows.Forms.DataGridView> コントロールは、<xref:System.Windows.Forms.DataGrid> コントロールを置き換える新しいコントロールです。 <xref:System.Windows.Forms.DataGridView> コントロールには、<xref:System.Windows.Forms.DataGrid> コントロールに不足している基本的な機能と高度な機能が多数用意されています。 さらに、<xref:System.Windows.Forms.DataGridView> コントロールのアーキテクチャにより、<xref:System.Windows.Forms.DataGrid> コントロールよりもはるかに簡単に拡張およびカスタマイズできます。  
  
 次の表では、<xref:System.Windows.Forms.DataGrid> コントロールにない <xref:System.Windows.Forms.DataGridView> コントロールで使用できる主な機能のいくつかについて説明します。  
  
|DataGridView コントロール機能|[説明]|  
|----------------------------------|-----------------|  
|複数の列の型|<xref:System.Windows.Forms.DataGridView> コントロールには、<xref:System.Windows.Forms.DataGrid> コントロールよりも多くの組み込み列型が用意されています。 これらの列の型は、最も一般的なシナリオのニーズを満たしますが、<xref:System.Windows.Forms.DataGrid> コントロールの列の型よりも簡単に拡張または置換することができます。 詳細については、「 [Windows フォーム DataGridView コントロールの列の型](column-types-in-the-windows-forms-datagridview-control.md)」を参照してください。|  
|データを表示する複数の方法|<xref:System.Windows.Forms.DataGrid> コントロールは、外部データソースのデータの表示に限定されます。 ただし、<xref:System.Windows.Forms.DataGridView> コントロールでは、コントロールに格納されている非バインドデータ、バインドされたデータソースのデータ、またはバインドされたデータとバインドされていないデータの両方を表示できます。 また、<xref:System.Windows.Forms.DataGridView> コントロールに仮想モードを実装して、カスタムデータ管理を提供することもできます。 詳細については、「 [Windows フォーム DataGridView コントロールのデータ表示モード](data-display-modes-in-the-windows-forms-datagridview-control.md)」を参照してください。|  
|データの表示をカスタマイズする複数の方法|<xref:System.Windows.Forms.DataGridView> コントロールには、データの書式設定方法と表示方法を指定できるさまざまなプロパティとイベントが用意されています。 たとえば、格納されているデータに応じてセル、行、および列の外観を変更することや、あるデータ型のデータを別の型の同等のデータに置き換えることができます。 詳細については、「 [Windows フォーム DataGridView コントロールでのデータの書式設定](data-formatting-in-the-windows-forms-datagridview-control.md)」を参照してください。|  
|セル、行、列、およびヘッダーの外観と動作を変更するための複数のオプション|<xref:System.Windows.Forms.DataGridView> コントロールを使用すると、さまざまな方法で個々のグリッドコンポーネントを操作できます。 たとえば、行と列を固定してスクロールできないようにすることができます。行、列、およびヘッダーを非表示にします。行、列、およびヘッダーのサイズを調整する方法を変更します。ユーザーが選択する方法を変更します。とには、個々のセル、行、および列のツールヒントとショートカットメニューが用意されています。|  
  
 <xref:System.Windows.Forms.DataGrid> コントロールは、旧バージョンとの互換性を維持し、特別なニーズに対応するために残されています。 ほぼすべての目的で、<xref:System.Windows.Forms.DataGridView> コントロールを使用する必要があります。 <xref:System.Windows.Forms.DataGridView> コントロールでは使用できない <xref:System.Windows.Forms.DataGrid> コントロールで使用できる唯一の機能は、1つのコントロール内の2つの関連テーブルの情報を階層的に表示することです。 マスター/詳細リレーションシップに含まれる2つのテーブルの情報を表示するには、2つの <xref:System.Windows.Forms.DataGridView> コントロールを使用する必要があります。  
  
## <a name="upgrading-to-the-datagridview-control"></a>DataGridView コントロールへのアップグレード  
 カスタマイズせずに単純なデータバインドシナリオで <xref:System.Windows.Forms.DataGrid> コントロールを使用する既存のアプリケーションがある場合は、単純に古いコントロールを新しいコントロールに置き換えることができます。 どちらのコントロールも標準の Windows フォームデータバインディングアーキテクチャを使用します。そのため、<xref:System.Windows.Forms.DataGridView> コントロールは、追加の構成を必要とせずにバインドされたデータを表示します。 ただし、データバインディングの強化を利用する場合は、データを <xref:System.Windows.Forms.BindingSource> コンポーネントにバインドして、<xref:System.Windows.Forms.DataGridView> コントロールにバインドできるようにすることをお勧めします。 詳細については、「 [BindingSource コンポーネント](bindingsource-component.md)」を参照してください。  
  
 <xref:System.Windows.Forms.DataGridView> コントロールにはまったく新しいアーキテクチャがあるため、<xref:System.Windows.Forms.DataGridView> コントロールで <xref:System.Windows.Forms.DataGrid> カスタマイズを使用できるようにするための単純な変換パスはありません。 ただし、新しいコントロールで使用できる組み込みの機能により、多くの <xref:System.Windows.Forms.DataGrid> カスタマイズは、<xref:System.Windows.Forms.DataGridView> コントロールでは不要です。 <xref:System.Windows.Forms.DataGridView> コントロールで使用する <xref:System.Windows.Forms.DataGrid> コントロールに対してカスタムの列の型を作成した場合は、新しいアーキテクチャを使用してもう一度実装する必要があります。 詳細については、「 [Windows フォーム DataGridView コントロールのカスタマイズ](customizing-the-windows-forms-datagridview-control.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGrid>
- <xref:System.Windows.Forms.BindingSource>
- [DataGridView コントロール](datagridview-control-windows-forms.md)
- [DataGrid コントロール](datagrid-control-windows-forms.md)
- [BindingSource コンポーネント](bindingsource-component.md)
- [Windows フォーム DataGridView コントロールの列型](column-types-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのデータ表示モード](data-display-modes-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのデータの書式設定](data-formatting-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールのサイズ変更オプション](sizing-options-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロール内の列の並べ替えモード](column-sort-modes-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールの選択モード](selection-modes-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールのカスタマイズ](customizing-the-windows-forms-datagridview-control.md)
