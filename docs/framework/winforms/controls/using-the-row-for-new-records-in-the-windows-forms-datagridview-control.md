---
title: DataGridView コントロールの新しいレコードに対して行を使用する
ms.date: 03/30/2017
helpviewer_keywords:
- DataGridView control [Windows Forms], adding rows for new records
- rows [Windows Forms], new records
- DataGridView control [Windows Forms], data entry
ms.assetid: 6110f1ea-9794-442c-a98a-f104a1feeaf4
ms.openlocfilehash: 2fcd35f8c4d04909cdbc26f6a4293fdd570385b8
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76728340"
---
# <a name="using-the-row-for-new-records-in-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロールにおける新規レコード行の使用
アプリケーションのデータを編集するために <xref:System.Windows.Forms.DataGridView> を使用する場合、多くの場合、ユーザーがデータストアに新しいデータ行を追加できるようにする必要があります。 <xref:System.Windows.Forms.DataGridView> コントロールは、常に最後の行として表示される新しいレコードの行を提供することによって、この機能をサポートします。 行ヘッダーにアスタリスク (*) 記号が付いています。 次のセクションでは、新しいレコードを有効にする行をプログラミングするときに考慮する必要がある事項について説明します。  
  
## <a name="displaying-the-row-for-new-records"></a>新しいレコードの行を表示する  
 新しいレコードの行を表示するかどうかを指定するには、<xref:System.Windows.Forms.DataGridView.AllowUserToAddRows%2A> プロパティを使用します。 このプロパティの既定値は `true` です。  
  
 データバインドケースでは、コントロールの <xref:System.Windows.Forms.DataGridView.AllowUserToAddRows%2A> プロパティとデータソースの <xref:System.ComponentModel.IBindingList.AllowNew%2A?displayProperty=nameWithType> プロパティの両方が `true`場合、新しいレコードの行が表示されます。 どちらかが `false` の場合、行は表示されません。  
  
## <a name="populating-the-row-for-new-records-with-default-data"></a>既定のデータを使用して新しいレコードの行を設定する  
 ユーザーが新しいレコードの行を現在の行として選択すると、<xref:System.Windows.Forms.DataGridView> コントロールによって <xref:System.Windows.Forms.DataGridView.DefaultValuesNeeded> イベントが発生します。  
  
 このイベントは、新しい <xref:System.Windows.Forms.DataGridViewRow> へのアクセスを提供し、新しい行に既定のデータを設定できるようにします。 詳細については、「[方法: Windows フォーム DataGridView コントロールで新しい行の既定値を指定する](specify-default-values-for-new-rows-in-the-datagrid.md)」を参照してください。  
  
## <a name="the-rows-collection"></a>Rows コレクション  
 新しいレコードの行は <xref:System.Windows.Forms.DataGridView> コントロールの <xref:System.Windows.Forms.DataGridView.Rows%2A> コレクションに含まれていますが、次の2つの点で動作が異なります。  
  
- 新しいレコードの行は、プログラムで <xref:System.Windows.Forms.DataGridView.Rows%2A> コレクションから削除することはできません。 このような場合は、<xref:System.InvalidOperationException> がスローされます。 ユーザーは、新しいレコードの行を削除することもできません。 <xref:System.Windows.Forms.DataGridViewRowCollection.Clear%2A?displayProperty=nameWithType> メソッドでは、この行が <xref:System.Windows.Forms.DataGridView.Rows%2A> コレクションから削除されることはありません。  
  
- 新しいレコードの行の後に行を追加することはできません。 このような場合は、<xref:System.InvalidOperationException> が発生します。 その結果、新しいレコードの行は常に <xref:System.Windows.Forms.DataGridView> コントロールの最後の行になります。 行を追加する <xref:System.Windows.Forms.DataGridViewRowCollection> のメソッド (<xref:System.Windows.Forms.DataGridViewRowCollection.Add%2A>、<xref:System.Windows.Forms.DataGridViewRowCollection.AddCopy%2A>、および <xref:System.Windows.Forms.DataGridViewRowCollection.AddCopies%2A>) は、新しいレコードの行が存在する場合に、すべての挿入メソッドを内部的に呼び出します。  
  
## <a name="visual-customization-of-the-row-for-new-records"></a>新しいレコードの行のビジュアルカスタマイズ  
 新しいレコードの行が作成されると、その行は <xref:System.Windows.Forms.DataGridView.RowTemplate%2A> プロパティによって指定された行に基づいて作成されます。 この行に対して指定されていないセルスタイルは、他のプロパティから継承されます。 セルスタイルの継承の詳細については、「 [Windows フォーム DataGridView コントロールのセルのスタイル](cell-styles-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
 新しいレコードの行のセルによって表示される初期値は、各セルの <xref:System.Windows.Forms.DataGridViewCell.DefaultNewRowValue%2A> プロパティから取得されます。 <xref:System.Windows.Forms.DataGridViewImageCell>型のセルの場合、このプロパティはプレースホルダーイメージを返します。 それ以外の場合は、`null` を返します。 このプロパティをオーバーライドして、カスタム値を返すことができます。 ただし、これらの初期値は、新しいレコードの行にフォーカスが入ったときに、<xref:System.Windows.Forms.DataGridView.DefaultValuesNeeded> のイベントハンドラーに置き換えることができます。  
  
 この行のヘッダーの標準アイコン (矢印またはアスタリスク) は、一般公開されていません。 アイコンをカスタマイズする場合は、カスタム <xref:System.Windows.Forms.DataGridViewRowHeaderCell> クラスを作成する必要があります。  
  
 標準アイコンは、行ヘッダーセルによって使用されている <xref:System.Windows.Forms.DataGridViewCellStyle> の <xref:System.Windows.Forms.DataGridViewCellStyle.ForeColor%2A> プロパティを使用します。 標準アイコンは、完全に表示するための十分な領域がない場合はレンダリングされません。  
  
 行ヘッダーセルに文字列値が設定されていて、テキストとアイコンの両方に十分な領域がない場合は、まずアイコンがドロップされます。  
  
## <a name="sorting"></a>並べ替え  
 非バインドモードでは、ユーザーが <xref:System.Windows.Forms.DataGridView>の内容を並べ替えた場合でも、新しいレコードは常に <xref:System.Windows.Forms.DataGridView> の最後に追加されます。 ユーザーは、行を正しい位置に並べ替えるために並べ替えを再度適用する必要があります。この動作は、<xref:System.Windows.Forms.ListView> コントロールの動作と似ています。  
  
 データバインドモードと仮想モードでは、並べ替えが適用されるときの挿入動作は、データモデルの実装に依存します。 ADO.NET の場合、行はすぐに正しい位置に並べ替えられます。  
  
## <a name="other-notes-on-the-row-for-new-records"></a>新しいレコードの行に関するその他のメモ  
 この行の <xref:System.Windows.Forms.DataGridViewRow.Visible%2A> プロパティを `false`に設定することはできません。 このような場合は、<xref:System.InvalidOperationException> が発生します。  
  
 新しいレコードの行は、常に選択されていない状態で作成されます。  
  
## <a name="virtual-mode"></a>仮想モード  
 仮想モードを実装する場合は、データモデルで新しいレコードの行が必要になるタイミングと、行の追加をロールバックするタイミングを追跡する必要があります。 この機能の正確な実装は、データモデルの実装とそのトランザクションセマンティクスによって異なります。たとえば、コミットスコープがセルレベルまたは行レベルのどちらであるかなどです。 詳細については、「 [Windows フォーム DataGridView コントロールでの仮想モード](virtual-mode-in-the-windows-forms-datagridview-control.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.DefaultValuesNeeded?displayProperty=nameWithType>
- [Windows フォーム DataGridView コントロールでのデータ入力](data-entry-in-the-windows-forms-datagridview-control.md)
- [方法: Windows フォーム DataGridView コントロールの新しい行に既定値を指定する](specify-default-values-for-new-rows-in-the-datagrid.md)
