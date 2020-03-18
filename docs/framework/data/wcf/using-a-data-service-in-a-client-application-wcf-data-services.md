---
title: クライアント アプリケーションでのデータ サービスの使用 (WCF Data Services)
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, client library
- WCF Data Services, getting started
ms.assetid: 90872d0c-e989-4490-b3e9-54afb10d33d4
ms.openlocfilehash: 41d3af831ff3c99e7f3000593db52d307d37ac38
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75900901"
---
# <a name="using-a-data-service-in-a-client-application-wcf-data-services"></a>クライアント アプリケーションでのデータ サービスの使用 (WCF Data Services)
Web ブラウザーに URI を指定することによって、Open Data Protocol (OData) フィードを公開するサービスにアクセスできます。 URI はリソースのアドレスを提供し、要求メッセージがこれらのアドレスに送信されてリソースが表す基になるデータのアクセスまたは変更を行います。 ブラウザーは HTTP GET コマンドを発行し、要求されたリソースを OData フィードとして返します。 詳細については、「 [Web ブラウザーからサービスにアクセス](accessing-the-service-from-a-web-browser-wcf-data-services-quickstart.md)する」を参照してください。  
  
 Web ブラウザーは、OData サービスが予期されるデータを返すことをテストする場合に便利ですが、データの作成、更新、および削除を可能にする運用環境の OData サービスは、通常、Web ページのアプリケーションコードまたはスクリプト言語によってアクセスされます。 このトピックでは、クライアントアプリケーションから OData フィードにアクセスする方法の概要について説明します。  
  
## <a name="accessing-and-changing-data-using-rest-semantics"></a>REST セマンティクスを使用したデータのアクセスおよび変更  
 Odata は、odata フィードを公開するサービスと odata フィードを使用するアプリケーションとの間の相互運用性を保証するのに役立ちます。 アプリケーションは、特定の HTTP アクションの要求メッセージと、アクションを実行するエンティティリソースをアドレス指定する URI を送信することによって、OData ベースのサービス内のデータにアクセスし、変更します。 エンティティ データを指定する必要がある場合、メッセージの本文に、エンコードされたペイロードとして明示的に指定します。  
  
### <a name="http-actions"></a>HTTP アクション  
 OData は、アドレス指定されたリソースが表すエンティティデータに対して、作成、読み取り、更新、削除の各操作を実行するために、次の HTTP アクションをサポートしています。  
  
- **HTTP GET** -これは、ブラウザーからリソースにアクセスするときの既定のアクションです。 要求メッセージではペイロードは指定されず、要求されたデータを含むペイロードを含む応答メソッドが返されます。  
  
- **HTTP POST** -指定されたリソースに新しいエンティティデータを挿入します。 挿入するデータは、要求メッセージのペイロードで指定されます。 応答メッセージのペイロードには、新しく作成されたエンティティのデータが含まれます。 これには自動生成されたキー値が含まれます。 ヘッダーには、新しいエンティティ リソースのアドレスを指定する URI も含まれます。  
  
- **HTTP DELETE** -指定されたリソースが表すエンティティデータを削除します。 ペイロードは要求メッセージおよび応答メッセージ内に存在しません。  
  
- **HTTP PUT** -要求されたリソースの既存のエンティティデータを、要求メッセージのペイロードで指定された新しいデータで置き換えます。  
  
- **HTTP マージ**-エンティティデータを変更するためだけに削除を実行した後、データソースに挿入すると、OData によって新しい HTTP マージアクションが導入されます。 要求メッセージのペイロードには、アドレス指定されたエンティティ リソースで変更する必要のあるプロパティが含まれます。 Http MERGE は HTTP 仕様で定義されていないため、非 OData 対応サーバー経由で HTTP MERGE 要求をルーティングするために、追加の処理が必要になる場合があります。  
  
 詳細については、「 [OData:](https://www.odata.org/documentation/odata-version-2-0/operations/)operation」を参照してください。
  
### <a name="payload-formats"></a>ペイロード形式  
 HTTP PUT、HTTP POST、または HTTP MERGE 要求の場合、要求メッセージのペイロードは、データ サービスに送信するエンティティ データを含んでいます。 ペイロードのコンテンツは、メッセージのデータ形式によって異なります。 そのようなペイロードは、DELETE 以外のすべてのアクションに対する HTTP 応答にも含まれます。 OData は、サービスを使用してデータにアクセスし、データを変更するための次のペイロード形式をサポートしています。  
  
- **Atom** -Web フィード、ポッドキャスト、wiki、および XML ベースのインターネット機能に対して HTTP 経由のデータ交換を有効にするために、OData によって Atom 公開プロトコル (AtomPub) の拡張として定義される xml ベースのメッセージエンコーディング。 詳細については、「 [OData: Atom 形式](https://www.odata.org/documentation/odata-version-2-0/atom-format/)」を参照してください。
  
- **Json (json** ) は、JavaScript プログラミング言語のサブセットに基づく軽量のデータ交換形式で、json (json) JavaScript Object Notation ます。 詳細については、「 [OData: JSON 形式](https://www.odata.org/documentation/odata-version-2-0/json-format/)」を参照してください。
  
 ペイロードのメッセージ形式は、HTTP 要求メッセージのヘッダーで要求されます。 詳細については、「 [OData:](https://www.odata.org/documentation/odata-version-2-0/operations/)operation」を参照してください。
  
## <a name="accessing-and-changing-data-using-client-libraries"></a>クライアント ライブラリを使用したデータのアクセスおよび変更  
 WCF Data Services には、.NET Framework および Silverlight ベースのクライアントアプリケーションから OData フィードを簡単に使用できるようにするクライアントライブラリが含まれています。 これらのライブラリは、HTTP メッセージの送受信を簡略化します。 また、メッセージ ペイロードをエンティティ データを表す CLR オブジェクトに変換します。 クライアント ライブラリには、 <xref:System.Data.Services.Client.DataServiceContext> および <xref:System.Data.Services.Client.DataServiceQuery%601>という 2 つのコア クラスがあります。 これらのクラスを使用すると、データ サービスをクエリして、返されるエンティティ データを CLR オブジェクトとして処理できます。 詳細については、「 [WCF Data Services クライアントライブラリ](wcf-data-services-client-library.md)と[WCF Data Services (Silverlight)](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838234(v=vs.95))」を参照してください。  
  
 Visual Studio の **[サービス参照の追加]** ダイアログボックスを使用して、データサービスへの参照を追加できます。 このツールは、参照されたデータ サービスからサービス メタデータを要求し、データ サービスを表す <xref:System.Data.Services.Client.DataServiceContext> を生成します。また、エンティティを表すクライアント データ サービス クラスも生成します。 詳細については、「[データサービスクライアントライブラリの生成](generating-the-data-service-client-library-wcf-data-services.md)」を参照してください。  
  
 他の種類のクライアントアプリケーションで OData フィードを利用するために使用できるプログラミングライブラリが用意されています。 OData SDK の詳細については、 [ODATA sdk のサンプルコード](https://www.odata.org/ecosystem/#sdk)を参照してください。
  
## <a name="see-also"></a>関連項目

- [データ サービス リソースへのアクセス](accessing-data-service-resources-wcf-data-services.md)
- [クイック スタート](quickstart-wcf-data-services.md)
