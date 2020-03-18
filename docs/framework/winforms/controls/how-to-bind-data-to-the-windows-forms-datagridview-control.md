---
title: DataGridView コントロールにデータをバインドする
ms.date: 02/08/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data binding [Windows Forms], grids
- data binding [Windows Forms], DataGridView control
- DataGridView control [Windows Forms], data binding
ms.assetid: 1660f69c-5711-45d2-abc1-e25bc6779124
ms.openlocfilehash: e2762bf363a469abf8c1e57b851d351c1cb41b62
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745076"
---
# <a name="how-to-bind-data-to-the-windows-forms-datagridview-control"></a>方法: Windows フォーム DataGridView コントロールにデータをバインドする

<xref:System.Windows.Forms.DataGridView> コントロールでは、標準の Windows フォームデータバインディングモデルがサポートされているため、さまざまなデータソースにバインドできます。 通常、データソースとの対話を管理する <xref:System.Windows.Forms.BindingSource> にバインドします。 <xref:System.Windows.Forms.BindingSource> には任意の Windows フォームデータソースを指定できます。これにより、データの場所を選択または変更する際の柔軟性が向上します。 <xref:System.Windows.Forms.DataGridView> コントロールでサポートされているデータソースの詳細については、「 [DataGridView コントロールの概要](datagridview-control-overview-windows-forms.md)」を参照してください。  

Visual Studio では、DataGridView コントロールへのデータバインドが広範囲にサポートされています。 詳細については、「[方法: デザイナーを使用してデータを Windows フォーム DataGridView コントロールにバインド](bind-data-to-the-datagrid-using-the-designer.md)する」を参照してください。  

DataGridView コントロールをデータに接続するには:

1. データの取得の詳細を処理するメソッドを実装します。 次のコード例では、<xref:System.Data.SqlClient.SqlDataAdapter>を初期化し、それを使用して <xref:System.Data.DataTable>を設定する `GetData` メソッドを実装しています。 次に、<xref:System.Data.DataTable> を <xref:System.Windows.Forms.BindingSource>にバインドします。 

2. フォームの <xref:System.Windows.Forms.Form.Load> イベントハンドラーで、<xref:System.Windows.Forms.DataGridView> コントロールを <xref:System.Windows.Forms.BindingSource>にバインドし、`GetData` メソッドを呼び出してデータを取得します。  

## <a name="example"></a>例

この完全なコード例では、データベースからデータを取得して、Windows フォームに DataGridView コントロールを設定します。 フォームには、データを再読み込みし、変更をデータベースに送信するためのボタンもあります。  

この例で必要な要素は次のとおりです。 

- Northwind SQL Server サンプルデータベースへのアクセス。 Northwind サンプルデータベースのインストールの詳細については、「 [ADO.NET コードサンプルのサンプルデータベースの取得](../../data/adonet/sql/linq/downloading-sample-databases.md)」を参照してください。 

- システム、System.xml、System.xml、および System.xml の各アセンブリへの参照です。  

この例をビルドして実行するには、新しい Windows フォームプロジェクトの*Form1*コードファイルにコードを貼り付けます。 C#または Visual Basic コマンドラインからのビルドの詳細については、「 [csc.exe を使用したコマンドライン](../../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md)からのビルド」または「[コマンドラインからのビルド](../../../visual-basic/reference/command-line-compiler/building-from-the-command-line.md)」を参照してください。  
  
Northwind SQL Server サンプルデータベース接続の値を使用して、例の `connectionString` 変数を設定します。 Windows 認証 (統合セキュリティとも呼ばれます) は、接続文字列にパスワードを格納するよりも、データベースに接続するより安全な方法です。 接続のセキュリティの詳細については、「[接続情報の保護](../../data/adonet/protecting-connection-information.md)」を参照してください。  

[!code-csharp[System.Windows.Forms.DataGridViewBoundEditable](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewBoundEditable/CS/datagridviewboundeditable.cs)]
[!code-vb[System.Windows.Forms.DataGridViewBoundEditable](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewBoundEditable/VB/datagridviewboundeditable.vb)]  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.DataSource%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.BindingSource>
- [Windows フォーム DataGridView コントロールにデータを表示する](displaying-data-in-the-windows-forms-datagridview-control.md)
- [接続情報の保護](../../data/adonet/protecting-connection-information.md)
