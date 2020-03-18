---
title: 遅延コンテンツの読み込み (WCF Data Services)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, client library
- WCF Data Services, deferred content
- WCF Data Services, loading data
ms.assetid: 32f9b588-c832-44c4-a7e0-fcce635df59a
ms.openlocfilehash: 811118755c4688bd0ea8cb9ba37b2101ab6c52cf
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74568953"
---
# <a name="loading-deferred-content-wcf-data-services"></a>遅延コンテンツの読み込み (WCF Data Services)
既定では、WCF Data Services によって、クエリが返すデータの量が制限されます。 その一方で、関連エンティティ、ページングされた応答データ、およびバイナリ データ ストリームを含む追加データをデータ サービスから必要に応じて明示的に読み込むことができます。 このトピックでは、このような遅延コンテンツをアプリケーションに読み込む方法について説明します。  
  
## <a name="related-entities"></a>関連エンティティ  
 クエリを実行すると、アドレス指定したエンティティ セット内のエンティティだけが返されます。 たとえば、Northwind データ サービスに対するクエリが `Customers` エンティティを返す場合、`Orders` と `Customers` の間にリレーションシップがあっても、既定では関連 `Orders` エンティティは返されません。 また、データ サービスでページングが有効になった場合は、以降のデータ ページをサービスから明示的に読み込む必要があります。 関連エンティティを読み込むには、2 つの方法があります。  
  
- **一括読み込み**: `$expand` クエリオプションを使用すると、クエリが要求したエンティティセットへのアソシエーションによって関連付けられたエンティティを返すように要求できます。 <xref:System.Data.Services.Client.DataServiceQuery%601.Expand%2A> に対して <xref:System.Data.Services.Client.DataServiceQuery%601> メソッドを使用して、データ サービスに送信されるクエリに `$expand` オプションを追加します。 次の例に示すように、エンティティ セットをコンマで区切ることによって関連する複数のエンティティ セットを要求できます。 クエリによって要求されたすべてのエンティティは、1 つの応答で返されます。 次の例では、`Order_Details` および `Customers` が `Orders` エンティティ セットと一緒に返されます。  
  
     [!code-csharp[Astoria Northwind Client#ExpandOrderDetailsSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#expandorderdetailsspecific)]
     [!code-vb[Astoria Northwind Client#ExpandOrderDetailsSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#expandorderdetailsspecific)]  
  
     WCF Data Services、`$expand` クエリオプションを使用して1つのクエリに含めることができるエンティティセットの数を12に制限します。  
  
- **明示的な読み込み**: <xref:System.Data.Services.Client.DataServiceContext> インスタンスで <xref:System.Data.Services.Client.DataServiceContext.LoadProperty%2A> メソッドを呼び出して、関連エンティティを明示的に読み込むことができます。 <xref:System.Data.Services.Client.DataServiceContext.LoadProperty%2A> メソッドへの各呼び出しによって、データ サービスへの個別の要求が作成されます。 次の例では、`Order_Details` エンティティの `Orders` を明示的に読み込みます。  
  
     [!code-csharp[Astoria Northwind Client#LoadRelatedOrderDetailsSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#loadrelatedorderdetailsspecific)]
     [!code-vb[Astoria Northwind Client#LoadRelatedOrderDetailsSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#loadrelatedorderdetailsspecific)]  
  
 どちらのオプションを使用するかを検討する場合、データ サービスへの要求の数と 1 つの応答で返されるデータの量のバランスを考慮してください。 アプリケーションが関連オブジェクトを必要とし、それらのオブジェクトを明示的に取得するための追加要求による待機時間を回避する場合は、一括読み込みを使用します。 しかし、アプリケーションが特定の関連エンティティ インスタンスのデータしか必要としない場合は、<xref:System.Data.Services.Client.DataServiceContext.LoadProperty%2A> メソッドを呼び出して、これらのエンティティを明示的に読み込むことを検討することをお勧めします。 詳細については、「[方法: 関連エンティティを読み込む](how-to-load-related-entities-wcf-data-services.md)」を参照してください。  
  
## <a name="paged-content"></a>ページングされたコンテンツ  
 データ サービスでページングが有効化されている場合、データ サービスが返すフィード内のエントリの数はデータ サービスの構成によって制限されます。 ページ制限は、各エンティティ セットに対して個別に設定できます。 詳細については、「[データサービスの構成](configuring-the-data-service-wcf-data-services.md)」を参照してください。 ページングが有効である場合、フィードの最終的なエントリには、データの次のページへのリンクが含まれます。 このリンクは、<xref:System.Data.Services.Client.DataServiceQueryContinuation%601> オブジェクトに含まれます。 データの次のページへの URI は、<xref:System.Data.Services.Client.QueryOperationResponse%601.GetContinuation%2A> が実行されたときに返される <xref:System.Data.Services.Client.QueryOperationResponse%601> で <xref:System.Data.Services.Client.DataServiceQuery%601> メソッドを呼び出すことによって取得します。 返された <xref:System.Data.Services.Client.DataServiceQueryContinuation%601> オブジェクトが使用されて、結果の次のページが読み込まれます。 クエリ結果は、<xref:System.Data.Services.Client.QueryOperationResponse%601.GetContinuation%2A> メソッドを呼び出す前に列挙する必要があります。 最初に `do…while` ループを使用してクエリ結果を列挙した後に、次の `non-null` リンク値をチェックすることをお勧めします。 <xref:System.Data.Services.Client.QueryOperationResponse%601.GetContinuation%2A> メソッドが `null` (Visual Basic の場合は `Nothing`) を返す場合、元のクエリに追加の結果ページはありません。 次の例は、Northwind サンプル データ サービスからページングされた顧客データを読み込む `do…while` ループを示します。  
  
 [!code-csharp[Astoria Northwind Client#LoadNextLink](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#loadnextlink)]
 [!code-vb[Astoria Northwind Client#LoadNextLink](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#loadnextlink)]  
  
 要求されたエンティティ セットと一緒に関連エンティティを 1 つの応答で返すようクエリが要求すると、ページング制限は、応答と共にインラインで含まれるネストされたフィードに影響することがあります。 たとえば、Northwind サンプル データ サービスで `Customers` エンティティ セットに対してページング制限が設定されている場合、次の Northwind サンプル データ サービスを定義する Northwind.svc.cs ファイルの例に示すように、独立したページング制限を関連 `Orders` エンティティ セットに設定することもできます。  
  
 [!code-csharp[Astoria Northwind Service#DataServiceConfigPaging](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_service/cs/northwind.svc.cs#dataserviceconfigpaging)]
 [!code-vb[Astoria Northwind Service#DataServiceConfigPaging](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_service/vb/northwind.svc.vb#dataserviceconfigpaging)]  
  
 この場合、最上位レベルの `Customers` エンティティ フィードとネストされた `Orders` エンティティ フィードの両方にページングを実装する必要があります。 次の例は、選択した `while` エンティティに関連する `Orders` エンティティのページを読み込むために使用する `Customers` ループを示します。  
  
 [!code-csharp[Astoria Northwind Client#LoadNextOrdersLink](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#loadnextorderslink)]
 [!code-vb[Astoria Northwind Client#LoadNextOrdersLink](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#loadnextorderslink)]  
  
 詳細については、「[方法: ページング](how-to-load-paged-results-wcf-data-services.md)された結果を読み込む」を参照してください。  
  
## <a name="binary-data-streams"></a>バイナリ データ ストリーム  
 WCF Data Services を使用すると、バイナリラージオブジェクト (BLOB) データにデータストリームとしてアクセスできます。 ストリーミングは必要になるまでバイナリ データの読み込みを待機し、クライアントは、このデータを効率的に処理できます。 この機能を活用するには、データ サービスは <xref:System.Data.Services.Providers.IDataServiceStreamProvider> プロバイダーを実装する必要があります。 詳細については、「 [Streaming Provider](streaming-provider-wcf-data-services.md)」を参照してください。 ストリーミングが有効である場合、エンティティ型は関連バイナリ データなしで返されます。 この場合、サービスからのバイナリデータのデータストリームにアクセスするには、<xref:System.Data.Services.Client.DataServiceContext> クラスの <xref:System.Data.Services.Client.DataServiceContext.GetReadStream%2A> メソッドを使用する必要があります。 同様に、<xref:System.Data.Services.Client.DataServiceContext.SetSaveStream%2A> メソッドを使用して、エンティティのバイナリ データをストリームとして追加または変更します。 詳細については、「[バイナリデータの操作](working-with-binary-data-wcf-data-services.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [WCF Data Services クライアント ライブラリ](wcf-data-services-client-library.md)
- [データ サービスに対するクエリ](querying-the-data-service-wcf-data-services.md)
