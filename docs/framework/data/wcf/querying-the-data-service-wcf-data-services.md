---
title: データ サービスに対するクエリ (WCF Data Services)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, client library
- WCF Data Services, querying
- WCF Data Services, accessing data
ms.assetid: 823e9444-27aa-4f1f-be8e-0486d67f54c0
ms.openlocfilehash: 99fe377e8fff193c4f8bb566946b95c61c1b3693
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74568881"
---
# <a name="querying-the-data-service-wcf-data-services"></a>データ サービスに対するクエリ (WCF Data Services)

WCF Data Services クライアントライブラリを使用すると、統合言語クエリ (LINQ) を含め、使い慣れた .NET Framework プログラミングパターンを使用して、データサービスに対するクエリを実行できます。 このクライアント ライブラリは、クライアント上で <xref:System.Data.Services.Client.DataServiceQuery%601> クラスのインスタンスとして定義されたクエリを HTTP GET 要求メッセージに変換します。 ライブラリは応答メッセージを受信し、クライアントデータサービスクラスのインスタンスに変換します。 これらのクラスは、<xref:System.Data.Services.Client.DataServiceContext> が属する <xref:System.Data.Services.Client.DataServiceQuery%601> によって追跡されます。

## <a name="data-service-queries"></a>データ サービス クエリ

<xref:System.Data.Services.Client.DataServiceQuery%601> ジェネリック クラスは、0 個以上のエンティティ型インスタンスのコレクションを返すクエリを表します。 データ サービス クエリは、常に既存のデータ サービス コンテキストに属します。 このコンテキストにより、クエリの作成と実行に必要なサービス URI とメタデータ情報が維持されます。

**[サービス参照の追加]** ダイアログボックスを使用して、.NET Framework ベースのクライアントアプリケーションにデータサービスを追加すると、<xref:System.Data.Services.Client.DataServiceContext> クラスを継承するエンティティコンテナークラスが作成されます。 このクラスには、型指定された <xref:System.Data.Services.Client.DataServiceQuery%601> インスタンスを返すプロパティが含まれます。 データ サービスが公開するエンティティ セットごとに 1 つのプロパティがあります。 これらのプロパティを使用すると、型指定された <xref:System.Data.Services.Client.DataServiceQuery%601> のインスタンスを簡単に作成できます。

クエリは次のシナリオで実行されます。

- 次のように結果が暗黙的に列挙される場合

  - <xref:System.Data.Services.Client.DataServiceContext> (C#) ループや `foreach` (Visual Basic) ループなどで、エンティティ セットを表す `For Each` のプロパティが列挙されているとき

  - `List` コレクションにクエリが割り当てられているとき

- <xref:System.Data.Services.Client.DataServiceQuery%601.Execute%2A> メソッドまたは <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> メソッドが明示的に呼び出されたとき

- <xref:System.Linq.Enumerable.First%2A> や <xref:System.Linq.Enumerable.Single%2A> などの LINQ クエリ実行演算子が呼び出されたとき。

次のクエリを実行すると、Northwind データ サービスのすべての `Customers` エンティティが返されます。

[!code-csharp[Astoria Northwind Client#GetAllCustomersSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#getallcustomersspecific)]
[!code-vb[Astoria Northwind Client#GetAllCustomersSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#getallcustomersspecific)]

詳細については、「[方法: データサービスクエリを実行する](how-to-execute-data-service-queries-wcf-data-services.md)」を参照してください。

WCF Data Services クライアントは、でC#*動的*な型を使用する場合など、遅延バインディングオブジェクトのクエリをサポートします。 パフォーマンス上の理由から、データ サービスに対しては常に厳密に型指定されたクエリを作成するようにしてください。 <xref:System.Tuple> 型と動的オブジェクトはクライアントでサポートされていません。

## <a name="linq-queries"></a>LINQ クエリ

<xref:System.Data.Services.Client.DataServiceQuery%601> クラスは LINQ で定義された <xref:System.Linq.IQueryable%601> インターフェイスを実装するため、WCF Data Services クライアントライブラリは、エンティティセットデータに対する LINQ クエリを、データサービスリソースに対して評価されるクエリ式を表す URI に変換できます。 次の例は、前述の <xref:System.Data.Services.Client.DataServiceQuery%601> と同じ LINQ クエリです。ここでは、輸送費が 30 ドルを超える `Orders` を取得し、その結果を輸送費順に並べ替えます。

[!code-csharp[Astoria Northwind Client#AddQueryOptionsLinqSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionslinqspecific)]
[!code-vb[Astoria Northwind Client#AddQueryOptionsLinqSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionslinqspecific)]

この LINQ クエリは、Northwind ベースの[クイックスタート](quickstart-wcf-data-services.md)データサービスに対して実行される次のクエリ URI に変換されます。

```http
http://localhost:12345/Northwind.svc/Orders?Orderby=ShippedDate&?filter=Freight gt 30
```

> [!NOTE]
> LINQ 構文で表現できるクエリのセットは、データ サービスによって使用される REST (Representational State Transfer) ベースの URI 構文で有効なクエリのセットよりも範囲が広くなります。 クエリを対象データ サービスの URI にマップできない場合、<xref:System.NotSupportedException> が発生します。

詳細については、「 [LINQ の考慮事項](linq-considerations-wcf-data-services.md)」を参照してください。

## <a name="adding-query-options"></a>クエリ オプションの追加

データサービスクエリは、WCF Data Servicess が提供するすべてのクエリオプションをサポートしています。 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> メソッドを呼び出して、クエリ オプションを <xref:System.Data.Services.Client.DataServiceQuery%601> インスタンスに追加します。 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> は、新しい <xref:System.Data.Services.Client.DataServiceQuery%601> インスタンスを返します。このインスタンスは元のクエリと同等ですが、新しいクエリ オプション セットを含みます。 次のクエリを実行すると、`Orders` 値でフィルターされ、`Freight` によって降順に並べ替えられた `OrderID` が返されます。

[!code-csharp[Astoria Northwind Client#AddQueryOptionsSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionsspecific)]
[!code-vb[Astoria Northwind Client#AddQueryOptionsSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionsspecific)]

`$orderby` クエリ オプションを使用すると、単一のプロパティに基づくクエリの並べ替えとフィルターの両方が可能です。次の例では、フィルターし、返された `Orders` オブジェクトを `Freight` プロパティの値に基づき並べ替えます。

[!code-csharp[Astoria Northwind Client#OrderWithFilter](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#orderwithfilter)]
[!code-vb[Astoria Northwind Client#OrderWithFilter](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#orderwithfilter)]

<xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> メソッドを連続して呼び出すと、複雑なクエリ式を作成できます。 詳細については、「[方法: データサービスクエリにクエリオプションを追加](how-to-add-query-options-to-a-data-service-query-wcf-data-services.md)する」を参照してください。

クエリ オプションを使用して、LINQ クエリの構文要素を表すことができます。 詳細については、「 [LINQ の考慮事項](linq-considerations-wcf-data-services.md)」を参照してください。

> [!NOTE]
> `$select` メソッドを使用してクエリ URI に <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> クエリ オプションを追加することはできません。 LINQ の <xref:System.Linq.Enumerable.Select%2A> メソッドを使用して、クライアントによって要求 URI に `$select` クエリ オプションが生成されるようにすることをお勧めします。

<a name="executingQueries"></a>

## <a name="client-versus-server-execution"></a>クライアントでの実行とサーバーでの実行

クライアントによるクエリの実行は 2 つの部分に分かれています。 可能な限り、クエリ内の式は最初にクライアントで評価されます。その後、URI ベースのクエリが生成され、データ サービスに送信されて、サービス内のデータに対して評価されます。 次の LINQ クエリについて考えてみましょう。

[!code-csharp[Astoria Northwind Client#LinqQueryClientEvalSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqqueryclientevalspecific)]
[!code-vb[Astoria Northwind Client#LinqQueryClientEvalSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqqueryclientevalspecific)]

この例では、式 `(basePrice – (basePrice * discount))` はクライアントで評価されます。 そのため、データ サービスに送信される実際のクエリ URI (`http://localhost:12345/northwind.svc/Products()?$filter=(UnitPrice gt 90.00M) and substringof('bike',ProductName)`) のフィルター句に計算済みの 10 進値 `90` が含まれています。 部分文字列式を含むフィルター式のその他の部分は、データ サービスによって評価されます。 クライアントで評価される式は、共通言語ランタイム (CLR) セマンティクスに従いますが、データサービスに送信される式は、OData プロトコルのデータサービス実装に依存します。 また、このように別々に評価されることで予期しない結果が生じる場合があることにも注意する必要があります。たとえば、クライアントとサービスの両方で、異なるタイム ゾーンを使用して時間に基づく評価が実行される場合があります。

## <a name="query-responses"></a>クエリ応答

<xref:System.Data.Services.Client.DataServiceQuery%601> を実行すると、要求したエンティティ型の <xref:System.Collections.Generic.IEnumerable%601> が返されます。 このクエリ結果は、次の例のように <xref:System.Data.Services.Client.QueryOperationResponse%601> オブジェクトにキャストできます。

[!code-csharp[Astoria Northwind Client#GetResponseSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#getresponsespecific)]
[!code-vb[Astoria Northwind Client#GetResponseSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#getresponsespecific)]

データ サービスのエンティティを表すエンティティ型インスタンスは、オブジェクトの具体化というプロセスによってクライアントで作成されます。 詳細については、「[オブジェクトの具体化](object-materialization-wcf-data-services.md)」を参照してください。 <xref:System.Data.Services.Client.QueryOperationResponse%601> オブジェクトは、<xref:System.Collections.Generic.IEnumerable%601> を実装してクエリ結果へのアクセスを提供します。

さらに、<xref:System.Data.Services.Client.QueryOperationResponse%601> には、クエリ結果に関する追加情報へのアクセスを可能にする次のメンバーが含まれます。

- <xref:System.Data.Services.Client.OperationResponse.Error%2A> - 発生すると、操作によってエラーがスローされます。

- <xref:System.Data.Services.Client.OperationResponse.Headers%2A> - クエリ応答に関連する HTTP 応答ヘッダーのコレクションが含まれます。

- <xref:System.Data.Services.Client.QueryOperationResponse.Query%2A> - <xref:System.Data.Services.Client.DataServiceQuery%601> を生成した元の <xref:System.Data.Services.Client.QueryOperationResponse%601> を取得します。

- <xref:System.Data.Services.Client.OperationResponse.StatusCode%2A> - クエリ応答の HTTP 応答コードを取得します。

- <xref:System.Data.Services.Client.QueryOperationResponse%601.TotalCount%2A> - <xref:System.Data.Services.Client.DataServiceQuery%601.IncludeTotalCount%2A> で <xref:System.Data.Services.Client.DataServiceQuery%601> メソッドが呼び出されるとエンティティ セット内のエンティティの合計数を取得します。

- <xref:System.Data.Services.Client.QueryOperationResponse.GetContinuation%2A> - 結果の次のページの URI を含む <xref:System.Data.Services.Client.DataServiceQueryContinuation> オブジェクトを返します。

既定では、WCF Data Services は、クエリ URI によって明示的に選択されたデータのみを返します。 これにより、必要に応じてデータ サービスから追加のデータを明示的に読み込むことができます。 データ サービスからデータを明示的に読み込むたびに要求がデータ サービスに送られます。 明示的に読み込むことができるデータには、関連エンティティ、ページングされた応答データ、バイナリ データ ストリームがあります。

> [!NOTE]
> データ サービスはページングされた応答を返す場合があるので、アプリケーションではページングされたデータ サービス応答を処理するプログラミング パターンを使用することをお勧めします。 詳細については、「[遅延コンテンツの読み込み](loading-deferred-content-wcf-data-services.md)」を参照してください。

エンティティの特定のプロパティのみが応答で返されるように指定することにより、クエリによって返されるデータの量を削減することもできます。 詳細については、「[クエリプロジェクション](query-projections-wcf-data-services.md)」を参照してください。

## <a name="getting-a-count-of-the-total-number-of-entities-in-the-set"></a>セット内のエンティティの合計数の取得

一部のシナリオでは、クエリによって返される数だけではなく、エンティティ セット内のエンティティの合計数を知っておくと役立ちます。 このセット内のエンティティの合計数がクエリ結果に含まれるように要求するには、<xref:System.Data.Services.Client.DataServiceQuery%601.IncludeTotalCount%2A> で <xref:System.Data.Services.Client.DataServiceQuery%601> メソッドを呼び出します。 この場合、返された <xref:System.Data.Services.Client.QueryOperationResponse%601.TotalCount%2A> の <xref:System.Data.Services.Client.QueryOperationResponse%601> プロパティが、セット内のエンティティの合計数を返します。

さらに、<xref:System.Int32> メソッドまたは <xref:System.Int64> メソッドを呼び出すことにより、それぞれ <xref:System.Linq.Enumerable.Count%2A> 値または <xref:System.Linq.Enumerable.LongCount%2A> 値として、セット内のエンティティの合計数のみを取得することもできます。 これらのメソッドを呼び出すと、<xref:System.Data.Services.Client.QueryOperationResponse%601> は返されず、カウント値のみが返されます。 詳細については、「[方法: クエリによって返されるエンティティの数を確認する](number-of-entities-returned-by-a-query-wcf.md)」を参照してください。

## <a name="in-this-section"></a>このセクションの内容

- [クエリ射影](query-projections-wcf-data-services.md)

- [オブジェクトの具体化](object-materialization-wcf-data-services.md)

- [LINQ に関する留意点](linq-considerations-wcf-data-services.md)

- [方法: データ サービス クエリを実行する](how-to-execute-data-service-queries-wcf-data-services.md)

- [方法: データ サービス クエリにクエリ オプションを追加する](how-to-add-query-options-to-a-data-service-query-wcf-data-services.md)

- [方法: クエリで返されたエンティティの数を確認する](number-of-entities-returned-by-a-query-wcf.md)

- [方法: データ サービス要求のクライアント資格情報の指定](specify-client-creds-for-a-data-service-request-wcf.md)

- [方法: クライアント要求のヘッダーを設定する](how-to-set-headers-in-the-client-request-wcf-data-services.md)

- [方法: クエリ結果を射影する](how-to-project-query-results-wcf-data-services.md)

## <a name="see-also"></a>参照

- [WCF Data Services クライアント ライブラリ](wcf-data-services-client-library.md)
