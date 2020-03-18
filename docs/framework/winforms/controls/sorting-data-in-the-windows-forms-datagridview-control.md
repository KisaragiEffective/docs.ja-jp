---
title: DataGridView コントロールでのデータの並べ替え
ms.date: 02/13/2018
helpviewer_keywords:
- data [Windows Forms], sorting in grids
- data grids [Windows Forms], sorting data
- DataGridView control [Windows Forms], sorting data
ms.assetid: c1d4f24c-d961-4181-809d-5a5caa6122e4
ms.openlocfilehash: 1fcd5a5f5c6d690c573c4c2c5fa7c32aa0292441
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742952"
---
# <a name="sorting-data-in-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロールでのデータの並べ替え

既定では、ユーザーはテキストボックスの列のヘッダーをクリックして <xref:System.Windows.Forms.DataGridView> コントロールのデータを並べ替えることができます (または、テキストボックスのセルが4.7.2 以降 .NET Framework のバージョンにフォーカスされている場合は、F3 キーを押します)。 特定の列の <xref:System.Windows.Forms.DataGridViewColumn.SortMode> プロパティを変更することで、ユーザーが他の列の種類を基準に並べ替えられるようにすることができます。 また、任意の列または複数の列を使用して、データをプログラムによって並べ替えることもできます。

## <a name="in-this-section"></a>このセクションの内容

[Windows フォーム DataGridView コントロール内の列の並べ替えモード](column-sort-modes-in-the-windows-forms-datagridview-control.md)  
コントロール内のデータを並べ替えるためのオプションについて説明します。

[方法: Windows フォーム DataGridView コントロール内の列の並べ替えモードを設定する](set-the-sort-modes-for-columns-wf-datagridview-control.md)  
既定では並べ替えられていない列でユーザーが並べ替えることができるようにする方法について説明します。

[方法: Windows フォーム DataGridView コントロールの並べ替え機能をカスタマイズする](how-to-customize-sorting-in-the-windows-forms-datagridview-control.md)  
プログラムによってデータを並べ替える方法と、<xref:System.Windows.Forms.DataGridView.SortCompare?displayProperty=nameWithType> イベントを使用するか <xref:System.Collections.IComparer> インターフェイスを実装することによって並べ替えをカスタマイズする方法について説明します。

## <a name="reference"></a>リファレンス

<xref:System.Windows.Forms.DataGridView>  
<xref:System.Windows.Forms.DataGridView> コントロールのリファレンス ドキュメントを提供します。  

<xref:System.Windows.Forms.DataGridView.Sort%2A?displayProperty=nameWithType>  
<xref:System.Windows.Forms.DataGridView.Sort%2A> メソッドのリファレンスドキュメントを提供します。

<xref:System.Windows.Forms.DataGridViewColumn.SortMode%2A?displayProperty=nameWithType>  
<xref:System.Windows.Forms.DataGridViewColumn.SortMode%2A> プロパティのリファレンスドキュメントを提供します。

<xref:System.Windows.Forms.DataGridViewColumnSortMode>  
<xref:System.Windows.Forms.DataGridViewColumnSortMode> 列挙体に関するリファレンスドキュメントを提供します。

## <a name="see-also"></a>参照

- [DataGridView コントロール](datagridview-control-windows-forms.md)
- [Windows フォーム DataGridView コントロールの列型](column-types-in-the-windows-forms-datagridview-control.md)
