---
title: WCF Data Services プロトコル実装の詳細
ms.date: 03/30/2017
ms.assetid: 712d689b-fada-4cbb-bcdb-d65a3ef83b4c
ms.openlocfilehash: 2302b5577bec3fc4221bc6e5161c87905c38bec3
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75900869"
---
# <a name="wcf-data-services-protocol-implementation-details"></a>WCF Data Services プロトコル実装の詳細
## <a name="odata-protocol-implementation-details"></a>OData プロトコル実装の詳細  
Open Data Protocol (OData) では、プロトコルを実装するデータサービスが特定の最小限の機能セットを提供する必要があります。 これらの機能については、プロトコルドキュメント「必要条件」と「必要」で説明しています。 その他のオプション機能については、「may」で説明します。 この記事では、WCF Data Services によって現在実装されていないこれらのオプション機能について説明します。
  
### <a name="support-for-the-format-query-option"></a>$format クエリ オプションのサポート  
 OData プロトコルでは、JavaScript 表記 (JSON) と Atom フィードの両方がサポートされており、OData には、クライアントが応答フィードの形式を要求できるようにする `$format` システムクエリオプションが用意されています。 このシステム クエリ オプションは、データ サービスでサポートされている場合、要求の Accept ヘッダーの値をオーバーライドする必要があります。 WCF Data Services は、JSON フィードと Atom フィードの両方を返すことができます。 ただし、既定の実装では `$format` クエリ オプションをサポートしておらず、Accept ヘッダーの値だけを使用して応答の形式を決定します。 クライアントが Accept ヘッダーを設定できない場合のように、データ サービスで `$format` クエリ オプションをサポートしなければならないこともあります。 このようなシナリオをサポートするには、このオプションを URI で処理するように、データ サービスを拡張する必要があります。 この機能をデータサービスに追加するには、MSDN コードギャラリー web サイトから[ADO.NET Data Services sample プロジェクトの JSONP および URL で制御される形式のサポート](https://go.microsoft.com/fwlink/?LinkId=208228)をダウンロードし、データサービスプロジェクトに追加します。 このサンプルは、`$format` クエリ オプションを削除し、Accept ヘッダーを `application/json` に変更します。 サンプル プロジェクトを含めるときに、`JSONPSupportBehaviorAttribute` をデータ サービス クラスに追加すると、サービスが `$format` クエリ オプションの `$format=json` を処理できるようになります。 `$format=atom` や他のカスタム形式も処理するには、このサンプル プロジェクトをさらにカスタマイズする必要があります。  
  
## <a name="wcf-data-services-behaviors"></a>WCF Data Services の動作  
 次の WCF Data Services 動作は、OData プロトコルによって明示的に定義されていません。  
  
### <a name="default-sorting-behavior"></a>既定の並べ替え動作  
 データ サービスに送信されるクエリ要求に、`$top` または `$skip` システム クエリ オプションが含まれ、`$orderby` システム クエリ オプションが含まれていない場合、返されるフィードはキー プロパティで昇順に並べ替えられます。 これは、結果を正しくページングするには並べ替えが必要であるためです。 そのために、データ サービスがクエリに並べ替え式を追加します。 この動作は、データ サービスでサーバー ドリブン ページングが有効になっている場合にも発生します。 詳細については、「[データサービスの構成](configuring-the-data-service-wcf-data-services.md)」を参照してください。返されたフィードの順序を制御するには、クエリ URI に `$orderby` を含める必要があります。  
  
## <a name="see-also"></a>関連項目

- [WCF Data Services の定義](defining-wcf-data-services.md)
- [WCF Data Services クライアント ライブラリ](wcf-data-services-client-library.md)
