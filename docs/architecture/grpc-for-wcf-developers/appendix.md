---
title: WCF 開発者向けの付録-gRPC
description: 最新のマイクロサービスアーキテクチャにおける分散トランザクションとその実装について説明します。
ms.date: 09/02/2019
ms.openlocfilehash: 9931681727f921e007c2f80852ad0e69cd7288de
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74711468"
---
# <a name="appendix-a---transactions"></a>付録 A-トランザクション

Windows Communication Foundation (WCF) では分散トランザクションがサポートされているため、複数のサービスにまたがるアトミック操作を実行できます。 この機能は、 [Microsoft 分散トランザクションコーディネーター](https://docs.microsoft.com/previous-versions/windows/desktop/ms684146(v=vs.85))に基づいています。

新しいマイクロサービスのランドスケープでは、この種類の自動化された分散トランザクション処理はできません。 リレーショナルデータベース、NoSQL データストア、メッセージングシステムなど、さまざまなテクノロジが関係しています。 また、1つの環境で使用されているオペレーティングシステム、プログラミング言語、フレームワークが混在している場合もあります。

WCF 分散トランザクションは、 [2 フェーズコミット (2pc)](https://en.wikipedia.org/wiki/Two-phase_commit_protocol)と呼ばれるものを実装したものです。 サービス間でメッセージを調整し、各サービス内で開いているトランザクションを作成し、成功または失敗に応じてコミットまたはロールバックメッセージを送信することにより、2PC トランザクションを手動で実装できます。 ただし、2PC の管理に伴う複雑さは、システムの進化に伴って急激に増加する可能性があります。 開いているトランザクションは、パフォーマンスに悪影響を及ぼす可能性のあるデータベースロックを保持するか、またはサービス間のデッドロックの原因となります。

可能な場合は、分散トランザクションを完全に回避することをお勧めします。 データの2つの項目が、アトミック更新を必要とするようにリンクされている場合は、それらを同じサービスで処理することを検討してください。 単一の要求またはメッセージをそのサービスに対して使用して、これらのアトミックな変更を適用します。

これが不可能な場合は、別の方法として、 [Saga パターン](https://microservices.io/patterns/data/saga.html)を使用する方法があります。 Saga では、更新は順番に処理されます。各更新が成功すると、次の更新がトリガーされます。 これらのトリガーは、サービスからサービスに伝達することも、saga コーディネーターまたは orchestrator で管理することもできます。 プロセスのどの時点でも更新が失敗した場合、更新を既に完了しているサービスでは、元に戻すための特定のロジックが適用されます。

また、「 [.Net マイクロサービスの電子書籍](https://docs.microsoft.com/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/)」で説明されているように、ドメイン駆動設計 (DDD) とコマンド/クエリ責務分離 (CQRS) を使用する方法もあります。 特に、ドメインイベントまたは[イベントソーシング](https://martinfowler.com/eaaDev/EventSourcing.html)を使用すると、直ちに更新が適用されることを保証するのに役立ちます。

>[!div class="step-by-step"]
>[前へ](application-performance-management.md)
