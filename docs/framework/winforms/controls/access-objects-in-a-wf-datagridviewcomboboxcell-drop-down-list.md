---
title: DataGridViewComboBoxCell ドロップダウンリストのオブジェクトにアクセスする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGridView control [Windows Forms], accessing objects in combo box cells
- combo boxes [Windows Forms], in DataGridView control
- combo boxes [Windows Forms], accessing objects in DataGridViewComboBoxCell drop-down lists
ms.assetid: bcbe794a-d1fa-47f8-b5a3-5f085b32097d
ms.openlocfilehash: 7e76ab1ac9089778e4371f4ee65b06d5ebc570bf
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746315"
---
# <a name="how-to-access-objects-in-a-windows-forms-datagridviewcomboboxcell-drop-down-list"></a>方法 : Windows フォームの DataGridViewComboBoxCell ドロップダウン リストのオブジェクトにアクセスする
<xref:System.Windows.Forms.ComboBox> コントロールと同様に、<xref:System.Windows.Forms.DataGridViewComboBoxColumn> と <xref:System.Windows.Forms.DataGridViewComboBoxCell> の種類を使用すると、任意のオブジェクトをドロップダウンリストに追加できます。 この機能を使用すると、対応するオブジェクトを別のコレクションに格納しなくても、ドロップダウンリストで複雑な状態を表すことができます。  
  
 <xref:System.Windows.Forms.ComboBox> コントロールとは異なり、<xref:System.Windows.Forms.DataGridView> 型には、現在選択されているオブジェクトを取得するための <xref:System.Windows.Forms.ComboBox.SelectedItem%2A> プロパティはありません。 代わりに、<xref:System.Windows.Forms.DataGridViewComboBoxColumn.ValueMember%2A?displayProperty=nameWithType> または <xref:System.Windows.Forms.DataGridViewComboBoxCell.ValueMember%2A?displayProperty=nameWithType> プロパティをビジネスオブジェクトのプロパティの名前に設定する必要があります。 ユーザーが選択を行うと、ビジネスオブジェクトの指定されたプロパティによって、セル <xref:System.Windows.Forms.DataGridViewCell.Value%2A> プロパティが設定されます。  
  
 セル値を使用してビジネスオブジェクトを取得するには、`ValueMember` プロパティに、ビジネスオブジェクト自体への参照を返すプロパティが指定されている必要があります。 したがって、ビジネスオブジェクトの型がコントロールの下にない場合は、継承によって型を拡張することによって、このようなプロパティを追加する必要があります。  
  
 次の手順では、ビジネスオブジェクトを使用してドロップダウンリストを設定し、セル <xref:System.Windows.Forms.DataGridViewCell.Value%2A> プロパティを使用してオブジェクトを取得する方法について説明します。  
  
### <a name="to-add-business-objects-to-the-drop-down-list"></a>ドロップダウンリストにビジネスオブジェクトを追加するには  
  
1. 新しい <xref:System.Windows.Forms.DataGridViewComboBoxColumn> を作成し、その <xref:System.Windows.Forms.DataGridViewComboBoxColumn.Items%2A> コレクションを設定します。 または、[列 <xref:System.Windows.Forms.DataGridViewComboBoxColumn.DataSource%2A>] プロパティをビジネスオブジェクトのコレクションに設定することもできます。 ただし、コレクション内に対応するビジネスオブジェクトを作成しなくても、ドロップダウンリストに "未割り当て" を追加することはできません。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewComboBoxObjectBinding#110](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewComboBoxObjectBinding/CS/form1.cs#110)]
     [!code-vb[System.Windows.Forms.DataGridViewComboBoxObjectBinding#110](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewComboBoxObjectBinding/vb/form1.vb#110)]  
  
2. <xref:System.Windows.Forms.DataGridViewComboBoxColumn.DisplayMember%2A> と <xref:System.Windows.Forms.DataGridViewComboBoxColumn.ValueMember%2A> プロパティを設定します。 <xref:System.Windows.Forms.DataGridViewComboBoxColumn.DisplayMember%2A> は、ドロップダウンリストに表示するビジネスオブジェクトのプロパティを示します。 <xref:System.Windows.Forms.DataGridViewComboBoxColumn.ValueMember%2A> は、ビジネスオブジェクトへの参照を返すプロパティを示します。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewComboBoxObjectBinding#115](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewComboBoxObjectBinding/CS/form1.cs#115)]
     [!code-vb[System.Windows.Forms.DataGridViewComboBoxObjectBinding#115](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewComboBoxObjectBinding/vb/form1.vb#115)]  
  
3. ビジネスオブジェクト型に、現在のインスタンスへの参照を返すプロパティが含まれていることを確認します。 このプロパティには、前の手順で <xref:System.Windows.Forms.DataGridViewComboBoxColumn.ValueMember%2A> に割り当てられた値を指定する必要があります。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewComboBoxObjectBinding#310](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewComboBoxObjectBinding/CS/form1.cs#310)]
     [!code-vb[System.Windows.Forms.DataGridViewComboBoxObjectBinding#310](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewComboBoxObjectBinding/vb/form1.vb#310)]  
  
### <a name="to-retrieve-the-currently-selected-business-object"></a>現在選択されているビジネスオブジェクトを取得するには  
  
- セル <xref:System.Windows.Forms.DataGridViewCell.Value%2A> プロパティを取得し、ビジネスオブジェクト型にキャストします。  
  
     [!code-csharp[System.Windows.Forms.DataGridViewComboBoxObjectBinding#120](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewComboBoxObjectBinding/CS/form1.cs#120)]
     [!code-vb[System.Windows.Forms.DataGridViewComboBoxObjectBinding#120](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewComboBoxObjectBinding/vb/form1.vb#120)]  
  
## <a name="example"></a>例  
 完全な例では、ドロップダウンリストでビジネスオブジェクトを使用する方法を示します。 この例では、<xref:System.Windows.Forms.DataGridView> コントロールが `Task` オブジェクトのコレクションにバインドされています。 各 `Task` オブジェクトには、そのタスクに現在割り当てられている `Employee` オブジェクトを示す `AssignedTo` プロパティがあります。 `Assigned To` 列には、割り当てられた各従業員の `Name` プロパティ値が表示されます。また、`Task.AssignedTo` プロパティ値が `null`場合は、"未割り当て" が表示されます。  
  
 この例の動作を表示するには、次の手順を実行します。  
  
1. ドロップダウンリストから別の値を選択するか、コンボボックスのセルで CTRL + 0 キーを押して、[`Assigned To`] 列の割り当てを変更します。  
  
2. 現在の割り当てを表示するには、[`Generate Report`] をクリックします。 これは、`Assigned To` 列の変更によって `tasks` コレクションが自動的に更新されることを示しています。  
  
3. `Request Status` ボタンをクリックして、その行の現在の `Employee` オブジェクトの `RequestStatus` メソッドを呼び出します。 これは、選択したオブジェクトが正常に取得されたことを示しています。  
  
 [!code-csharp[System.Windows.Forms.DataGridViewComboBoxObjectBinding#000](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewComboBoxObjectBinding/CS/form1.cs#000)]
 [!code-vb[System.Windows.Forms.DataGridViewComboBoxObjectBinding#000](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewComboBoxObjectBinding/vb/form1.vb#000)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- System アセンブリおよび System.Windows.Forms アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewComboBoxColumn>
- <xref:System.Windows.Forms.DataGridViewComboBoxColumn.Items%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewComboBoxColumn.DataSource%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewComboBoxColumn.ValueMember%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewComboBoxCell>
- <xref:System.Windows.Forms.DataGridViewComboBoxCell.Items%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewComboBoxCell.DataSource%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewComboBoxCell.ValueMember%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridViewCell.Value%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.ComboBox>
- [Windows フォーム DataGridView コントロールでのデータの表示](displaying-data-in-the-windows-forms-datagridview-control.md)
