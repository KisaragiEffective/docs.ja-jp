---
title: BindingSource コンポーネントを使用した ADO.NET データの並べ替えとフィルター処理
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- sorting data
- data sorting [Windows Forms], ADO.NET
- data [Windows Forms], filtering
- BindingSource component [Windows Forms], sorting and filtering data
- filtering [Windows Forms], ADO.NET
- data [Windows Forms], sorting
- ADO.NET [Windows Forms]
ms.assetid: 6c206daf-d706-4602-9dbe-435343052063
ms.openlocfilehash: cf1da3bb94b26449eb72b0e4930b262236487acc
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742969"
---
# <a name="how-to-sort-and-filter-adonet-data-with-the-windows-forms-bindingsource-component"></a>方法 : Windows フォーム BindingSource コンポーネントで ADO.NET データを並べ替える/フィルター処理する
<xref:System.Windows.Forms.BindingSource.Sort%2A> と <xref:System.Windows.Forms.BindingSource.Filter%2A> のプロパティを使用して、<xref:System.Windows.Forms.BindingSource> コントロールの並べ替えとフィルター処理の機能を公開できます。 基になるデータソースが <xref:System.ComponentModel.IBindingList>であれば、単純な並べ替えを適用できます。また、データソースが <xref:System.ComponentModel.IBindingListView>である場合は、フィルター処理や高度な並べ替えを適用できます。 <xref:System.Windows.Forms.BindingSource.Sort%2A> プロパティには、標準の ADO.NET 構文が必要です。これには、データソース内のデータ列の名前を表す文字列と、リストを昇順と降順のどちらで並べ替えるかを示す `ASC` または `DESC` を指定します。 各列をコンマ区切り記号で区切ることによって、高度な並べ替えまたは複数列の並べ替えを設定できます。 <xref:System.Windows.Forms.BindingSource.Filter%2A> プロパティは、文字列式を受け取ります。  
  
> [!NOTE]
> 接続文字列内に機密情報 (パスワードなど) を格納すると、アプリケーションのセキュリティに影響を及ぼすことがあります。 データベースへのアクセスを制御する方法としては、Windows 認証 (統合セキュリティとも呼ばれます) を使用する方が安全です。 詳細については、「[接続情報の保護](../../data/adonet/protecting-connection-information.md)」を参照してください。  
  
### <a name="to-filter-data-with-the-bindingsource"></a>BindingSource を使用してデータをフィルター処理するには  
  
- <xref:System.Windows.Forms.BindingSource.Filter%2A> プロパティを目的の式に設定します。  
  
     次のコード例では、式は列名の後に列に必要な値を指定しています。  
  
 [!code-csharp[System.Windows.Forms.DataConnectorFilterAndSort#11](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorFilterAndSort/CS/form1.cs#11)]
 [!code-vb[System.Windows.Forms.DataConnectorFilterAndSort#11](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorFilterAndSort/VB/form1.vb#11)]  
  
### <a name="to-sort-data-with-the-bindingsource"></a>BindingSource を使用してデータを並べ替えるには  
  
1. <xref:System.Windows.Forms.BindingSource.Sort%2A> プロパティに、`ASC` または `DESC` 昇順または降順を示す列名を設定します。  
  
2. 複数の列をコンマで区切ります。  
  
 [!code-csharp[System.Windows.Forms.DataConnectorFilterAndSort#12](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorFilterAndSort/CS/form1.cs#12)]
 [!code-vb[System.Windows.Forms.DataConnectorFilterAndSort#12](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorFilterAndSort/VB/form1.vb#12)]  
  
## <a name="example"></a>例  
 次のコード例では、Northwind サンプルデータベースの Customers テーブルから <xref:System.Windows.Forms.DataGridView> コントロールにデータを読み込み、表示されるデータをフィルター処理して並べ替えます。  
  
 [!code-csharp[System.Windows.Forms.DataConnectorFilterAndSort#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorFilterAndSort/CS/form1.cs#1)]
 [!code-vb[System.Windows.Forms.DataConnectorFilterAndSort#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataConnectorFilterAndSort/VB/form1.vb#1)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例を実行するには、`BindingSource1` という名前の <xref:System.Windows.Forms.BindingSource> と `dataGridView1`という名前の <xref:System.Windows.Forms.DataGridView> を含むフォームにコードを貼り付けます。 フォームの <xref:System.Windows.Forms.Form.Load> イベントを処理し、load イベントハンドラーメソッドで `InitializeSortedFilteredBindingSource` を呼び出します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.BindingSource.Sort%2A>
- <xref:System.Windows.Forms.BindingSource.Filter%2A>
- [方法 : サンプル データベースをインストールする](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/8b6y4c7s(v=vs.120))
- [BindingSource コンポーネント](bindingsource-component.md)
