---
title: データ サービス リソースへのアクセス (WCF Data Services)
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, querying
- getting started, WCF Data Services
- querying the data service [WCF Data Service]
- WCF Data Services, getting started
- WCF Data Services, accessing data
ms.assetid: 9665ff5b-3e3a-495d-bf83-d531d5d060ed
ms.openlocfilehash: 7eea23ba3dc5e9cc327d9cdfba10c72af7525c30
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569404"
---
# <a name="accessing-data-service-resources-wcf-data-services"></a>データ サービス リソースへのアクセス (WCF Data Services)
WCF Data Services は、Open Data Protocol (OData) をサポートして、Uri でアドレス指定できるリソースを含むフィードとしてデータを公開します。 これらのリソースは、 [Entity Data Model](../adonet/entity-data-model.md)のエンティティとリレーションシップの規則に従って表されます。 このモデルでは、エンティティはアプリケーション ドメイン内のデータの操作単位 (データ型) を表します (顧客、注文、項目、製品など)。 エンティティ データは、Representational State Transfer (REST) のセマンティクス (特に、標準的な HTTP 動詞である GET、PUT、POST、および DELETE) を使用してアクセスおよび変更できます。  
  
## <a name="addressing-resources"></a>リソースへの対処  
 OData では、URI を使用してデータモデルによって公開されているすべてのデータに対応します。 たとえば、次の URI は、Customers エンティティセットであるフィードを返します。これには、Customer エンティティ型のすべてのインスタンスのエントリが含まれます。  
  
<https://services.odata.org/Northwind/Northwind.svc/Customers>
  
 エンティティには、エンティティ キーという特別なプロパティがあります。 エンティティ キーは、エンティティ セット内の 1 つのエンティティを一意に識別するために使用されます。 そのため、エンティティ セット内のエンティティ型の特定のインスタンスのアドレスを指定できます。 たとえば、次の URI は、`ALFKI` のキー値のある Customer エンティティ型の特定のインスタンスのエントリを返します。  
  
<https://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')>
  
 エンティティ インスタンスのプリミティブ プロパティおよび複合プロパティは、個別にアドレス指定することもできます。 たとえば、次の URI は特定の Customer の `ContactName` プロパティ値が含まれた XML 要素を返します。  
  
<https://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/ContactName>
  
 前の URI に `$value` エンドポイントを含めた場合、応答メッセージにはプリミティブ プロパティの値のみが返されます。 次の例では、XML 要素のない "Maria Anders" という文字列のみが返されます。  
  
<https://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/ContactName/$value>
  
 エンティティ間のリレーションシップは、アソシエーションによってデータ モデル内で定義されます。 これらのアソシエーションによって、エンティティ インスタンスのナビゲーション プロパティを使用して関連エンティティをアドレス指定することが可能になります。 ナビゲーション プロパティは、単一の関連エンティティ (多対一のリレーションシップの場合) または関連エンティティのセット (一対多のリレーションシップの場合) のいずれかを返します。 たとえば、次の URI は、特定の Customer に関連付けられたすべての Orders のセットであるフィードを返します。  
  
<https://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/Orders>
  
 リレーションシップは通常は双方向であり、ナビゲーション プロパティのペアで表されます。 前の例で示したリレーションシップとは逆に、次の URI は特定の Order エンティティが属する Customer エンティティへの参照を返します。  
  
<https://services.odata.org/Northwind/Northwind.svc/Orders(10643)/Customer>
  
 OData を使用すると、クエリ式の結果に基づいてリソースのアドレスを確認することもできます。 これにより、評価された式に基づいてリソースのセットをフィルター処理できます。 たとえば、次の URI は、リソースをフィルターして特定の Customer に 1997 年 9 月 22 日以降に出荷された Orders だけを返します。  
  
<https://services.odata.org/Northwind/Northwind.svc/Customers( ' ALFKI ')/Orders? $filter = 発送日 gt datetime ' 1997-09-22T00:00:00 ' >
  
 詳細については、「 [OData: URI 規則](https://www.odata.org/documentation/odata-version-2-0/uri-conventions/)」を参照してください。
  
## <a name="system-query-options"></a>System Query Options (システム クエリ オプション)  
 OData は、フィルター処理、並べ替え、ページングなどのリソースに対して従来のクエリ操作を実行するために使用できる一連のシステムクエリオプションを定義します。 たとえば、次の URI は、関連する `Order_Detail` エンティティ、`100`で終了しない郵便番号など、すべての `Order` エンティティのセットを返します。  
  
<https://services.odata.org/Northwind/Northwind.svc/Orders? $filter = not endswith (ShipPostalCode, ' 100 ') & $expand = Order_Details & $orderby = ShipCity >
  
 返されたフィードのエントリは、注文の ShipCity プロパティの値でも並べ替えられています。  
  
 WCF Data Services では、次の OData システムクエリオプションがサポートされています。  
  
|クエリ オプション|説明|  
|------------------|-----------------|  
|`$orderby`|返されるフィードのエンティティについて、既定の並べ替え順序を定義します。 次のクエリで返される Customers フィードは、国 (County) と都市 (City) で並べ替えられています。<br /><br /> <https://services.odata.org/Northwind/Northwind.svc/Customers?$orderby=Country,City>|  
|`$top`|返されるフィードに含まれるエンティティの数を指定します。 次の例では、最初の 10 件の顧客をスキップし、その次の 10 件を返します。<br /><br /> <https://services.odata.org/Northwind/Northwind.svc/Customers?$skip=10&$top=10>|  
|`$skip`|フィードにエンティティを返し始めるまでにスキップするエンティティの数を指定します。 次の例では、最初の 10 件の顧客をスキップし、その次の 10 件を返します。<br /><br /> <https://services.odata.org/Northwind/Northwind.svc/Customers?$skip=10&$top=10>|  
|`$filter`|フィードに返されるエンティティを特定の条件に基づいてフィルターする式を定義します。 このクエリ オプションは、フィルター式の評価に使用される一連の論理比較演算子、算術演算子、および定義済みクエリ関数をサポートします。 次の例は、郵便番号の末尾が 100 でないすべての注文を返します。<br /><br /> <https://services.odata.org/Northwind/Northwind.svc/Orders? $filter = endswith (ShipPostalCode, ' 100 ') >|  
|`$expand`|クエリで返される関連エンティティを指定します。 関連エンティティは、クエリで返されるエンティティにフィードまたはエントリとしてインラインで含まれています。 次の例では、顧客 'ALFKI' の注文を各注文の品目の詳細と共に返します。<br /><br /> <https://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/Orders?$expand=Order_Details>|  
|`$select`|フィードとして返されるエンティティのプロパティを指定します。 既定では、エンティティのすべてのプロパティがフィードとして返されます。 次のクエリでは、`Customer` エンティティの 3 つのプロパティが返されます。<br /><br /> <https://services.odata.org/Northwind/Northwind.svc/Customers?$select=CustomerID,CompanyName,City>|  
|`$inlinecount`|フィードで返されるエンティティの数のカウントがフィードに含まれるように要求します。|  
  
## <a name="addressing-relationships"></a>リレーションシップのアドレス指定  
 OData では、エンティティセットとエンティティインスタンスをアドレス指定するだけでなく、エンティティ間のリレーションシップを表すアソシエーションをアドレス指定することもできます。 この機能は、2 つのエンティティ インスタンス (Northwind サンプル データベースの注文に関連付けられた配送会社など) の間のリレーションシップを作成または変更するために必要です。 OData では、エンティティ間のアソシエーションに特に対応するために、`$link` 演算子がサポートされています。 たとえば、次の URI を HTTP PUT 要求メッセージで指定した場合、指定した注文の配送会社を新しい配送会社に変更します。  
  
<https://services.odata.org/Northwind/Northwind.svc/Orders(10643)/$links/Shipper>
  
 詳細については、 [OData: URI 規則](https://www.odata.org/documentation/odata-version-2-0/uri-conventions/)の `3.2. Addressing Links between Entries` に関するセクションを参照してください。
  
## <a name="consuming-the-returned-feed"></a>返されたフィードの使用  
 OData リソースの URI を使用すると、サービスによって公開されるエンティティデータに対処できます。 Web ブラウザーの [アドレス] フィールドに URI を入力すると、要求されたリソースの OData フィード表現が返されます。 詳細については、 [WCF Data Services のクイックスタート](quickstart-wcf-data-services.md)を参照してください。 Web ブラウザーは、データサービスリソースから予想されるデータが返されることをテストする場合に便利ですが、データの作成、更新、および削除を行うことができる運用データサービスは、通常、Web ページのアプリケーションコードまたはスクリプト言語によってアクセスされます。 詳細については、「[クライアントアプリケーションでのデータサービスの使用](using-a-data-service-in-a-client-application-wcf-data-services.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [Open Data Protocol Web サイト](https://www.odata.org/)
