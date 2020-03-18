---
title: '方法: DataGrid コントロールを使用して検証を実装する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGrid [WPF], validation
- validation [WPF], DataGrid
ms.assetid: ec6078a8-1e42-4648-b414-f4348e81bda1
ms.openlocfilehash: 401949aa4bea924b458a91dc00c454c97aff4895
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458453"
---
# <a name="how-to-implement-validation-with-the-datagrid-control"></a>方法: DataGrid コントロールを使用して検証を実装する
<xref:System.Windows.Controls.DataGrid> コントロールを使用すると、セルレベルと行レベルの両方で検証を実行できます。 セルレベルの検証では、ユーザーが値を更新するときに、バインドされたデータオブジェクトの個々のプロパティを検証します。 行レベルの検証では、ユーザーが変更を行にコミットするときに、データオブジェクト全体を検証します。 また、検証エラーに対してカスタマイズされた視覚的フィードバックを提供したり、<xref:System.Windows.Controls.DataGrid> コントロールが提供する既定のビジュアルフィードバックを使用したりすることもできます。  
  
 次の手順では、<xref:System.Windows.Controls.DataGrid> バインディングに検証規則を適用し、視覚的なフィードバックをカスタマイズする方法について説明します。  
  
### <a name="to-validate-individual-cell-values"></a>個々のセル値を検証するには  
  
- 列で使用するバインディングに対して1つ以上の検証規則を指定します。 これは、「[データバインディングの概要](../data/data-binding-overview.md)」で説明されているように、単純なコントロールでのデータの検証に似ています。  
  
     次の例は、ビジネスオブジェクトの異なるプロパティに4つの列がバインドされた <xref:System.Windows.Controls.DataGrid> コントロールを示しています。 列の3つは、<xref:System.Windows.Data.Binding.ValidatesOnExceptions%2A> プロパティを `true`に設定することによって、<xref:System.Windows.Controls.ExceptionValidationRule> を指定します。  
  
     [!code-xaml[DataGrid_Validation#BasicXaml](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/window1.xaml#basicxaml)]  
  
     ユーザーが無効な値 ([Course ID] 列に整数以外の値) を入力すると、そのセルの周囲に赤い枠線が表示されます。 この既定の検証フィードバックは、次の手順に従って変更できます。  
  
### <a name="to-customize-cell-validation-feedback"></a>セルの検証に関するフィードバックをカスタマイズするには  
  
- 列の <xref:System.Windows.Controls.DataGridBoundColumn.EditingElementStyle%2A> プロパティを、列の編集コントロールに適したスタイルに設定します。 編集コントロールは実行時に作成されるため、単純なコントロールの場合と同様に、添付プロパティ <xref:System.Windows.Controls.Validation.ErrorTemplate%2A?displayProperty=nameWithType> を使用することはできません。  
  
     次の例では、検証規則を持つ3つの列によって共有されているエラースタイルを追加して、前の例を更新します。 ユーザーが無効な値を入力すると、スタイルによってセルの背景色が変更され、ツールヒントが追加されます。 トリガーを使用して、検証エラーが発生しているかどうかを確認することに注意してください。 これは、セルに専用のエラーテンプレートがないために必要です。  
  
     [!code-xaml[DataGrid_Validation#CellValidationXaml](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/mainwindow.xaml#cellvalidationxaml)]  
  
     列で使用されている <xref:System.Windows.Controls.DataGridColumn.CellStyle%2A> を置き換えることで、より広範なカスタマイズを実装できます。  
  
### <a name="to-validate-multiple-values-in-a-single-row"></a>1つの行で複数の値を検証するには  
  
1. バインドされたデータオブジェクトの複数のプロパティをチェックする <xref:System.Windows.Controls.ValidationRule> サブクラスを実装します。 <xref:System.Windows.Controls.ValidationRule.Validate%2A> メソッドの実装では、`value` パラメーター値を <xref:System.Windows.Data.BindingGroup> インスタンスにキャストします。 その後、<xref:System.Windows.Data.BindingGroup.Items%2A> プロパティを使用してデータオブジェクトにアクセスできます。  
  
     次の例では、`Course` オブジェクトの `StartDate` プロパティ値が `EndDate` プロパティ値よりも前かどうかを検証するプロセスを示します。  
  
     [!code-csharp[DataGrid_Validation#CourseValidationRule](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/mainwindow.xaml.cs#coursevalidationrule)]
     [!code-vb[DataGrid_Validation#CourseValidationRule](~/samples/snippets/visualbasic/VS_Snippets_Wpf/datagrid_validation/vb/mainwindow.xaml.vb#coursevalidationrule)]  
  
2. 検証規則を <xref:System.Windows.Controls.DataGrid.RowValidationRules%2A?displayProperty=nameWithType> コレクションに追加します。 <xref:System.Windows.Controls.DataGrid.RowValidationRules%2A> プロパティは、コントロールによって使用されるすべてのバインドをグループ化する <xref:System.Windows.Data.BindingGroup> インスタンスの <xref:System.Windows.Data.BindingGroup.ValidationRules%2A> プロパティへの直接アクセスを提供します。  
  
     次の例では、XAML で <xref:System.Windows.Controls.DataGrid.RowValidationRules%2A> プロパティを設定します。 バインドされたデータオブジェクトが更新された後にのみ検証が行われるように、<xref:System.Windows.Controls.ValidationRule.ValidationStep%2A> プロパティは <xref:System.Windows.Controls.ValidationStep.UpdatedValue> に設定されています。  
  
     [!code-xaml[DataGrid_Validation#RowValidationRulesXaml](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/mainwindow.xaml#rowvalidationrulesxaml)]  
  
     ユーザーが開始日よりも前の終了日を指定すると、行ヘッダーに赤い感嘆符 (!) が表示されます。 この既定の検証フィードバックは、次の手順に従って変更できます。  
  
### <a name="to-customize-row-validation-feedback"></a>行の検証に関するフィードバックをカスタマイズするには  
  
- <xref:System.Windows.Controls.DataGrid.RowValidationErrorTemplate%2A?displayProperty=nameWithType> プロパティを設定します。 このプロパティを使用すると、個々の <xref:System.Windows.Controls.DataGrid> コントロールに対する行の検証フィードバックをカスタマイズできます。 また、暗黙的な行スタイルを使用して <xref:System.Windows.Controls.DataGridRow.ValidationErrorTemplate%2A?displayProperty=nameWithType> のプロパティを設定することによって、複数のコントロールに影響を与えることもできます。  
  
     次の例では、行の検証に関する既定のフィードバックを、よりわかりやすいインジケーターに置き換えています。 ユーザーが無効な値を入力すると、行ヘッダーに白い感嘆符を含む赤い円が表示されます。 これは、行とセルの両方の検証エラーに対して発生します。 関連するエラーメッセージがツールヒントに表示されます。  
  
     [!code-xaml[DataGrid_Validation#RowValidationFeedbackXaml](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/mainwindow.xaml#rowvalidationfeedbackxaml)]  
  
## <a name="example"></a>例  
 次の例では、セルと行の検証の完全なデモを示します。 `Course` クラスには、トランザクションをサポートするための <xref:System.ComponentModel.IEditableObject> を実装するサンプルデータオブジェクトが用意されています。 <xref:System.Windows.Controls.DataGrid> コントロールは <xref:System.ComponentModel.IEditableObject> と対話し、ユーザーが ESC キーを押して変更を元に戻すことができるようにします。  
  
> [!NOTE]
> Visual Basic を使用する場合は、Mainwindow.xaml の最初の行で `x:Class="DataGridValidation.MainWindow"` を `x:Class="MainWindow"`に置き換えます。  
  
 検証をテストするには、次の操作を実行します。  
  
- [Course ID] 列に、整数以外の値を入力します。  
  
- [終了日] 列に、開始日よりも前の日付を入力します。  
  
- コース ID、開始日、または終了日の値を削除します。  
  
- 無効なセル値を元に戻すには、セルにカーソルを置き、ESC キーを押します。  
  
- 現在のセルが編集モードのときに行全体の変更を元に戻すには、ESC キーを2回押します。  
  
- 検証エラーが発生した場合は、行ヘッダーのインジケーターの上にマウスポインターを移動すると、関連付けられているエラーメッセージが表示されます。  
  
 [!code-csharp[DataGrid_Validation#FullCode](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/mainwindow.xaml.cs#fullcode)]
 [!code-vb[DataGrid_Validation#FullCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/datagrid_validation/vb/mainwindow.xaml.vb#fullcode)]  
  
 [!code-xaml[DataGrid_Validation#FullXaml](~/samples/snippets/csharp/VS_Snippets_Wpf/datagrid_validation/cs/mainwindow.xaml#fullxaml)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Controls.DataGrid>
- [DataGrid](datagrid.md)
- [データ バインディング](../../../desktop-wpf/data/data-binding-overview.md)
- [バインディングの検証の実装](../data/how-to-implement-binding-validation.md)
- [カスタム オブジェクトに検証ロジックを実装する](../data/how-to-implement-validation-logic-on-custom-objects.md)
