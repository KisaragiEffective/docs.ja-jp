---
description: '詳細情報: 配信のアーキテクチャ'
title: 配信のアーキテクチャ
ms.date: 03/30/2017
ms.assetid: ed4ca86e-e3d8-4acb-87aa-1921fbc353be
ms.openlocfilehash: 274ec121629bcb4e596401ad989309d3ef02c128
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643736"
---
# <a name="architecture-of-syndication"></a>配信のアーキテクチャ

配信 API は、形式に依存せず、さまざま形式で概要コンテンツをネットワークに書き込むことができるプログラミング モデルを提供することを目的としています。 抽象データ モデルは、次のクラスで構成されています。  
  
- <xref:System.ServiceModel.Syndication.SyndicationCategory>  
  
- <xref:System.ServiceModel.Syndication.SyndicationFeed>  
  
- <xref:System.ServiceModel.Syndication.SyndicationItem>  
  
- <xref:System.ServiceModel.Syndication.SyndicationLink>  
  
- <xref:System.ServiceModel.Syndication.SyndicationPerson>  
  
 これらのクラスは、一部の名前が異なっていますが、Atom 1.0 仕様に規定されるコンストラクトに厳密にマップされています。  
  
 Windows Communication Foundation (WCF) では、配信フィードはその戻り値の型が <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> の派生クラスの 1 つである別の種類のサービス操作としてモデル化されています。 フィードの取得は要求/応答のメッセージ交換としてモデル化されています。 クライアントはサービスに要求を送信し、サービスがこれに応答します。 要求メッセージはインフラストラクチャ プロトコル (生の HTTP など) 上に設定され、応答メッセージは広く認識されている配信形式 (RSS 2.0 または Atom 1.0) から構成されるペイロードを含んでいます。 このようなメッセージ交換を実装するサービスは、配信サービスと呼ばれます。  
  
 配信サービスのコントラクトは、<xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> クラスのインスタンスを返す一連の操作から構成されます。 配信サービスのインターフェイス宣言の例を次に示します。  
  
 [!code-csharp[S_UE_SyndicationBoth#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ue_syndicationboth/cs/service.cs#0)]  
  
 配信のサポートは、<xref:System.ServiceModel.WebHttpBinding> バインディングを定義する WCF REST プログラミング モデル上に組み込まれています。このバインディングを <xref:System.ServiceModel.Description.WebHttpBehavior> と共に使用すると、フィードをサービスとして利用できるようになります。 WCF REST プログラミング モデルの詳細については、「[WCF Web HTTP プログラミング モデルの概要](wcf-web-http-programming-model-overview.md)」を参照してください。  
  
> [!NOTE]
> Atom 1.0 仕様では、date コンストラクトで小数秒を指定できます。 シリアル化および逆シリアル化する場合、WCF の実装では小数秒は無視されます。  
  
## <a name="object-model"></a>オブジェクト モデル  

 配信のオブジェクト モデルは、次の各表に示すクラスのグループから構成されています。  
  
 形式設定クラス  
  
|クラス|説明|  
|-----------|-----------------|  
|<xref:System.ServiceModel.Syndication.Atom10FeedFormatter>|<xref:System.ServiceModel.Syndication.SyndicationFeed> インスタンスを Atom 1.0 形式にシリアル化するクラス。|  
|<xref:System.ServiceModel.Syndication.Atom10FeedFormatter%601>|<xref:System.ServiceModel.Syndication.SyndicationFeed> 派生クラスを Atom 1.0 形式にシリアル化するクラス。|  
|<xref:System.ServiceModel.Syndication.Atom10ItemFormatter>|<xref:System.ServiceModel.Syndication.SyndicationItem> インスタンスを Atom 1.0 形式にシリアル化するクラス。|  
|<xref:System.ServiceModel.Syndication.Atom10ItemFormatter%601>|<xref:System.ServiceModel.Syndication.SyndicationItem> 派生クラスを Atom 1.0 形式にシリアル化するクラス。|  
|<xref:System.ServiceModel.Syndication.Rss20FeedFormatter>|<xref:System.ServiceModel.Syndication.SyndicationFeed> インスタンスを RSS 2.0 形式にシリアル化するクラス。|  
|<xref:System.ServiceModel.Syndication.Rss20FeedFormatter%601>|<xref:System.ServiceModel.Syndication.SyndicationFeed> 派生クラスを RSS 2.0 形式にシリアル化するクラス。|  
|<xref:System.ServiceModel.Syndication.Rss20ItemFormatter>|<xref:System.ServiceModel.Syndication.SyndicationItem> インスタンスを RSS 2.0 形式にシリアル化するクラス。|  
|<xref:System.ServiceModel.Syndication.Rss20ItemFormatter%601>|<xref:System.ServiceModel.Syndication.SyndicationItem> 派生クラスを RSS 2.0 形式にシリアル化するクラス。|  
  
 オブジェクト モデル クラス  
  
|クラス|説明|  
|-----------|-----------------|  
|<xref:System.ServiceModel.Syndication.SyndicationCategory>|配信フィードのカテゴリを表すクラス。|  
|<xref:System.ServiceModel.Syndication.SyndicationContent>|配信コンテンツを表す基本クラス。|  
|<xref:System.ServiceModel.Syndication.SyndicationElementExtension>|配信要素拡張を表すクラス。|  
|<xref:System.ServiceModel.Syndication.SyndicationElementExtensionCollection>|<xref:System.ServiceModel.Syndication.SyndicationElementExtension> オブジェクトのコレクション。|  
|<xref:System.ServiceModel.Syndication.SyndicationFeed>|トップレベルのフィード オブジェクトを表すクラス。|  
|<xref:System.ServiceModel.Syndication.SyndicationItem>|フィード項目を表すクラス。|  
|<xref:System.ServiceModel.Syndication.SyndicationLink>|配信フィードまたは項目内のリンクを表すクラス。|  
|<xref:System.ServiceModel.Syndication.SyndicationPerson>|Atom Person コンストラクトを表すクラス。|  
|<xref:System.ServiceModel.Syndication.SyndicationVersions>|サポートされる配信プロトコルのバージョンを表すクラス。|  
|<xref:System.ServiceModel.Syndication.TextSyndicationContent>|エンド ユーザーに表示される任意の <xref:System.ServiceModel.Syndication.SyndicationItem> コンテンツを表すクラス。|  
|<xref:System.ServiceModel.Syndication.TextSyndicationContentKind>|テキスト配信コンテンツでサポートされる各種の型を表す列挙型。|  
|<xref:System.ServiceModel.Syndication.UrlSyndicationContent>|別のリソースへの URL から構成される配信コンテンツを表すクラス。|  
|<xref:System.ServiceModel.Syndication.XmlSyndicationContent>|ブラウザーに表示されない配信コンテンツを表すクラス。|  
  
 オブジェクト モデル内における抽象化コア データは Feed と Item であり、<xref:System.ServiceModel.Syndication.SyndicationFeed> クラスと <xref:System.ServiceModel.Syndication.SyndicationItem> クラスに対応します。 Feed は、フィード レベルのメタデータの一部 (Title、Description、Author など)、未知の拡張を格納する場所、およびフィードの情報コンテンツの残りの部分を作成する一連の項目を公開します。 Item では、項目レベルのメタデータの一部 (Title、Summary、PublicationDate など)、未知の拡張を格納する場所、および項目の情報コンテンツの残りの部分を含むコンテンツ要素を利用できます。 Feed と Item のコア抽象化は、Atom 1.0 および RSS の仕様で参照されている共通データ コンストラクトを表す追加のクラスによってサポートされています。  
  
 Feed インスタンスに含まれる情報は、各種の XML 形式に変換できます。 XML との間の双方向の変換処理は、<xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> クラスによって管理されます。 このクラスは抽象クラスであり、具体的な実装は Atom 1.0 および RSS 2.0 (<xref:System.ServiceModel.Syndication.Atom10FeedFormatter> および <xref:System.ServiceModel.Syndication.Rss20FeedFormatter>) で提供されます。 Feed の派生クラスを使用するには、派生した Feed クラスを特定できるように <xref:System.ServiceModel.Syndication.Atom10FeedFormatter%601> または <xref:System.ServiceModel.Syndication.Rss20FeedFormatter%601> のいずれかを使用します。 Item の派生クラスを使用するには、派生した Item クラスを特定できるように <xref:System.ServiceModel.Syndication.Atom10ItemFormatter%601> または <xref:System.ServiceModel.Syndication.Rss20ItemFormatter%601> のいずれかを使用します。各種の配信形式をサポートするために、サード パーティでは <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> の独自の実装を派生することができます。  
  
## <a name="extensibility"></a>機能拡張  
  
- 配信プロトコルの主な機能は拡張性です。 Atom 1.0 と RSS 2.0 では、仕様で定義されていない属性および要素を配信フィードに追加できます。 WCF の配信プログラミング モデルには、カスタム属性と拡張機能を扱う方法として、新しいクラスの派生と弱い型指定のアクセスの 2 つの方法が用意されています。 詳細については、[配信の拡張](syndication-extensibility.md)に関するページを参照してください。  
  
## <a name="see-also"></a>関連項目

- [WCF 配信の概要](wcf-syndication-overview.md)
- [WCF 配信オブジェクト モデルを Atom や RSS に割り当てる方法](how-the-wcf-syndication-object-model-maps-to-atom-and-rss.md)
- [WCF Web HTTP プログラミング モデル](wcf-web-http-programming-model.md)
