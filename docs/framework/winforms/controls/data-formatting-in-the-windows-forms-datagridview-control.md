---
title: DataGridView コントロールでのデータの書式設定
ms.date: 03/30/2017
helpviewer_keywords:
- DataGridView control [Windows Forms], formatting data
- data [Windows Forms], formatting in grids
- data grids [Windows Forms], formatting data
ms.assetid: 07bf558d-3748-42ba-8ba0-37fdef924081
ms.openlocfilehash: a041bcc5e1b65afb3123f1f42e0f1b5a479b5764
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743982"
---
# <a name="data-formatting-in-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロールでのデータの書式設定
<xref:System.Windows.Forms.DataGridView> コントロールは、セル値と親列に表示されるデータ型の間の自動変換を提供します。 たとえば、テキストボックスの列では、日付、時刻、数値、列挙値の文字列形式を表示し、ユーザーが入力した文字列値をデータストアで必要な型に変換します。  
  
## <a name="formatting-with-the-datagridviewcellstyle-class"></a>DataGridViewCellStyle クラスを使用した書式設定  
 <xref:System.Windows.Forms.DataGridView> コントロールは、<xref:System.Windows.Forms.DataGridViewCellStyle> クラスを使用して、セル値の基本的なデータ書式設定を提供します。 <xref:System.Windows.Forms.DataGridViewCellStyle.Format%2A> プロパティを使用すると、「[型の書式設定](../../../standard/base-types/formatting-types.md)」で説明されている書式指定子を使用して、現在の既定のカルチャの日付、時刻、数値、および列挙値の書式を設定できます。 また、<xref:System.Windows.Forms.DataGridViewCellStyle.FormatProvider%2A> プロパティを使用して、特定のカルチャの値を書式設定することもできます。 指定された形式は、データの表示と、ユーザーが指定した形式で入力したデータの解析の両方に使用されます。  
  
 <xref:System.Windows.Forms.DataGridViewCellStyle> クラスは、折り返し、テキストの配置、および null データベース値のカスタム表示に関する追加の書式設定プロパティを提供します。 詳細については、「[方法 : Windows フォーム DataGridView コントロールのデータの書式を設定する](how-to-format-data-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
## <a name="formatting-with-the-cellformatting-event"></a>CellFormatting 設定イベントを使用した書式設定  
 基本的な書式設定がニーズに合わない場合は、<xref:System.Windows.Forms.DataGridView.CellFormatting?displayProperty=nameWithType> イベントのハンドラーにカスタムデータ書式を提供できます。 ハンドラーに渡される <xref:System.Windows.Forms.DataGridViewCellFormattingEventArgs> には、最初にセル値を格納する <xref:System.Windows.Forms.ConvertEventArgs.Value%2A> プロパティがあります。 通常、この値は自動的に表示の種類に変換されます。 値を自分で変換するには、<xref:System.Windows.Forms.ConvertEventArgs.Value%2A> プロパティを表示の種類の値に設定します。  
  
> [!NOTE]
> 書式指定文字列がセルに対して有効になっている場合、<xref:System.Windows.Forms.DataGridViewCellFormattingEventArgs.FormattingApplied%2A> プロパティを `true`に設定しない限り、<xref:System.Windows.Forms.ConvertEventArgs.Value%2A> プロパティ値の変更はオーバーライドされます。  
  
 <xref:System.Windows.Forms.DataGridView.CellFormatting> イベントは、個々のセルの値に基づいて <xref:System.Windows.Forms.DataGridViewCellStyle> プロパティを設定する場合にも便利です。 詳細については、「[方法: Windows フォーム DataGridView コントロールでデータの書式をカスタマイズする](how-to-customize-data-formatting-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
 ユーザー指定の値の既定の解析がニーズに合わない場合は、<xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Windows.Forms.DataGridView.CellParsing> イベントを処理して、カスタム解析を提供できます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewCellStyle>
- [Windows フォーム DataGridView コントロールでのデータの表示](displaying-data-in-the-windows-forms-datagridview-control.md)
- [Windows フォーム DataGridView コントロールでのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールのデータの書式を設定する](how-to-format-data-in-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールのデータの書式設定をカスタマイズする](how-to-customize-data-formatting-in-the-windows-forms-datagridview-control.md)
