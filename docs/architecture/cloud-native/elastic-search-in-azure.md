---
title: クラウドネイティブアプリケーションでのエラスティックサーチ
description: クラウドネイティブ アプリケーションに Elastic Search 機能を追加する方法について説明します。
author: robvet
ms.date: 01/22/2020
ms.openlocfilehash: 1bce255b6315006b11e0b6ac77040300f67ed984
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79141290"
---
# <a name="elasticsearch-in-a-cloud-native-app"></a>クラウドネイティブ アプリでのエラスティック検索

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

Elasticsearch は分散型の検索および分析システムであり、多様な種類のデータに対して複雑な検索機能を提供します。 それはオープンソースで、広く普及しています。 次の企業が Elasticsearch をアプリケーションに統合する方法を検討してください。

- [ウィキペディア](https://blog.wikimedia.org/2014/01/06/wikimedia-moving-to-elasticsearch/)で全文検索とインクリメンタル検索(入力時に検索)
- [GitHub](https://www.elastic.co/customers/github)は、800 万を超えるコード リポジトリをインデックス化して公開します。  
- コンテナー ライブラリを検出可能にするための[Docker。](https://www.elastic.co/customers/docker)

弾性検索は[、Apache Lucene](https://lucene.apache.org/core/)フルテキスト検索エンジンの上に構築されています。 Lucene は、高パフォーマンスのドキュメントインデックス作成とクエリを提供します。 逆インデックス作成スキームでデータをインデックス化し、ページをキーワードにマッピングする代わりに、ブックの最後にある用語集と同じように、キーワードをページにマップします。 Lucene には強力なクエリ構文機能があり、次の方法でデータをクエリできます。

- 用語 (完全な単語)
- プレフィックス (先頭から単語)
- ワイルドカード ("" または "?"\*フィルターを使用)
- フレーズ (ドキュメント内の一連のテキスト)
- ブール値 (クエリを組み合わせた複雑な検索)

Lucene は検索用の低レベル配管を提供しますが、Elasticsearch は Lucene の上に位置するサーバーを提供します。 Elasticsearch は、Lucene のインデックス作成と検索機能にアクセスするための RESTful API など、作業を簡素化する高レベルの機能を追加します。 また、大規模な拡張性、フォールト トレランス、および高可用性を実現できる分散インフラストラクチャも提供します。

複雑な検索要件を持つ大規模なクラウド ネイティブ アプリケーションでは、Elasticsearch を Azure のマネージド サービスとして利用できます。 Microsoft Azure マーケットプレースには、開発者が Azure に Elasticsearch クラスターをデプロイするために使用できる、事前に構成されたテンプレートが用意されています。

開発者は、Microsoft Azure マーケットプレースから、Azure に Elasticsearch クラスターを迅速にデプロイするために構築された構成済みテンプレートを使用できます。 Azure 管理製品を使用すると、最大 50 個のデータ ノード、20 個の調整ノード、および 3 つの専用マスター ノードをデプロイできます。

## <a name="summary"></a>まとめ

この章では、クラウドネイティブシステムのデータについて詳しく説明しました。 まず、モノリシックアプリケーションのデータストレージとクラウドネイティブシステムのデータストレージパターンを比べていました。 クラウドネイティブシステムに実装されているデータパターン(サービス間のクエリ、分散トランザクション、大量のシステムに対応するパターンなど)を見てきました。 SQL と NoSQL データを対比しました。 マイクロソフト中心のオプションとオープン ソース オプションの両方を含む、Azure で利用可能なデータ ストレージ オプションについて説明しました。 最後に、クラウドネイティブアプリケーションでのキャッシュと Elasticsearch について説明しました。

### <a name="references"></a>References

- [コマンド クエリ責務分離 (CQRS) パターン](https://docs.microsoft.com/azure/architecture/patterns/cqrs)

- [イベント ソーシング パターン](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)

- [RDBMS と NoSQL データベース: 概要](https://maxivak.com/rdbms-vs-nosql-databases/)

- [CAP 定理で RDBMS パーティショントレラントが使用できないのはなぜですか。](https://stackoverflow.com/questions/36404765/why-isnt-rdbms-partition-tolerant-in-cap-theorem-and-why-is-it-available)

- [マテリアライズビュー](https://docs.microsoft.com/azure/architecture/patterns/materialized-view)

- [オープンソースデータベースについて本当に知っておくべきこと](https://www.ibm.com/blogs/systems/all-you-really-need-to-know-about-open-source-databases/)

- [補正トランザクション パターン](https://docs.microsoft.com/azure/architecture/patterns/compensating-transaction)

- [佐賀パターン](https://microservices.io/patterns/data/saga.html)

- [サガパターン |マイクロサービスを使用してビジネス トランザクションを実装する方法](https://blog.couchbase.com/saga-pattern-implement-business-transactions-using-microservices-part/)

- [補正トランザクション パターン](https://docs.microsoft.com/azure/architecture/patterns/compensating-transaction)

- [9ボールの背後に立つ:コスモスDBの一貫性レベルの説明](https://blog.jeremylikness.com/blog/2018-03-23_getting-behind-the-9ball-cosmosdb-consistency-levels/)

- [NoSQL データベースの異なるタイプの探索パート II](https://www.3pillarglobal.com/insights/exploring-the-different-types-of-nosql-databases)

- [RDBMS、NoSQL、およびニューSQLデータベース。ジョン・ライアンインタビュー](http://www.odbms.org/blog/2018/03/on-rdbms-nosql-and-newsql-databases-interview-with-john-ryan/)
  
- [SQL 対 NoSQL 対 新しい SQL: 完全な比較](https://www.xenonstack.com/blog/sql-vs-nosql-vs-newsql/)

- [DASH: クベルネテス-ネイティブデータベースの4つのプロパティ](https://thenewstack.io/dash-four-properties-of-kubernetes-native-databases/)

- [ゴキブリDB](https://www.cockroachlabs.com/)

- [ティジド](https://pingcap.com/en/)

- [ユガバイトDB](https://www.yugabyte.com/)

- [ヴィテス](https://vitess.io/)

- [Elasticsearch: 決定版ガイド](http://shop.oreilly.com/product/0636920028505.do)
  
- [アパッチ・ルセンの紹介](https://www.baeldung.com/lucene)

>[!div class="step-by-step"]
>[前次](azure-caching.md)
>[Next](resiliency.md) <!-- Next Chapter -->
