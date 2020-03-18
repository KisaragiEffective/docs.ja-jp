---
title: サービス操作 (WCF Data Services)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- service operations [WCF Data Services]
- WCF Data Services, service operations
ms.assetid: 583a690a-e60f-4990-8991-d6efce069d76
ms.openlocfilehash: c254a7362c7bc28f4b38fc0189ae0ea763bc90cc
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74568846"
---
# <a name="service-operations-wcf-data-services"></a>サービス操作 (WCF Data Services)

WCF Data Services を使用すると、データサービスのサービス操作を定義して、サーバーでメソッドを公開できます。 その他のデータ サービス リソースと同様に、サービス操作は URI によってアドレス指定できます。 サービス操作では、データ サービスでビジネス ロジックを公開できます (検証ロジックの実装、ロール ベースのセキュリティの適用、特殊なクエリ機能の公開など)。 サービス操作は、<xref:System.Data.Services.DataService%601> から派生するデータ クラスに追加されるメソッドです。 その他のすべてのデータ サービス リソースと同様に、パラメーターをサービス操作メソッドに指定できます。 たとえば、次のサービス操作 URI ([クイックスタート](quickstart-wcf-data-services.md)データサービスに基づく) は、`London` 値を `city` パラメーターに渡します。

```http
http://localhost:12345/Northwind.svc/GetOrdersByCity?city='London'
```

このサービス操作の定義を次に示します。

[!code-csharp[Astoria Northwind Service#ServiceOperationDef](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_service/cs/northwind2.svc.cs#serviceoperationdef)]
[!code-vb[Astoria Northwind Service#ServiceOperationDef](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_service/vb/northwind2.svc.vb#serviceoperationdef)]

<xref:System.Data.Services.DataService%601.CurrentDataSource%2A> の <xref:System.Data.Services.DataService%601> を使用して、データ サービスが使用するデータ ソースに直接アクセスできます。 詳細については、「[方法: サービス操作を定義](how-to-define-a-service-operation-wcf-data-services.md)する」を参照してください。

.NET Framework クライアントアプリケーションからサービス操作を呼び出す方法については、「[サービス操作の呼び出し](calling-service-operations-wcf-data-services.md)」を参照してください。

## <a name="service-operation-requirements"></a>サービス操作の要件

データ サービスでサービス操作を定義する場合、次の要件が適用されます。 これらの要件を満たしていないメソッドは、データ サービスのサービス操作として公開されません。

- 操作は、データ サービス クラスのメンバーであるパブリック インスタンス メソッドである必要があります。

- 操作メソッドは入力パラメーターだけを受け取ることができます。 メッセージの本文内で送信されるデータは、データ サービスからアクセスできません。

- パラメーターを定義する場合、各パラメーターの型はプリミティブ型である必要があります。 非プリミティブ型のいずれかのデータもシリアル化され、文字列パラメーターに渡される必要があります。

- メソッドは次のいずれかを返す必要があります。

  - `void` (Visual Basic の場合は `Nothing`)。

  - <xref:System.Collections.Generic.IEnumerable%601>

  - <xref:System.Linq.IQueryable%601>

  - データ サービスが公開するデータ モデル内のエンティティ型。

  - プリミティブ クラス (整数や文字列など)。

- 並べ替え、ページング、フィルター処理などのクエリ オプションをサポートするために、サービス操作メソッドから <xref:System.Linq.IQueryable%601> が返される必要があります。 クエリ オプションを含むサービス操作への要求は、<xref:System.Collections.Generic.IEnumerable%601> だけを返す操作で拒否されます。

- ナビゲーション プロパティを使用した関連エンティティへのアクセスをサポートするために、サービス操作は <xref:System.Linq.IQueryable%601> を返す必要があります。

- メソッドには、`[WebGet]` 属性または `[WebInvoke]` 属性を使用して注釈を付ける必要があります。

  - `[WebGet]` では、GET 要求を使用してメソッドを呼び出すことができます。

  - `[WebInvoke(Method = "POST")]` では、POST 要求を使用してメソッドを呼び出すことができます。 その他の <xref:System.ServiceModel.Web.WebInvokeAttribute> メソッドはサポートされていません。

- サービス操作には、<xref:System.Data.Services.SingleResultAttribute> を使用して注釈を付けることができます。この属性は、メソッドからの戻り値がエンティティのコレクションではなく 1 つのエンティティとなるように指定します。 この区別により、応答の結果のシリアル化、および追加のナビゲーション プロパティのトラバーサルを URI で表す方法が決定されます。 たとえば、AtomPub シリアル化を使用すると、1 種類のリソースのインスタンスがエントリ要素として表され、一連のインスタンスがフィード要素として表されます。

## <a name="addressing-service-operations"></a>サービス操作のアドレス指定

メソッドの名前を URI の最初のパス セグメントに配置することによってサービス操作のアドレスを指定できます。 たとえば、次の URI の場合、`GetOrdersByState` オブジェクトの <xref:System.Linq.IQueryable%601> コレクションを返す `Orders` 操作にアクセスします。

```http
http://localhost:12345/Northwind.svc/GetOrdersByState?state='CA'&includeItems=true
```

サービス操作を呼び出すときは、パラメーターはクエリのオプションとして指定されます。 前のサービス操作は、文字列パラメーター `state` と、関連する `includeItems` オブジェクトを応答に含めるかどうかを指定するブール値パラメーター `Order_Detail` の両方を受け入れます。

サービス操作の有効な戻り値の型を次に示します。

|有効な戻り値の型|URI のルール|
|------------------------|---------------|
|`void` (Visual Basic の場合は `Nothing`)。<br /><br /> -または-<br /><br /> エンティティ型<br /><br /> -または-<br /><br /> プリミティブ型|URI は、サービス操作の名前である 1 つのパス セグメントである必要があります。 クエリ オプションは許可されません。|
|<xref:System.Collections.Generic.IEnumerable%601>|URI は、サービス操作の名前である 1 つのパス セグメントである必要があります。 結果型は <xref:System.Linq.IQueryable%601> 型ではないので、クエリ オプションは使用できません。|
|<xref:System.Linq.IQueryable%601>|サービス操作の名前であるパスに追加したクエリ パス セグメント セグメントが許可されます。 クエリ オプションも許可されます。|

サービス操作の戻り値の型によっては、パス セグメントやクエリ オプションをさらに URI に追加できます。 たとえば、次の URI は、関連する `Order_Details` オブジェクトと一緒に `Orders` オブジェクトの <xref:System.Linq.IQueryable%601> コレクションを `RequiredDate` の降順で返す `GetOrdersByCity` 操作にアクセスします。

```http
http://localhost:12345/Northwind.svc/GetOrdersByCity?city='London'&$expand=Order_Details&$orderby=RequiredDate desc
```

## <a name="service-operations-access-control"></a>サービス操作のアクセス制御

サービス操作のサービス全体の表示は、エンティティ セットの表示が <xref:System.Data.Services.IDataServiceConfiguration.SetServiceOperationAccessRule%2A> メソッドを使用して制御されるのと同じような方法で、<xref:System.Data.Services.IDataServiceConfiguration> クラスの <xref:System.Data.Services.IDataServiceConfiguration.SetEntitySetAccessRule%2A> メソッドによって制御されます。 たとえば、データ サービス定義内の次のコード行は、`CustomersByCity` サービス操作へのアクセスを有効にします。

[!code-csharp[Astoria Northwind Service#ServiceOperationConfig](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_service/cs/northwind2.svc.cs#serviceoperationconfig)]
[!code-vb[Astoria Northwind Service#ServiceOperationConfig](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_service/vb/northwind2.svc.vb#serviceoperationconfig)]

> [!NOTE]
> 元になるエンティティ セットの制限アクセスによって非表示にされている戻り値の型がサービス操作にある場合、サービス操作はクライアント アプリケーションで使用できません。

詳細については、「[方法: サービス操作を定義](how-to-define-a-service-operation-wcf-data-services.md)する」を参照してください。

## <a name="raising-exceptions"></a>例外の発生

データ サービスの実行で例外が発生するたびに、<xref:System.Data.Services.DataServiceException> クラスを使用することお勧めします。 これは、データ サービス ランタイムがこの例外のオブジェクトのプロパティを HTTP 応答メッセージに正しくマップする方法を認識しているためです。 サービス操作で <xref:System.Data.Services.DataServiceException> が発生する場合、返された例外は <xref:System.Reflection.TargetInvocationException> にラップされています。 <xref:System.Data.Services.DataServiceException> をラップせずにベース <xref:System.Reflection.TargetInvocationException> を返すには、<xref:System.Data.Services.DataService%601.HandleException%2A> の <xref:System.Data.Services.DataService%601> メソッドをオーバーライドし、<xref:System.Data.Services.DataServiceException> から <xref:System.Reflection.TargetInvocationException> を抽出して、トップ レベル エラーとして返す必要があります。次に例を示します。

[!code-csharp[Astoria Northwind Service#HandleExceptions](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_service/cs/northwind2.svc.cs#handleexceptions)]
[!code-vb[Astoria Northwind Service#HandleExceptions](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_service/vb/northwind2.svc.vb#handleexceptions)]

## <a name="see-also"></a>参照

- [インターセプター](interceptors-wcf-data-services.md)
