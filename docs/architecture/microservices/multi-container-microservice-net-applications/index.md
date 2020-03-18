---
title: 複数のコンテナーとマイクロサービス ベースの NET アプリケーションのデザインと開発
description: '.NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ | 複数のコンテナーとマイクロサービス ベースの NET アプリケーションをデザインおよび開発するための外部アーキテクチャを理解します。'
ms.date: 10/02/2018
ms.openlocfilehash: 8c2f828e9913a0efcdf580371124b0f624daeffe
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "70295229"
---
# <a name="designing-and-developing-multi-container-and-microservice-based-net-applications"></a>複数のコンテナーとマイクロサービス ベースの NET アプリケーションのデザインと開発

*コンテナー化されたマイクロサービス アプリケーションを開発しているということは、複数コンテナーのアプリケーションを構築しているということです。ただし、複数コンテナーのアプリケーションは、単純である可能性があり (3 層アプリケーションなど)、マイクロサービス アーキテクチャを使って構築されない場合があります。*

以前に "マイクロサービス アーキテクチャを構築するときに Docker は必要か?" という疑問を提起しました。 その答えは、明らかに "いいえ" です。 Docker は拡張機能であり、大きな利点を提供できますが、コンテナーおよび Docker はマイクロサービスのハード要件ではありません。 たとえば、簡単なプロセスまたは Docker コンテナーとして実行されているマイクロサービスをサポートする、Azure Service Fabric を使用している場合、Docker の有無にかかわらず、マイクロサービス ベースのアプリケーションを作成できます。

ただし、Docker コンテナーにも基づくマイクロサービス ベースのアプリケーションをデザインおよび開発する方法を知っている場合、その他の単純なアプリケーション モデルをデザインおよび開発することができます。 たとえば、複数コンテナーの手法も必要とする、3 層のアプリケーションをデザインする可能性があります。 そのために、また、マイクロサービス アーキテクチャはコンテナーの世界で重要なトレンドであるため、このセクションでは、Docker コンテナーを使用するマイクロサービス アーキテクチャの実装に注目します。

>[!div class="step-by-step"]
>[前へ](../docker-application-development-process/docker-app-development-workflow.md)
>[次へ](microservice-application-design.md)
