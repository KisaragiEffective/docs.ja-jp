---
title: マイクロサービスで DDD と CQRS パターンを使ってビジネスの複雑さに取り組む
description: コンテナー化された .NET アプリケーションの .NET マイクロサービス アーキテクチャ | DDD と CQRS パターンを適用して複雑なビジネス シナリオに取り組む方法を理解する
ms.date: 10/08/2018
ms.openlocfilehash: 88b105b68307c8587f877bb9ddf370e143d8539b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73739841"
---
# <a name="tackle-business-complexity-in-a-microservice-with-ddd-and-cqrs-patterns"></a>マイクロサービスで DDD と CQRS パターンを使ってビジネスの複雑さに取り組む

*ビジネス ドメインの理解を反映するマイクロソフトサービスまたはコンテキスト境界ごとのドメイン モデルを設計する*

このセクションでは、複雑なサブシステムへの取り組みが必要な場合に実装する高度なマイクロサービスについて、またドメイン専門家の知識と絶えず変化するビジネス ルールに由来するマイクロサービスについて説明します。 このセクションで使用するアーキテクチャ パターンは、図 7-1 に示すように、ドメイン駆動設計 (DDD) とコマンドクエリ責務分離 (CQRS) の手法に基づいています。

:::image type="complex" source="./media/index/internal-versus-external-architecture.png" alt-text="外部アーキテクチャと内部アーキテクチャのパターンを比較した図。":::
外部アーキテクチャ (マイクロサービス パターン、API ゲートウェイ、回復力のある通信、pub/sub など) と、内部アーキテクチャ (データ駆動型/CRUD、DDD パターン、依存関係の挿入、複数のライブラリなど) の違い。
:::image-end:::

**図 7-1**。 外部マイクロサービス アーキテクチャとマイクロサービスごとの内部アーキテクチャ パターンとの対比

ただし、ASP.NET Core Web API サービスの実装方法や、Swashbuckle または NSwag を使った Swagger メタデータの公開方法など、データ駆動型マイクロサービスのテクニックのほとんどは、DDD パターンを使って内部的に実装される高度なマイクロサービスにも適用されます。 前述した実施方法のほとんどはここでも、または任意の種類のマイクロ サービスにも適用されるため、このセクションは前のセクションの内容を増補するものとなっています。

このセクションでは、まず eShopOnContainers 参照アプリケーションで使用される簡略化された CQRS パターンの詳細を示します。 後で DDD 手法の概要を説明しますが、そこでは、アプリケーションで再利用できる一般的なパターンを確認できます。

DDD は、学習用に豊富な技術資料が提供されている大きなテーマです。 入門書としては、Eric Evans 著の『[Domain-Driven Design](https://domainlanguage.com/ddd/)』 (ドメイン駆動設計)、さらに Vaughn Vernon、Jimmy Nilsson、Greg Young、Udi Dahan、Jimmy Bogard の各氏、およびその他多くの DDD/CQRS の専門家による技術資料などを利用できます。 ただし、DDD 手法の適用方法の習得には何より、具体的なビジネス ドメイン内で専門家との対話、ホワイトボードを使った議論、ドメイン モデリングのセッションを利用する試みが必要です。

#### <a name="additional-resources"></a>その他の技術情報

##### <a name="ddd-domain-driven-design"></a>DDD (ドメイン駆動設計)

- **Eric Evans。ドメイン言語** \
  <https://domainlanguage.com/>

- **Martin Fowler。ドメイン駆動設計** \
  <https://martinfowler.com/tags/domain%20driven%20design.html>

- **Jimmy Bogard。ドメインの強化: 入門** \
  <https://lostechies.com/jimmybogard/2010/02/04/strengthening-your-domain-a-primer/>

##### <a name="ddd-books"></a>DDD 関連の書籍

- **Eric Evans。Domain-Driven Design:Tackling Complexity in the Heart of Software (エリック・エヴァンスのドメイン駆動設計)**  \
  <https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/>

- **Eric Evans。Domain-Driven Design Reference: Definitions and Pattern Summaries (ドメイン駆動設計のリファレンス: 定義とパターンの概要)**  \
  <https://www.amazon.com/Domain-Driven-Design-Reference-Definitions-2014-09-22/dp/B01N8YB4ZO/>

- **Vaughn Vernon。Implementing Domain-Driven Design (実践ドメイン駆動設計)**  \
  <https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577/>

- **Vaughn Vernon。Domain-Driven Design Distilled (ドメイン駆動設計の基本)**  \
  <https://www.amazon.com/Domain-Driven-Design-Distilled-Vaughn-Vernon/dp/0134434420/>

- **Jimmy Nilsson。Applying Domain-Driven Design and Patterns (ドメイン駆動)**  \
  <https://www.amazon.com/Applying-Domain-Driven-Design-Patterns-Examples/dp/0321268202/>

- **Cesar de la Torre。N-Layered Domain-Oriented Architecture Guide with .NET (.NET による N 層ドメイン指向アーキテクチャ ガイド)**  \
  <https://www.amazon.com/N-Layered-Domain-Oriented-Architecture-Guide-NET/dp/8493903612/>

- **Abel Avram および Floyd Marinescu。Domain-Driven Design Quickly (ドメイン駆動設計簡易ガイド)**  \
  <https://www.amazon.com/Domain-Driven-Design-Quickly-Abel-Avram/dp/1411609255/>

- **Scott Millett、Nick Tune - Patterns, Principles, and Practices of Domain-Driven Design (ドメイン駆動設計のパターン、原則、実践)**  \
  <http://www.wrox.com/WileyCDA/WroxTitle/Patterns-Principles-and-Practices-of-Domain-Driven-Design.productCd-1118714709.html>

##### <a name="ddd-training"></a>DDD に関するトレーニング

- **Julie Lerman および Steve Smith。Domain-Driven Design Fundamentals (ドメイン駆動設計の基礎)**  \
  <https://bit.ly/PS-DDD>

>[!div class="step-by-step"]
>[前へ](../multi-container-microservice-net-applications/implement-api-gateways-with-ocelot.md)
>[次へ](apply-simplified-microservice-cqrs-ddd-patterns.md)
