---
title: DataGridView コントロールでの列と行のサイズ変更
ms.date: 03/30/2017
helpviewer_keywords:
- DataGridView control [Windows Forms], sizing rows and columns
- columns [Windows Forms], resizing in grids
- data grids [Windows Forms], resizing columns and rows
ms.assetid: 7532764d-e5c1-4943-a08b-6377a722d3b6
ms.openlocfilehash: 8f8394a837ccc11c469f9ad4feeb60464d0014fe
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742411"
---
# <a name="resizing-columns-and-rows-in-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロール内の列と行のサイズ変更
`DataGridView` コントロールには、列と行のサイズ変更動作をカスタマイズするためのさまざまなオプションが用意されています。 通常、`DataGridView` のセルは、その内容に基づいてサイズが変更されることはありません。 代わりに、セルよりも大きい表示値をクリップします。 コンテンツを文字列として表示できる場合、セルはツールヒントに表示されます。  
  
 既定では、ユーザーは、行、列、およびヘッダーの区切り線をマウスでドラッグして、より多くの情報を表示できます。 ユーザーは、区分線をダブルクリックして、関連付けられている行、列、またはヘッダーのバンドのサイズをその内容に基づいて自動的に変更することもできます。  
  
 `DataGridView` コントロールには、これらのすべてのユーザー向け動作をカスタマイズまたは無効にするためのプロパティ、メソッド、およびイベントが用意されています。 また、行、列、ヘッダーの内容に合わせてプログラムによってサイズを変更することもできます。また、コンテンツが変更されるたびに自動的にサイズが変更されるように構成することもできます。 また、使用可能なコントロールの幅を指定した比率で自動的に分割するように列を構成することもできます。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [Windows フォーム DataGridView コントロールのサイズ変更オプション](sizing-options-in-the-windows-forms-datagridview-control.md)  
 行、列、およびヘッダーのサイズを変更するためのオプションについて説明します。 また、サイズに関連するプロパティとメソッドの詳細について説明し、一般的な使用シナリオについても説明します。  
  
 [Windows フォーム DataGridView コントロールの列フィル モード](column-fill-mode-in-the-windows-forms-datagridview-control.md)  
 列の塗りつぶしモードの詳細について説明し、列の塗りつぶしモードとその他のモードを試すために使用できるデモコードを示します。  
  
 [方法: Windows フォーム DataGridView コントロールのサイズ変更モードを設定する](how-to-set-the-sizing-modes-of-the-windows-forms-datagridview-control.md)  
 一般的な目的でサイズ変更モードを構成する方法について説明します。  
  
 [方法: Windows フォームの DataGridView コントロールの内容に合わせてセルのサイズをプログラムで変更する](programmatically-resize-cells-to-fit-content-in-the-datagrid.md)  
 プログラムによるサイズ変更のテストに使用できるデモコードを示します。  
  
 [方法: Windows フォームの DataGridView コントロールの内容変更時にセルのサイズを自動的に変更する](automatically-resize-cells-when-content-changes-in-the-datagrid.md)  
 自動サイズ調整モードを試すために使用できるデモコードを示します。  
  
## <a name="reference"></a>リファレンス  
 <xref:System.Windows.Forms.DataGridView>  
 <xref:System.Windows.Forms.DataGridView> コントロールのリファレンス ドキュメントを提供します。  
  
## <a name="see-also"></a>参照

- [DataGridView コントロール](datagridview-control-windows-forms.md)
