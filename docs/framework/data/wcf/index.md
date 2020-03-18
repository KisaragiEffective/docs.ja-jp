---
title: WCF Data Services 4.5
ms.date: 03/30/2017
helpviewer_keywords:
- Astoria
- WCF Data Services, getting started
ms.assetid: 73d2bec3-7c92-4110-b905-11bb0462357a
ms.openlocfilehash: aace683b1a105445b5a3ba3de0a6a671859588b5
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75937441"
---
# <a name="wcf-data-services-45"></a>WCF Data Services 4.5

WCF Data Services (旧称 "ADO.NET Data Services" と呼ばれていた) は、 [Representational State Transfer (REST)](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)のセマンティクスを使用して、Web またはイントラネット上のデータを公開および使用するための Open Data Protocol (OData) を使用するサービスを作成できるようにする、.NET Framework のコンポーネントです。 OData は、URI でアドレス指定できるリソースとしてデータを公開します。 データへのアクセスやデータの変更には、HTTP の標準の動詞である GET、PUT、POST、および DELETE を使用します。 OData は、 [Entity Data Model](../adonet/entity-data-model.md)のエンティティとリレーションシップの規則を使用して、アソシエーションによって関連付けられたエンティティのセットとしてリソースを公開します。

WCF Data Services は、OData プロトコルを使用してリソースのアドレス指定と更新を行います。 このようにして、OData をサポートする任意のクライアントからこれらのサービスにアクセスできます。 OData を使用すると、一般的な転送形式 (Atom、データを XML として交換および更新するための標準のセット)、および AJAX で幅広く使用されるテキストベースのデータ交換形式である JavaScript Object Notation (JSON) を使用して、リソースにデータを要求して書き込むことができます。プログラム.

WCF Data Services は、さまざまなソースから送信されたデータを OData フィードとして公開できます。 Visual Studio tools を使用すると、ADO.NET Entity Framework データモデルを使用して、OData ベースのサービスを簡単に作成できます。 また、共通言語ランタイム (CLR) クラスに基づいて OData フィードを作成したり、遅延バインディングまたは型指定されていないデータを作成したりすることもできます。

WCF Data Services には、一般的な .NET Framework クライアントアプリケーション用と Silverlight ベースのアプリケーション用のクライアントライブラリのセットも含まれています。 これらのクライアントライブラリは、.NET Framework や Silverlight などの環境から OData フィードにアクセスするときに、オブジェクトベースのプログラミングモデルを提供します。

## <a name="where-should-i-start"></a>開始すべき場所

興味に応じて、次のトピックのいずれかで WCF Data Services の使用を開始することを検討してください。

すぐに使用を開始する…

- [クイック スタート](quickstart-wcf-data-services.md)

- [はじめに](getting-started-with-wcf-data-services.md)

コードを表示するだけです...

- [クイック スタート](quickstart-wcf-data-services.md)

- [方法: データ サービス クエリを実行する](how-to-execute-data-service-queries-wcf-data-services.md)

- [方法: Windows Presentation Foundation 要素にデータをバインドする](bind-data-to-wpf-elements-wcf-data-services.md)

OData の詳細を知りたい...

- [ホワイトペーパー: OData の概要](https://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf)
- [Open Data Protocol web サイト](https://www.odata.org/)
- [OData: SDK](https://www.odata.org/ecosystem/)

エンドツーエンドのサンプルを確認するには...

- [WCF Data Services クイックスタート](https://github.com/microsoftarchive/msdn-code-gallery-community-s-z/tree/master/WCF%20Data%20Services%20Quickstart%20(OData%20Service%20and%20WPF%20Client))
- [OData SDK-サンプルコード](https://www.odata.org/ecosystem/#sdk)

Visual Studio との統合について知りたい

- [データ サービス クライアント ライブラリの生成](generating-the-data-service-client-library-wcf-data-services.md)

- [データ サービスの作成](creating-the-data-service.md)

- [Entity Framework プロバイダー](entity-framework-provider-wcf-data-services.md)

何に使用できるかを知りたい

- [概要](wcf-data-services-overview.md)

- [アプリケーション シナリオ](application-scenarios-wcf-data-services.md)

LINQ...

- [データ サービスに対するクエリ](querying-the-data-service-wcf-data-services.md)

- [LINQ に関する留意点](linq-considerations-wcf-data-services.md)

- [方法: データ サービス クエリを実行する](how-to-execute-data-service-queries-wcf-data-services.md)

まだいくつかの情報が必要です...

- [WCF Data Services チームのブログ](https://docs.microsoft.com/archive/blogs/astoriateam/)

- [リソース](wcf-data-services-resources.md)

## <a name="in-this-section"></a>このセクションの内容

[概要](wcf-data-services-overview.md)

WCF Data Services で使用できる機能の概要について説明します。

[WCF Data Services 5.0 の新機能](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/ee373845(v=vs.103))

WCF Data Services の新機能と、新しい OData 機能のサポートについて説明します。

[はじめに](getting-started-with-wcf-data-services.md)

WCF Data Services を使用して OData フィードを公開および使用する方法について説明します。

[WCF Data Services の定義](defining-wcf-data-services.md)

OData フィードを公開するデータサービスを作成および構成する方法について説明します。

[WCF Data Services クライアント ライブラリ](wcf-data-services-client-library.md)

クライアントライブラリを使用して、.NET Framework クライアントアプリケーションから OData フィードを使用する方法について説明します。

## <a name="see-also"></a>関連項目

- [Representational State Transfer (REST)](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
