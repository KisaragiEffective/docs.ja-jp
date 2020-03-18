---
title: DataView によるフィルター処理 (LINQ to DataSet)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 5632d74a-ff53-4ea7-9fe7-4a148eeb1c68
ms.openlocfilehash: 426e8c43f0ff8af94ab56230cf002637f351cd75
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634860"
---
# <a name="filtering-with-dataview-linq-to-dataset"></a>DataView によるフィルター処理 (LINQ to DataSet)
特定の条件に基づいてデータをフィルター処理し、UI コントロールを介してそのデータをクライアントに提供する機能は、データ バインドの重要な特徴です。 <xref:System.Data.DataView> は、データにフィルターを適用し、特定のフィルター条件を満たすデータ行のサブセットを返す方法をいくつか提供します。 <xref:System.Data.DataView> には、文字列ベースのフィルター処理機能に加えて、フィルター条件に LINQ 式を使用する機能も用意されています。 LINQ 式では、文字列ベースのフィルター処理よりもはるかに複雑で強力なフィルター処理が可能です。  
  
 <xref:System.Data.DataView> を使用してデータをフィルター処理する方法は 2 つあります。  
  
- Where 句を使用して、LINQ to DataSet クエリから <xref:System.Data.DataView> を作成します。  
  
- <xref:System.Data.DataView> の既存の文字列ベースのフィルター機能を使用します。  
  
## <a name="creating-dataview-from-a-query-with-filtering-information"></a>フィルター情報を含むクエリによる DataView の作成  
 <xref:System.Data.DataView> オブジェクトは、LINQ to DataSet クエリから作成できます。 このクエリに `Where` 句が含まれている場合、<xref:System.Data.DataView> はクエリのフィルター情報を使用して作成されます。 `Where` 句内の式は、<xref:System.Data.DataView> に含めるデータ行の決定に使用され、これがフィルターの基礎となります。  
  
 式ベースのフィルターは、文字列ベースのフィルターよりもはるかに強力で複雑なフィルター機能を提供します。 文字列ベースのフィルターと式ベースのフィルターは、相互に排他的です。 <xref:System.Data.DataView.RowFilter%2A> をクエリから作成した後に文字列ベースの <xref:System.Data.DataView> を設定した場合、クエリから推論される式ベースのフィルターはクリアされます。  
  
> [!NOTE]
> ほとんどの場合、フィルターに使用する式は、副作用のない確定的な式である必要があります。 また、フィルター処理は任意の回数実行されるため、特定の実行回数に依存するロジックが式に含まれないようにしてください。  
  
### <a name="example"></a>使用例  
 次の例では、SalesOrderDetail テーブルに対して数量が 3 個以上 5 個以下の注文を取得するクエリを実行し、このクエリから <xref:System.Data.DataView> を作成します。次に、<xref:System.Data.DataView> を <xref:System.Windows.Forms.BindingSource> にバインドします。  
  
 [!code-csharp[DP DataView Samples#LDVFromQueryWhere](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvfromquerywhere)]
 [!code-vb[DP DataView Samples#LDVFromQueryWhere](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvfromquerywhere)]  
  
### <a name="example"></a>使用例  
 次の例では、2001 年 6 月 6 日以降に受けた注文に対するクエリから <xref:System.Data.DataView> を作成します。  
  
 [!code-csharp[DP DataView Samples#LDVFromQueryWhere3](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvfromquerywhere3)]
 [!code-vb[DP DataView Samples#LDVFromQueryWhere3](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvfromquerywhere3)]  
  
### <a name="example"></a>使用例  
 フィルターは並べ替えと組み合わせることもできます。 次の例では、姓が "S" で始まる連絡先を姓、名の順に並べ替えるクエリから <xref:System.Data.DataView> を作成します。  
  
 [!code-csharp[DP DataView Samples#LDVFromQueryWhereOrderByThenBy](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvfromquerywhereorderbythenby)]
 [!code-vb[DP DataView Samples#LDVFromQueryWhereOrderByThenBy](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvfromquerywhereorderbythenby)]  
  
### <a name="example"></a>使用例  
 次の例では、SoundEx アルゴリズムを使用して、"Zhu" に類似する姓を持つ連絡先を検索します。 SoundEx アルゴリズムは、SoundEx メソッドに実装されています。  
  
 [!code-csharp[DP DataView Samples#LDVSoundExFilter](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvsoundexfilter)]
 [!code-vb[DP DataView Samples#LDVSoundExFilter](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvsoundexfilter)]  
  
 SoundEx は、米国国勢調査局 (U.S. Census Bureau) で最初に開発された、名前を英語の発音でインデックス化する音声アルゴリズムです。 SoundEx メソッドは、1 つの名前に対して、1 つの英字とそれに続く 3 つの数字から構成される 4 文字のコードを返します。 文字は名前の最初の文字であり、数字は名前に含まれる残りの子音をコード化したものです。 発音が近い名前には、同じ SoundEx コードが与えられます。 前の例の SoundEx メソッドに使用されている SoundEx 実装を次に示します。  
  
 [!code-csharp[DP DataView Samples#SoundEx](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#soundex)]
 [!code-vb[DP DataView Samples#SoundEx](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#soundex)]  
  
## <a name="using-the-rowfilter-property"></a>RowFilter プロパティの使用  
 <xref:System.Data.DataView> の既存の文字列ベースのフィルター機能は、LINQ to DataSet のコンテキストでも動作します。 文字列ベースの <xref:System.Data.DataView.RowFilter%2A> フィルター処理の詳細については、「[データの並べ替えとフィルター処理](./dataset-datatable-dataview/sorting-and-filtering-data.md)」を参照してください。  
  
 次の例では、Contact テーブルから <xref:System.Data.DataView> を作成し、<xref:System.Data.DataView.RowFilter%2A> プロパティを設定して、連絡先の姓が "Zhu" である行を返します。  
  
 [!code-csharp[DP DataView Samples#LDVRowFilter](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvrowfilter)]
 [!code-vb[DP DataView Samples#LDVRowFilter](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvrowfilter)]  
  
 <xref:System.Data.DataTable> または LINQ to DataSet クエリから <xref:System.Data.DataView> を作成した後、<xref:System.Data.DataView.RowFilter%2A> プロパティを使用して、列の値に基づいて行のサブセットを指定できます。 文字列ベースのフィルターと式ベースのフィルターは、相互に排他的です。 <xref:System.Data.DataView.RowFilter%2A> プロパティを設定すると、LINQ to DataSet クエリから推論されるフィルター式がクリアされ、フィルター式をリセットできなくなります。  
  
 [!code-csharp[DP DataView Samples#LDVFromQueryWhereSetRowFilter](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvfromquerywheresetrowfilter)]
 [!code-vb[DP DataView Samples#LDVFromQueryWhereSetRowFilter](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvfromquerywheresetrowfilter)]  
  
 データ サブセットの動的ビューの作成とは対照的に、データに対する特定のクエリの結果を取得する場合は、<xref:System.Data.DataView.Find%2A> プロパティを設定する代わりに <xref:System.Data.DataView.FindRows%2A> の <xref:System.Data.DataView> メソッドまたは <xref:System.Data.DataView.RowFilter%2A> メソッドを使用します。 <xref:System.Data.DataView.RowFilter%2A> プロパティは、データ連結アプリケーションでの使用に適しています。このアプリケーションでは、連結されたコントロールによってフィルター処理結果が表示されます。 <xref:System.Data.DataView.RowFilter%2A> プロパティを設定すると、データのインデックスが再構築され、アプリケーションのオーバーヘッドが増加してパフォーマンスの低下を招きます。 <xref:System.Data.DataView.Find%2A> メソッドと <xref:System.Data.DataView.FindRows%2A> メソッドでは、現在のインデックスが使用されます。このため、インデックスを再構築する必要はありません。 <xref:System.Data.DataView.Find%2A> または <xref:System.Data.DataView.FindRows%2A> を 1 回だけ呼び出す場合は、既存の <xref:System.Data.DataView> を使用するようにします。 <xref:System.Data.DataView.Find%2A> または <xref:System.Data.DataView.FindRows%2A> を複数回呼び出す場合は、新しい <xref:System.Data.DataView> を作成して検索対象の列のインデックスを再構築した後、<xref:System.Data.DataView.Find%2A> メソッドまたは <xref:System.Data.DataView.FindRows%2A> メソッドを呼び出します。 <xref:System.Data.DataView.Find%2A> および <xref:System.Data.DataView.FindRows%2A> メソッドの詳細については、「行と[DataView のパフォーマンス](dataview-performance.md)の[検出](./dataset-datatable-dataview/finding-rows.md)」を参照してください。  
  
## <a name="clearing-the-filter"></a>フィルターのクリア  
 <xref:System.Data.DataView> のフィルターは、<xref:System.Data.DataView.RowFilter%2A> プロパティを使用すると、フィルタリングが設定された後にクリアできます。 <xref:System.Data.DataView> のフィルターは、2 つの異なる方法でクリアできます。  
  
- <xref:System.Data.DataView.RowFilter%2A> プロパティを `null`に設定します。  
  
- <xref:System.Data.DataView.RowFilter%2A> プロパティを空の文字列に設定します。  
  
### <a name="example"></a>使用例  
 次の例では、クエリから <xref:System.Data.DataView> を作成した後、<xref:System.Data.DataView.RowFilter%2A> プロパティを `null` に設定してフィルターをクリアします。  
  
 [!code-csharp[DP DataView Samples#LDVClearRowFilter2](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvclearrowfilter2)]
 [!code-vb[DP DataView Samples#LDVClearRowFilter2](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvclearrowfilter2)]  
  
### <a name="example"></a>使用例  
 次の例では、テーブルから <xref:System.Data.DataView> を作成し、<xref:System.Data.DataView.RowFilter%2A> プロパティを設定します。その後、<xref:System.Data.DataView.RowFilter%2A> プロパティを空の文字列に設定してフィルターをクリアします。  
  
 [!code-csharp[DP DataView Samples#LDVClearRowFilter](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvclearrowfilter)]
 [!code-vb[DP DataView Samples#LDVClearRowFilter](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvclearrowfilter)]  
  
## <a name="see-also"></a>関連項目

- [データ バインディングと LINQ to DataSet](data-binding-and-linq-to-dataset.md)
- [DataView による並べ替え](sorting-with-dataview-linq-to-dataset.md)
