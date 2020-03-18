---
title: コンテナー設計の共通原則
description: コンテナーでは 1 つのプロセスのみをホストする必要があるという、適切なコンテナー設計の基本原則について学習します。
ms.date: 02/15/2019
ms.openlocfilehash: 69f3ff6c9303f0c4082695d861a8c90031295b6a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "68672499"
---
# <a name="common-container-design-principles"></a>コンテナー設計の共通原則

開発プロセスに進む前に、コンテナーの使用方法について言及すべき基本概念がいくつかあります。

## <a name="container-equals-a-process"></a>コンテナーはプロセスと等しい

コンテナー モデルでは、コンテナーは単一のプロセスを表します。 コンテナーをプロセス境界として定義することで、プロセスのスケーリング、またはバッチ処理に使用されるプリミティブの作成を開始します。 Docker コンテナーを実行すると、[ENTRYPOINT](https://docs.docker.com/engine/reference/builder/#/entrypoint) 定義が表示されます。 これでプロセスとコンテナーの有効期間が定義されます。 プロセスが完了すると、コンテナーのライフサイクルが終了します。 Web サーバーなどの実行時間の長いプロセス、およびバッチ ジョブなどの短期間のプロセスがあります。これらは、Microsoft Azure [WebJobs](https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/) として実装されている可能性があります。 プロセスが失敗した場合、コンテナーが終了し、Orchestrator が引き継ぎます。 オーケストレーターが 5 つのインスタンスの実行を維持するように指示されており、1 つが失敗した場合、オーケストレーターでは、失敗したプロセスを置換する別のコンテナーが作成されます。 バッチ ジョブで、プロセスはパラメーターを指定して開始されます。 プロセスが完了すると、作業が完了します。

単一のコンテナーで複数のプロセスを実行する必要があるシナリオが見られる場合があります。 どのアーキテクチャ ドキュメントでも、絶対に "絶対" は存在せず、また、いつも "いつも" は存在しません。 複数のプロセスを必要とするシナリオの場合、一般的なパターンは[スーパーバイザー](http://supervisord.org/)を使用することです。

>[!div class="step-by-step"]
>[前へ](design-docker-applications.md)
>[次へ](monolithic-applications.md)
