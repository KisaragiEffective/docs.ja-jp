---
title: クエリ射影 (WCF Data Services)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- projection [WCF Data Services]
- WCF Data Services, projection
- query projection [WCF Data Services]
- WCF Data Services, querying
ms.assetid: a09f4985-9f0d-48c8-b183-83d67a3dfe5f
ms.openlocfilehash: 764ea6a77ba267e691d48bc72d17c02f6b3c18ca
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75900971"
---
# <a name="query-projections-wcf-data-services"></a>クエリ射影 (WCF Data Services)

射影では、Open Data Protocol (OData) のメカニズムを使用して、エンティティの特定のプロパティのみが応答で返されるように指定することによって、クエリによって返されるフィードのデータ量を削減します。 詳細については、セクション4.8 を参照してください。 [URI 規則 (OData バージョン 2.0)](https://www.odata.org/documentation/odata-version-2-0/uri-conventions/)のシステムクエリオプション ($select) を選択します。

このトピックでは、クエリ射影を定義する方法、エンティティ型、エンティティ型以外の要件、射影された結果に対する更新、射影された型の作成について説明すると共に、射影に関する注意事項をリストします。

## <a name="defining-a-query-projection"></a>クエリ射影の定義

クエリに射影句を追加するには、URI で `$select` クエリオプションを使用するか、LINQ クエリで[select](../../../csharp/language-reference/keywords/select-clause.md)句 (Visual Basic で[選択](../../../visual-basic/language-reference/queries/select-clause.md)) を使用します。 返されたエンティティ データは、クライアント上のエンティティ型またはエンティティ型以外に射影できます。 このトピックでは、LINQ クエリで `select` 句を使用する例を取り上げます。

> [!IMPORTANT]
> 射影された型に対して行った更新を保存すると、データ サービスでデータの損失が発生する場合があります。 詳細については、「[射影の考慮事項](#considerations)」を参照してください。

## <a name="requirements-for-entity-and-non-entity-types"></a>エンティティ型およびエンティティ型以外の要件

エンティティ型には、エンティティ キーを構成する 1 つ以上の Identity プロパティが必要です。 エンティティ型は、次のいずれかの方法によりクライアントで定義されます。

- <xref:System.Data.Services.Common.DataServiceKeyAttribute> または <xref:System.Data.Services.Common.DataServiceEntityAttribute> を型に適用する。

- 型に `ID` という名前のプロパティがある場合。

- 型に*型*`ID`という名前のプロパティがある*場合は、type が型*の名前になります。

既定では、クライアントで定義された型にクエリ結果を射影した場合、射影で要求されたプロパティがクライアント型に存在する必要があります。 ただし、`true` の <xref:System.Data.Services.Client.DataServiceContext.IgnoreMissingProperties%2A> プロパテに <xref:System.Data.Services.Client.DataServiceContext> という値を指定した場合、射影で指定されたプロパティがクライアント型に含まれる必要はありません。

### <a name="making-updates-to-projected-results"></a>射影された結果に対する更新

クエリ結果をクライアント上のエンティティ型に射影する場合、<xref:System.Data.Services.Client.DataServiceContext> はそれらのオブジェクトを追跡し、<xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> メソッドが呼び出されるとデータ サービスに更新内容が送り返されます。 ただし、クライアント上のエンティティ型以外に射影されたデータの更新内容は、データ サービスに送り返すことはできません。 これは、エンティティ インスタンスを識別するキーがなければ、データ サービスはデータ ソース内の正しいエンティティを更新できないためです。 エンティティ型以外は <xref:System.Data.Services.Client.DataServiceContext> にはアタッチされません。

データ サービスで定義されたエンティティ型の 1 つ以上のプロパティが、エンティティの射影先のクライアント型に含まれない場合、新しいエンティティの挿入には、これらの欠損しているプロパティは含まれません。 この場合、既存のエンティティに対して行われた更新に**も**、これらの不足しているプロパティは含まれません。 そのようなプロパティに対する値が存在する場合、データ ソースの定義に従い、更新ではその値がそのプロパティの既定値としてリセットされます。

### <a name="creating-projected-types"></a>射影型の作成

次の例では、`Customers` 型のアドレス関連のプロパティを、クライアントで定義されエンティティ型として属性化された新しい `CustomerAddress` 型に射影する匿名の LINQ クエリを使用します。

[!code-csharp[Astoria Northwind Client#SelectCustomerAddressSpecific](~/samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#selectcustomeraddressspecific)]
[!code-vb[Astoria Northwind Client#SelectCustomerAddressSpecific](~/samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#selectcustomeraddressspecific)]

この例では、コンストラクターを呼び出す代わりに、オブジェクト初期化子パターンを使用して `CustomerAddress` 型の新しいインスタンスを作成します。 コンストラクターは、エンティティ型への射影ではサポートされていませんが、エンティティ型以外および匿名型への射影では使用できます。 `CustomerAddress` は、エンティティ型であるため、変更を加えてデータ サービスに送り返すことができます。

さらに、`Customer` 型からのデータは、匿名型ではなく `CustomerAddress` エンティティ型のインスタンスに射影されます。 匿名型への射影はサポートされていますが、匿名型はエンティティ型以外として扱われるので、データは読み取り専用です。

<xref:System.Data.Services.Client.MergeOption> の <xref:System.Data.Services.Client.DataServiceContext> 設定は、クエリ射影時の ID 解決に使用されます。 これは、`Customer` 型のインスタンスが <xref:System.Data.Services.Client.DataServiceContext> に存在する場合、同じ ID を持つ `CustomerAddress` のインスタンスは <xref:System.Data.Services.Client.MergeOption> で設定された ID 解決ルールに従うことを意味します。

次に、結果をエンティティ型と非エンティティ型に射影する場合の動作について説明します。

**初期化子を使用して新しい射影インスタンスを作成する**

- 例:

   [!code-csharp[Astoria Northwind Client#ProjectWithInitializer](~/samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#projectwithinitializer)]
   [!code-vb[Astoria Northwind Client#ProjectWithInitializer](~/samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#projectwithinitializer)]

- エンティティ型: サポートされています

- 非エンティティ型: サポートされています

**コンストラクターを使用して新しい射影インスタンスを作成する**

- 例:

   [!code-csharp[Astoria Northwind Client#ProjectWithConstructor](~/samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#projectwithconstructor)]
   [!code-vb[Astoria Northwind Client#ProjectWithConstructor](~/samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#projectwithconstructor)]

- エンティティ型: <xref:System.NotSupportedException> が発生します。

- 非エンティティ型: サポートされています

**プロジェクションを使用してプロパティ値を変換する**

- 例:

   [!code-csharp[Astoria Northwind Client#ProjectWithTransform](~/samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#projectwithtransform)]
   [!code-vb[Astoria Northwind Client#ProjectWithTransform](~/samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#projectwithtransform)]

- エンティティ型: この変換は、混乱を招く可能性があり、別のエンティティに属するデータソースのデータを上書きする可能性があるため、エンティティ型ではサポートされていません。 <xref:System.NotSupportedException> 発生

- 非エンティティ型: サポートされています

<a name="considerations"></a>

## <a name="projection-considerations"></a>射影時の注意事項

クエリ射影を定義する場合は、次の点にも注意してください。

- Atom 形式のフィードを独自に定義する場合、カスタム マッピングが定義されているすべてのエンティティ プロパティが射影に含まれることを確認する必要があります。 マップされているエンティティ プロパティがこの射影に含まれていない場合、データの損失が発生することがあります。 詳細については、「[フィードのカスタマイズ](feed-customization-wcf-data-services.md)」を参照してください。

- データ サービスのデータ モデルのエンティティのすべてのプロパティを含まない射影型に挿入を行った場合、クライアントで射影に含まれていないプロパティは既定値に設定されます。

- データ サービスのデータ モデルのエンティティのすべてのプロパティを含まない射影影型に対して更新を行った場合、クライアントで射影に含まれていない既存の値は初期化されていない既定値で上書きされます。

- 射影に複合プロパティが含まれる場合、複合オブジェクト全体が返される必要があります。

- 射影にナビゲーション プロパティが含まれる場合、<xref:System.Data.Services.Client.DataServiceQuery%601.Expand%2A> メソッドを呼び出す必要はなく、関連オブジェクトが暗黙的に読み込まれます。 射影されたクエリでの <xref:System.Data.Services.Client.DataServiceQuery%601.Expand%2A> メソッドの使用はサポートされません。

- クライアント上のクエリ射影クエリは、要求 URI の `$select` クエリ オプションを使用するように変換されます。 `$select` クエリオプションをサポートしていない以前のバージョンの WCF Data Services に対して射影を使用したクエリを実行すると、エラーが返されます。 これは、データ サービスの  <xref:System.Data.Services.DataServiceBehavior.MaxProtocolVersion%2A> の <xref:System.Data.Services.DataServiceBehavior> が <xref:System.Data.Services.Common.DataServiceProtocolVersion.V1> という値に設定されている場合にも発生します。 詳細については、「[データサービスのバージョン管理](data-service-versioning-wcf-data-services.md)」を参照してください。

詳細については、「[方法: クエリ結果を射影する](how-to-project-query-results-wcf-data-services.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [データ サービスに対するクエリ](querying-the-data-service-wcf-data-services.md)
