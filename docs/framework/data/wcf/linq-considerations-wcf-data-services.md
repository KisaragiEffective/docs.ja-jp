---
title: LINQ に関する留意点 (WCF Data Services)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, LINQ
- querying the data service [WCF Data Services]
- WCF Data Services, querying
ms.assetid: cc4ec9e9-348f-42a6-a78e-1cd40e370656
ms.openlocfilehash: f8ec53323abec7077c69f50fe522338228ceddbb
ms.sourcegitcommit: ed3f926b6cdd372037bbcc214dc8f08a70366390
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2020
ms.locfileid: "76116637"
---
# <a name="linq-considerations-wcf-data-services"></a>LINQ に関する留意点 (WCF Data Services)
このトピックでは、WCF Data Services クライアントを使用しているときに LINQ クエリを作成および実行する方法について説明します。また、LINQ を使用して Open Data Protocol (OData) を実装するデータサービスを照会する場合の制限事項についても説明します。 OData ベースのデータサービスに対するクエリの作成と実行の詳細については、「[データサービスのクエリ](querying-the-data-service-wcf-data-services.md)」を参照してください。  
  
## <a name="composing-linq-queries"></a>LINQ クエリの作成  
 LINQ を使用すると、<xref:System.Collections.Generic.IEnumerable%601> を実装するオブジェクトのコレクションに対するクエリを作成できます。 Visual Studio の **[サービス参照の追加]** ダイアログボックスと datasvcutil.exe ツールを使用して、OData サービスの表現を、<xref:System.Data.Services.Client.DataServiceContext>から継承するエンティティコンテナークラスとして、およびフィードで返されるエンティティを表すオブジェクトとして生成します。 これらのツールでは、サービスによってフィードとして公開されるコレクションに対応するエンティティ コンテナー クラスのプロパティも生成されます。 データ サービスをカプセル化するクラスのこれらのプロパティは、それぞれ <xref:System.Data.Services.Client.DataServiceQuery%601> を返します。 <xref:System.Data.Services.Client.DataServiceQuery%601> クラスは LINQ で定義された <xref:System.Linq.IQueryable%601> インターフェイスを実装するので、データ サービスによって公開されるフィードに対する LINQ クエリを作成できます。作成した LINQ クエリは、クライアント ライブラリにより、実行時にデータ サービスに送信されるクエリ要求 URI に変換されます。  
  
> [!IMPORTANT]
> LINQ 構文で表現できるクエリのセットは、OData データサービスで使用される URI 構文で有効になっているものよりも大きくなります。 クエリを対象データ サービスの URI にマップできない場合、<xref:System.NotSupportedException> が発生します。 詳細については、このトピックの「サポートされて[いない LINQ メソッド](linq-considerations-wcf-data-services.md#unsupportedMethods)」を参照してください。  
  
 次の例の LINQ クエリは、輸送費が 30 ドルを超える `Orders` を取得し、結果を出荷日の新しい順に並べ替えます。  
  
[!code-csharp[Astoria Northwind Client#AddQueryOptionsLinqSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionslinqspecific)]      
[!code-vb[Astoria Northwind Client#AddQueryOptionsLinqSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionslinqspecific)]    
  
 この LINQ クエリは、Northwind ベースの[クイックスタート](quickstart-wcf-data-services.md)データサービスに対して実行される次のクエリ URI に変換されます。  
  
```http
http://localhost:12345/Northwind.svc/Orders?Orderby=ShippedDate&?filter=Freight gt 30  
```  
  
 Linq の一般的な情報については、「[統合言語クエリ (linq C# )](../../../csharp/programming-guide/concepts/linq/index.md) 」または「[統合言語クエリ (LINQ) (Visual Basic)](../../../visual-basic/programming-guide/concepts/linq/index.md)」を参照してください。  
  
 LINQ を使用してクエリを作成する際には、前の例のような言語固有の宣言型のクエリ構文と、標準クエリ演算子と呼ばれる一連のクエリ メソッドの両方を使用できます。 したがって、次の例のように、前の例と同等のクエリをメソッド ベースの構文のみを使用して作成することもできます。  
  
[!code-csharp[Astoria Northwind Client#AddQueryOptionsLinqExpressionSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionslinqexpressionspecific)]      
[!code-vb[Astoria Northwind Client#AddQueryOptionsLinqExpressionSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionslinqexpressionspecific)]    
  
 WCF Data Services クライアントは、両方の種類のクエリをクエリ URI に変換できます。また、クエリ式にクエリメソッドを追加して LINQ クエリを拡張することもできます。 クエリ式または <xref:System.Data.Services.Client.DataServiceQuery%601> にメソッド構文を追加して LINQ クエリを作成した場合は、メソッドが呼び出される順序で操作がクエリ URI に追加されます。 これは、<xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> メソッドを呼び出して各クエリ オプションをクエリ URI に追加するのと同じです。  
  
## <a name="executing-linq-queries"></a>LINQ クエリの実行  
 クエリに特定の LINQ クエリ メソッド (<xref:System.Linq.Enumerable.First%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%29>、<xref:System.Linq.Enumerable.Single%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%29> など) を追加するとクエリが実行されます。 クエリは、結果が暗黙的に列挙される場合 (`foreach` ループの間など) や、クエリが `List` コレクションに割り当てられている場合にも実行されます。 詳細については、「[データサービスのクエリ](querying-the-data-service-wcf-data-services.md)」を参照してください。  
  
 クライアントによる LINQ クエリの実行は 2 つの部分に分かれています。 可能な限り、クエリ内の LINQ 式は最初にクライアントで評価されます。その後、URI ベースのクエリが生成され、データ サービスに送信されて、サービス内のデータに対して評価されます。 詳細については、「[データサービスのクエリ](querying-the-data-service-wcf-data-services.md)」の「[クライアントとサーバーの実行](querying-the-data-service-wcf-data-services.md#executingQueries)」を参照してください。  
  
 OData 準拠のクエリ URI で LINQ クエリを変換できない場合、実行が試行されると例外が発生します。 詳細については、「[データサービスのクエリ](querying-the-data-service-wcf-data-services.md)」を参照してください。  
  
## <a name="linq-query-examples"></a>LINQ クエリの例  
 次のセクションの例では、OData サービスに対して実行できる LINQ クエリの種類について説明します。  
  
<a name="filtering"></a>   
### <a name="filtering"></a>フィルター処理  
 このセクションの LINQ クエリは、サービスによって返されるフィード内のデータをフィルター処理します。  
  
 以下の各例は、返された `Orders` エンティティをフィルター処理して、輸送費が 30 ドルを超える注文のみが返されるようにする同等のクエリを示しています。  
  
- LINQ クエリ構文を使用する場合:  
  
[!code-csharp[Astoria Northwind Client#LinqWhereClauseSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqwhereclausespecific)]      
[!code-vb[Astoria Northwind Client#LinqWhereClauseSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqwhereclausespecific)]     
  
- LINQ クエリ メソッドを使用する場合:  
  
[!code-csharp[Astoria Northwind Client#LinqWhereMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqwheremethodspecific)]      
[!code-vb[Astoria Northwind Client#LinqWhereMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqwheremethodspecific)]       
  
- URI クエリ文字列オプション `$filter`:  
  
[!code-csharp[Astoria Northwind Client#ExplicitQueryWhereMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#explicitquerywheremethodspecific)]      
[!code-vb[Astoria Northwind Client#ExplicitQueryWhereMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#explicitquerywheremethodspecific)]       
  
 上の例はすべて、`http://localhost:12345/northwind.svc/Orders()?$filter=Freight gt 30M` というクエリ URI に変換されます。  
  
<a name="sorting"></a>   
### <a name="sorting"></a>並べ替え  
 以下の各例は、返されたデータを会社名と郵便番号の降順で並べ替える同等のクエリを示しています。  
  
- LINQ クエリ構文を使用する場合:  
  
[!code-csharp[Astoria Northwind Client#LinqOrderByClauseSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqorderbyclausespecific)]      
[!code-vb[Astoria Northwind Client#LinqOrderByClauseSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqorderbyclausespecific)]        
  
- LINQ クエリ メソッドを使用する場合:  
  
[!code-csharp[Astoria Northwind Client#LinqOrderByMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqorderbymethodspecific)]      
[!code-vb[Astoria Northwind Client#LinqOrderByMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqorderbymethodspecific)]        
  
- URI クエリ文字列オプション `$orderby`:  
  
[!code-csharp[Astoria Northwind Client#ExplicitQueryOrderByMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#explicitqueryorderbymethodspecific)]      
[!code-vb[Astoria Northwind Client#ExplicitQueryOrderByMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#explicitqueryorderbymethodspecific)]         
  
 上の例はすべて、`http://localhost:12345/northwind.svc/Customers()?$orderby=CompanyName,PostalCode desc` というクエリ URI に変換されます。  
  
<a name="projection"></a>   
### <a name="projection"></a>射影  
 以下の各例は、返されたデータをより範囲の狭い `CustomerAddress` 型に射影する同等のクエリを示しています。  
  
- LINQ クエリ構文を使用する場合:  
  
[!code-csharp[Astoria Northwind Client#LinqSelectClauseSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqselectclausespecific)]      
[!code-vb[Astoria Northwind Client#LinqSelectClauseSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqselectclausespecific)]         
  
- LINQ クエリ メソッドを使用する場合:  
  
[!code-csharp[Astoria Northwind Client#LinqSelectMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqselectmethodspecific)]      
[!code-vb[Astoria Northwind Client#LinqSelectMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqselectmethodspecific)]         

> [!NOTE]
> `$select` メソッドを使用してクエリ URI に <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> クエリ オプションを追加することはできません。 LINQ の <xref:System.Linq.Enumerable.Select%2A> メソッドを使用して、クライアントによって要求 URI に `$select` クエリ オプションが生成されるようにすることをお勧めします。  
  
 上の例はいずれも、`"http://localhost:12345/northwind.svc/Customers()?$filter=Country eq 'GerGerm'&$select=CustomerID,Address,City,Region,PostalCode,Country"` というクエリ URI に変換されます。  
  
<a name="paging"></a>   
### <a name="client-paging"></a>クライアントのページング  
 以下の各例は、25 件の注文を含む並べ替え済みの注文エンティティのページを、最初の 50 件の注文をスキップして要求する同等のクエリを示しています。  
  
- LINQ クエリにクエリ メソッドを適用する場合:  
  
[!code-csharp[Astoria Northwind Client#LinqSkipTakeMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqskiptakemethodspecific)]      
[!code-vb[Astoria Northwind Client#LinqSkipTakeMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqskiptakemethodspecific)]     
  
- URI クエリ文字列オプション `$skip` および `$top`:  
  
[!code-csharp[Astoria Northwind Client#ExplicitQuerySkipTakeMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#explicitqueryskiptakemethodspecific)]      
[!code-vb[Astoria Northwind Client#ExplicitQuerySkipTakeMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#explicitqueryskiptakemethodspecific)]     
  
 上の例はいずれも、`http://localhost:12345/northwind.svc/Orders()?$orderby=OrderDate desc&$skip=50&$top=25` というクエリ URI に変換されます。  
  
<a name="expand"></a>   
### <a name="expand"></a>Expand  
 OData データサービスに対してクエリを実行する場合は、クエリの対象となるエンティティに関連するエンティティを、返されたフィードに含めるように要求できます。 そのためには、LINQ クエリの対象のエンティティ セットの <xref:System.Data.Services.Client.DataServiceQuery%601.Expand%2A> で <xref:System.Data.Services.Client.DataServiceQuery%601> メソッドを呼び出して、関連するエンティティ セットの名前を `path` パラメーターとして渡します。 詳細については、「[遅延コンテンツの読み込み](loading-deferred-content-wcf-data-services.md)」を参照してください。  
  
 以下の各例は、クエリで <xref:System.Data.Services.Client.DataServiceQuery%601.Expand%2A> メソッドを使用する同等の方法を示しています。  
  
- LINQ クエリ構文の場合:  
  
[!code-csharp[Astoria Northwind Client#LinqQueryExpandSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqqueryexpandspecific)]      
[!code-vb[Astoria Northwind Client#LinqQueryExpandSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqqueryexpandspecific)]  
  
- LINQ クエリ メソッドの場合:  

[!code-csharp[Astoria Northwind Client#LinqQueryExpandMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqqueryexpandmethodspecific)]       
[!code-vb[Astoria Northwind Client#LinqQueryExpandMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqqueryexpandmethodspecific)]       

 上の例はいずれも、`http://localhost:12345/northwind.svc/Orders()?$filter=CustomerID eq 'ALFKI'&$expand=Order_Details` というクエリ URI に変換されます。  
  
<a name="unsupportedMethods"></a>   
## <a name="unsupported-linq-methods"></a>サポートされていない LINQ メソッド  
 次の表に、LINQ メソッドのクラスはサポートされていないため、OData サービスに対して実行されるクエリには含めることができません。  
  
|演算の種類|サポートされていないメソッド|  
|--------------------|------------------------|  
|集合演算子|すべての集合演算子は <xref:System.Data.Services.Client.DataServiceQuery%601> に対してサポートされていません。以下に例を示します。<br /><br /> -   <xref:System.Linq.Enumerable.All%2A><br />-   <xref:System.Linq.Enumerable.Any%2A><br />-   <xref:System.Linq.Enumerable.Concat%2A><br />-   <xref:System.Linq.Enumerable.DefaultIfEmpty%2A><br />-   <xref:System.Linq.Enumerable.Distinct%2A><br />-   <xref:System.Linq.Enumerable.Except%2A><br />-   <xref:System.Linq.Enumerable.Intersect%2A><br />-   <xref:System.Linq.Enumerable.Union%2A><br />-   <xref:System.Linq.Enumerable.Zip%2A>|  
|順序付け演算子|<xref:System.Collections.Generic.IComparer%601> を必要とする以下の順序付け演算子は <xref:System.Data.Services.Client.DataServiceQuery%601> に対してサポートされていません。<br /><br /> -   <xref:System.Linq.Enumerable.OrderBy%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%2CSystem.Collections.Generic.IComparer%7B%60%601%7D%29><br />-   <xref:System.Linq.Enumerable.OrderByDescending%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%2CSystem.Collections.Generic.IComparer%7B%60%601%7D%29><br />-   <xref:System.Linq.Enumerable.ThenBy%60%602%28System.Linq.IOrderedEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%2CSystem.Collections.Generic.IComparer%7B%60%601%7D%29><br />-   <xref:System.Linq.Enumerable.ThenByDescending%60%602%28System.Linq.IOrderedEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%2CSystem.Collections.Generic.IComparer%7B%60%601%7D%29>|  
|プロジェクション演算子とフィルター演算子|位置指定引数を受け取る以下のプロジェクション演算子とフィルター演算子は <xref:System.Data.Services.Client.DataServiceQuery%601> に対してサポートされていません。<br /><br /> -   <xref:System.Linq.Enumerable.Join%60%604%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Collections.Generic.IEnumerable%7B%60%601%7D%2CSystem.Func%7B%60%600%2C%60%602%7D%2CSystem.Func%7B%60%601%2C%60%602%7D%2CSystem.Func%7B%60%600%2C%60%601%2C%60%603%7D%2CSystem.Collections.Generic.IEqualityComparer%7B%60%602%7D%29><br />-   <xref:System.Linq.Enumerable.Select%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2CSystem.Int32%2C%60%601%7D%29><br />-   <xref:System.Linq.Enumerable.SelectMany%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2CSystem.Collections.Generic.IEnumerable%7B%60%601%7D%7D%29><br />-   <xref:System.Linq.Enumerable.SelectMany%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2CSystem.Int32%2CSystem.Collections.Generic.IEnumerable%7B%60%601%7D%7D%29><br />-   <xref:System.Linq.Enumerable.SelectMany%60%603%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2CSystem.Collections.Generic.IEnumerable%7B%60%601%7D%7D%2CSystem.Func%7B%60%600%2C%60%601%2C%60%602%7D%29><br />-   <xref:System.Linq.Enumerable.SelectMany%60%603%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2CSystem.Int32%2CSystem.Collections.Generic.IEnumerable%7B%60%601%7D%7D%2CSystem.Func%7B%60%600%2C%60%601%2C%60%602%7D%29><br />-   <xref:System.Linq.Enumerable.Where%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2CSystem.Int32%2CSystem.Boolean%7D%29>|  
|グループ化演算子|すべてのグループ化演算子は <xref:System.Data.Services.Client.DataServiceQuery%601> に対してサポートされていません。以下に例を示します。<br /><br /> -   <xref:System.Linq.Enumerable.GroupBy%2A><br />-   <xref:System.Linq.Enumerable.GroupJoin%2A><br /><br /> グループ化の操作はクライアント側で実行する必要があります。|  
|集計演算子|すべての集計演算子は <xref:System.Data.Services.Client.DataServiceQuery%601> に対してサポートされていません。以下に例を示します。<br /><br /> -   <xref:System.Linq.Enumerable.Aggregate%2A><br />-   <xref:System.Linq.Enumerable.Average%2A><br />-   <xref:System.Linq.Enumerable.Count%2A><br />-   <xref:System.Linq.Enumerable.LongCount%2A><br />-   <xref:System.Linq.Enumerable.Max%2A><br />-   <xref:System.Linq.Enumerable.Min%2A><br />-   <xref:System.Linq.Enumerable.Sum%2A><br /><br /> 集計操作は、クライアント側で実行するか、サービス操作でカプセル化する必要があります。|  
|ページング演算子|以下のページング演算子は <xref:System.Data.Services.Client.DataServiceQuery%601> に対してサポートされていません。<br /><br /> -   <xref:System.Linq.Enumerable.ElementAt%2A><br />-   <xref:System.Linq.Enumerable.Last%2A><br />-   <xref:System.Linq.Enumerable.LastOrDefault%2A><br />-   <xref:System.Linq.Enumerable.SkipWhile%2A><br />-   <xref:System.Linq.Enumerable.TakeWhile%2A><br/><br/>**注:** 空のシーケンスに対して実行されるページング演算子は、null を返します。|  
|その他の演算子|<xref:System.Data.Services.Client.DataServiceQuery%601>に対しては、次の演算子もサポートされていません。<br /><br /> - <xref:System.Linq.Enumerable.Empty%2A><br />- <xref:System.Linq.Enumerable.Range%2A><br />- <xref:System.Linq.Enumerable.Repeat%2A><br />- <xref:System.Linq.Enumerable.ToDictionary%2A><br />- <xref:System.Linq.Enumerable.ToLookup%2A>|  
  
<a name="supportedExpressions"></a>   
## <a name="supported-expression-functions"></a>サポートされている式の関数  
 次の共通言語ランタイム (CLR) のメソッドとプロパティは、OData サービスへの要求 URI に含めるためにクエリ式で変換できるため、サポートされています。  
  
|<xref:System.String> メンバー|サポートされている OData 関数|  
|-----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|  
|<xref:System.String.Concat%28System.String%2CSystem.String%29>|`string concat(string p0, string p1)`|  
|<xref:System.String.Contains%28System.String%29>|`bool substringof(string p0, string p1)`|  
|<xref:System.String.EndsWith%28System.String%29>|`bool endswith(string p0, string p1)`|  
|<xref:System.String.IndexOf%28System.String%2CSystem.Int32%29>|`int indexof(string p0, string p1)`|  
|<xref:System.String.Length>|`int length(string p0)`|  
|<xref:System.String.Replace%28System.String%2CSystem.String%29>|`string replace(string p0, string find, string replace)`|  
|<xref:System.String.Substring%28System.Int32%29>|`string substring(string p0, int pos)`|  
|<xref:System.String.Substring%28System.Int32%2CSystem.Int32%29>|`string substring(string p0, int pos, int length)`|  
|<xref:System.String.ToLower>|`string tolower(string p0)`|  
|<xref:System.String.ToUpper>|`string toupper(string p0)`|  
|<xref:System.String.Trim>|`string trim(string p0)`|  
  
|<xref:System.DateTime> メンバー<sup>1</sup>|サポートされている OData 関数|  
|-------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|  
|<xref:System.DateTime.Day>|`int day(DateTime p0)`|  
|<xref:System.DateTime.Hour>|`int hour(DateTime p0)`|  
|<xref:System.DateTime.Minute>|`int minute(DateTime p0)`|  
|<xref:System.DateTime.Month>|`int month(DateTime p0)`|  
|<xref:System.DateTime.Second>|`int second(DateTime p0)`|  
|<xref:System.DateTime.Year>|`int year(DateTime p0)`|  
  
 <sup>1</sup><xref:Microsoft.VisualBasic.DateAndTime?displayProperty=nameWithType> の同じ日付と時刻のプロパティと Visual Basic の <xref:Microsoft.VisualBasic.DateAndTime.DatePart%2A> メソッドもサポートされています。  
  
|<xref:System.Math> メンバー|サポートされている OData 関数|  
|---------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|  
|<xref:System.Math.Ceiling%28System.Decimal%29>|`decimal ceiling(decimal p0)`|  
|<xref:System.Math.Ceiling%28System.Double%29>|`double ceiling(double p0)`|  
|<xref:System.Math.Floor%28System.Decimal%29>|`decimal floor(decimal p0)`|  
|<xref:System.Math.Floor%28System.Double%29>|`double floor(double p0)`|  
|<xref:System.Math.Round%28System.Decimal%29>|`decimal round(decimal p0)`|  
|<xref:System.Math.Round%28System.Double%29>|`double round(double p0)`|  
  
|<xref:System.Linq.Expressions.Expression> メンバー|サポートされている OData 関数|  
|---------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|  
|<xref:System.Linq.Expressions.Expression.TypeIs%28System.Linq.Expressions.Expression%2CSystem.Type%29>|`bool isof(type p0)`|  
  
 クライアント側でその他の CLR 関数を評価できる場合もあります。 クライアント側で評価することも、サーバー側で評価するために有効な要求 URI に変換することもできない式に対しては、<xref:System.NotSupportedException> が発生します。  
  
## <a name="see-also"></a>関連項目

- [データ サービスに対するクエリ](querying-the-data-service-wcf-data-services.md)
- [クエリ射影](query-projections-wcf-data-services.md)
- [オブジェクトの具体化](object-materialization-wcf-data-services.md)
- [OData: URI 規則](https://www.odata.org/documentation/odata-version-2-0/uri-conventions/)
