---
title: バッチ処理 (WCF Data Services)
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, client library
ms.assetid: 962a49d1-cc11-4b96-bc7d-071dd6607d6c
ms.openlocfilehash: f6e6356b8acc6b5abb217ecc4edd2c3cd185f71a
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75346158"
---
# <a name="batching-operations-wcf-data-services"></a>バッチ処理 (WCF Data Services)
Open Data Protocol (OData) は、OData ベースのサービスへの要求のバッチ処理をサポートしています。 詳細については、「 [OData: バッチ処理](https://www.odata.org/documentation/odata-version-2-0/batch-processing/)」を参照してください。 WCF Data Services では、<xref:System.Data.Services.Client.DataServiceContext>を使用する各操作 (クエリの実行や変更の保存など) によって、データサービスに個別の要求が送信されます。 操作セットの論理スコープを維持するために、操作バッチを明示的に定義する必要があります。 これにより、バッチ内のすべての操作が1つの HTTP 要求でデータサービスに送信されるようになり、サーバーは操作をアトミックに処理し、データサービスへのラウンドトリップの回数を減らすことができます。  
  
## <a name="batching-query-operations"></a>クエリ操作のバッチ処理  
 複数のクエリを 1 つのバッチで実行するには、バッチ内の各クエリを <xref:System.Data.Services.Client.DataServiceRequest%601> クラスの個別のインスタンスとして作成する必要があります。 この方法でクエリ要求を作成すると、クエリ自身が URI として定義されます。このクエリは、リソースのアドレス指定のルールに従います。 詳細については、「[データサービスリソースへのアクセス](accessing-data-service-resources-wcf-data-services.md)」を参照してください。 バッチ化されたクエリ要求は、クエリ要求オブジェクトを含む <xref:System.Data.Services.Client.DataServiceContext.ExecuteBatch%2A> メソッドが呼び出されたときにデータ サービスに送信されます。 このメソッドは、<xref:System.Data.Services.Client.DataServiceResponse> オブジェクトを返します。このオブジェクトは、バッチ内の個々の要求への応答を表す <xref:System.Data.Services.Client.QueryOperationResponse%601> オブジェクトのコレクションです。個々のオブジェクトには、クエリによって返されるオブジェクトのコレクションまたはエラー情報が含まれます。 バッチ内のいずれかのクエリ操作が失敗した場合、<xref:System.Data.Services.Client.QueryOperationResponse%601> オブジェクトで失敗した操作のエラー情報が返され、残りの操作は引き続き実行されます。 詳細については、「[方法: バッチ内でクエリを実行する](how-to-execute-queries-in-a-batch-wcf-data-services.md)」を参照してください。  
  
 バッチ クエリは、非同期で実行することもできます。 詳細については、「[非同期操作](asynchronous-operations-wcf-data-services.md)」を参照してください。  
  
## <a name="batching-the-savechanges-operation"></a>変更の保存操作のバッチ処理  
 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> メソッドを呼び出すと、コンテキストが追跡するすべての変更が、OData サービスに要求として送信される REST ベースの操作に変換されます。 既定では、これらの変更は 1 つの要求メッセージで送信されません。 すべての変更を1つの要求で送信することを要求するには、<xref:System.Data.Services.Client.DataServiceContext.SaveChanges%28System.Data.Services.Client.SaveChangesOptions%29> メソッドを呼び出し、メソッドに指定する <xref:System.Data.Services.Client.SaveChangesOptions> 列挙体に <xref:System.Data.Services.Client.SaveChangesOptions.Batch> の値を含める必要があります。  
  
 バッチ処理された変更を非同期で保存することもできます。 詳細については、「[非同期操作](asynchronous-operations-wcf-data-services.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [WCF Data Services クライアント ライブラリ](wcf-data-services-client-library.md)
