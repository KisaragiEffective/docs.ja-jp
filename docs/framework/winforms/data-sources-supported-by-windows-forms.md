---
title: サポートされるデータ ソース
ms.date: 03/30/2017
helpviewer_keywords:
- collections [Windows Forms], binding to
- OLE DB providers [Windows Forms], Windows Forms
- DataTable class [Windows Forms], binding and Windows Forms
- Windows Forms, data binding
- DataView class [Windows Forms], binding and Windows Forms
- DataViewManager class [Windows Forms], binding and Windows Forms
- Windows Forms, source data
- arrays [Windows Forms]
- data sources [Windows Forms]
- data providers [Windows Forms]
- DataSet class [Windows Forms], binding and Windows Forms
- data [Windows Forms], data providers
ms.assetid: 3d2c43f6-462b-4d35-9c86-13e9afe012e1
ms.openlocfilehash: 83ce4bb0d6f0bf81bcad4e38af212dccc3483ca5
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742315"
---
# <a name="data-sources-supported-by-windows-forms"></a>Windows フォームがサポートするデータ ソース
従来、データベースに格納されているデータを利用するために、アプリケーション内でデータバインディングが使用されていました。 Windows フォームデータバインディングを使用すると、特定の最小要件が満たされていれば、データベースのデータだけでなく、配列やコレクションなどの他の構造体のデータにもアクセスできます。  
  
## <a name="structures-to-bind-to"></a>バインド先の構造体  
 Windows フォームでは、単純なオブジェクト (単純なバインド) から ADO.NET data tables (complex binding) などの複雑なリストにまで、さまざまな構造体にバインドできます。 単純なバインドの場合、Windows フォームは、単純なオブジェクトのパブリックプロパティへのバインドをサポートします。 Windows フォームリストベースのバインディングでは、通常、オブジェクトが <xref:System.Collections.IList> インターフェイスまたは <xref:System.ComponentModel.IListSource> インターフェイスをサポートする必要があります。 さらに、<xref:System.Windows.Forms.BindingSource> コンポーネントを通じてバインドする場合は、<xref:System.Collections.IEnumerable> インターフェイスをサポートするオブジェクトにバインドできます。 データバインディングに関連するインターフェイスの詳細については、「[データバインディングに関連するインターフェイス](interfaces-related-to-data-binding.md)」を参照してください。  
  
 次の一覧は Windows フォームでバインドできる構造を示しています。  
  
 <xref:System.Windows.Forms.BindingSource>  
 <xref:System.Windows.Forms.BindingSource> は、最も一般的な Windows フォームデータソースであり、データソースと Windows フォームコントロールの間のプロキシ機能を果たします。 一般的な <xref:System.Windows.Forms.BindingSource> 使用パターンでは、コントロールを <xref:System.Windows.Forms.BindingSource> にバインドし、<xref:System.Windows.Forms.BindingSource> をデータソース (たとえば、ADO.NET データテーブルまたはビジネスオブジェクト) にバインドします。 <xref:System.Windows.Forms.BindingSource> には、データバインディングサポートのレベルを有効にし、改善するサービスが用意されています。 たとえば、<xref:System.Windows.Forms.DataGridView> や <xref:System.Windows.Forms.ComboBox> などの Windows フォームリストベースのコントロールは、<xref:System.Collections.IEnumerable> データソースへのバインドを直接サポートしていませんが、<xref:System.Windows.Forms.BindingSource>を使用してバインドすることによって、このシナリオを有効にすることができます。 この場合、<xref:System.Windows.Forms.BindingSource> によって、データソースが <xref:System.Collections.IList>に変換されます。  
  
 単純なオブジェクト  
 Windows フォームは、<xref:System.Windows.Forms.Binding> 型を使用して、オブジェクトのインスタンスのパブリックプロパティに対するデータバインディングコントロールプロパティをサポートします。 Windows フォームは、<xref:System.Windows.Forms.BindingSource> が使用されている場合のオブジェクトインスタンスへの <xref:System.Windows.Forms.ListControl> など、バインドリストベースのコントロールもサポートします。  
  
 配列またはコレクション  
 データソースとして機能するには、リストに <xref:System.Collections.IList> インターフェイスを実装する必要があります。1つの例として、<xref:System.Array> クラスのインスタンスである配列が挙げられます。 配列の詳細については、「[方法: オブジェクトの配列を作成する (Visual Basic)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/487y7874(v=vs.100))」を参照してください。  
  
 一般に、データバインディング用のオブジェクトのリストを作成する場合は、<xref:System.ComponentModel.BindingList%601> を使用する必要があります。 <xref:System.ComponentModel.BindingList%601> は、<xref:System.ComponentModel.IBindingList> インターフェイスのジェネリックバージョンです。 <xref:System.ComponentModel.IBindingList> インターフェイスは、双方向のデータバインディングに必要なプロパティ、メソッド、およびイベントを追加することによって、<xref:System.Collections.IList> インターフェイスを拡張します。  
  
 <xref:System.Collections.IEnumerable>  
 Windows フォームコントロールは、<xref:System.Windows.Forms.BindingSource> コンポーネントを通じてバインドされている場合にのみ、<xref:System.Collections.IEnumerable> インターフェイスをサポートするデータソースにバインドできます。  
  
 ADO.NET data オブジェクト  
 ADO.NET には、へのバインドに適したさまざまなデータ構造体が用意されています。 それぞれの方法は、洗練され、複雑さによって異なります。  
  
- [https://login.microsoftonline.com/consumers/](<xref:System.Data.DataColumn>) <xref:System.Data.DataColumn> は、テーブルを構成する複数の列で構成される <xref:System.Data.DataTable>の重要な構成要素です。 各 <xref:System.Data.DataColumn> には、列に保持されるデータの種類を決定する <xref:System.Data.DataColumn.DataType%2A> のプロパティがあります (たとえば、自動車を説明するテーブルの自動車の作成)。 コントロール (<xref:System.Windows.Forms.TextBox> コントロールの <xref:System.Windows.Forms.Control.Text%2A> プロパティなど) をデータテーブル内の列に単純にバインドすることができます。  
  
- [https://login.microsoftonline.com/consumers/](<xref:System.Data.DataTable>) <xref:System.Data.DataTable> は、ADO.NET に行と列を含むテーブルを表現したものです。 データテーブルには、<xref:System.Data.DataColumn>という2つのコレクションが含まれています。これは、指定されたテーブル内のデータの列 (最終的には、そのテーブルに入力できるデータの種類を決定します) を表し、<xref:System.Data.DataRow>は特定のテーブル内のデータ行を表します。 データテーブルに格納されている情報 (<xref:System.Windows.Forms.DataGridView> コントロールをデータテーブルにバインドするなど) に、コントロールを複雑にバインドできます。 ただし、<xref:System.Data.DataTable>にバインドすると、実際にはテーブルの既定のビューにバインドされます。  
  
- [https://login.microsoftonline.com/consumers/](<xref:System.Data.DataView>) <xref:System.Data.DataView> は、フィルター処理または並べ替えが可能な単一のデータテーブルのカスタマイズされたビューです。 データビューは、複合バインドコントロールによって使用されるデータ "スナップショット" です。 データビュー内のデータに単純バインドまたは複雑なバインドを行うことができますが、クリーンな更新データソースではなく、データの固定の "画像" にバインドすることに注意してください。  
  
- [https://login.microsoftonline.com/consumers/](<xref:System.Data.DataSet>) <xref:System.Data.DataSet> は、データベース内のデータのテーブル、リレーションシップ、および制約のコレクションです。 データセット内のデータに単純バインドまたは複雑なバインドを行うことができますが、<xref:System.Data.DataSet> の既定の <xref:System.Data.DataViewManager> にバインドしている点に注意してください (次の箇条書きを参照)。  
  
- [https://login.microsoftonline.com/consumers/](<xref:System.Data.DataViewManager>) <xref:System.Data.DataViewManager> は、<xref:System.Data.DataView>に似ていますが、関係が含まれている <xref:System.Data.DataSet>全体のカスタマイズされたビューです。 <xref:System.Data.DataViewManager.DataViewSettings%2A> コレクションを使用すると、特定のテーブルに対して <xref:System.Data.DataViewManager> が持つすべてのビューに対して、既定のフィルターおよび並べ替えオプションを設定できます。  
  
## <a name="see-also"></a>参照

- [Windows フォーム データ バインドの変更通知](change-notification-in-windows-forms-data-binding.md)
- [データ連結と Windows フォーム](data-binding-and-windows-forms.md)
- [Windows フォームでのデータ バインディング](windows-forms-data-binding.md)
