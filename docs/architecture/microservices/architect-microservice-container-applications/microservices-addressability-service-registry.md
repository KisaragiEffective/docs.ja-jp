---
title: マイクロサービスのアドレス指定能力およびサービス レジストリ
description: マイクロサービス アーキテクチャ内のコンテナー イメージ レジストリの役割を理解します。
ms.date: 09/20/2018
ms.openlocfilehash: d72ba399f3da730f0e57c44c5ec01c1cc9f5fc05
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "68674049"
---
# <a name="microservices-addressability-and-the-service-registry"></a>マイクロサービスのアドレス指定能力およびサービス レジストリ

各マイクロサービスには、その場所を解決するために使用する一意の名前 (URL) が付けられています。 ご利用のマイクロサービスは、その実行場所にかかわらず、アドレス指定可能である必要があります。 特定のマイクロサービスをどのコンピューターで実行するかについて考える必要がある場合、事態はすぐに悪化する可能性があります。 DNS が URL を特定のコンピューターに解決するのと同様に、マイクロサービスの現在の場所が探索できるようにマイクロサービスには一意の名前が割り当てられている必要があります。 マイクロサービスには、実行されているインフラストラクチャからマイクロサービスを独立させるためのアドレス指定可能な名前が必要です。 このことは、[サービス レジストリ](https://microservices.io/patterns/service-registry.html)が必要であるために、サービスの展開方法とサービスの検出方法との間には相互作用があることを意味しています。 同様に、コンピューターで障害が発生した場合、レジストリ サービスは、サービスが現在実行されている場所を示すことができなければなりません。

[サービス レジストリ パターン](https://microservices.io/patterns/service-registry.html)は、サービス検出の重要な部分です。 レジストリは、サービス インスタンスのネットワークの場所を含むデータベースです。 サービス レジストリは、可用性が高く、最新の状態である必要があります。 クライアントは、サービス レジストリから取得したネットワークの場所をキャッシュすることが可能です。 ただし、その情報は最終的には期限切れになり、クライアントはサービス インスタンスを検出できなくなります。 したがって、サービス レジストリは、レプリケーション プロトコルを使用して整合性を維持するサーバーのクラスターで構成されます。

一部のマイクロサービスの展開環境 (クラスターと呼ばれ、後のセクションで説明します) には、サービス検出が組み込まれています。 たとえば、Kubernetes (AKS) 環境での Azure Container Service では、サービス インスタンスの登録と登録解除を処理できます。 また、これはサーバー側の検出ルーターの役割を果たす各クラスター ホスト上でプロキシも実行します。

## <a name="additional-resources"></a>その他の技術情報

- **Chris Richardson。パターン: サービス レジストリ** \
  <https://microservices.io/patterns/service-registry.html>

- **Auth0。サービス レジストリ** \
  <https://auth0.com/blog/an-introduction-to-microservices-part-3-the-service-registry/>

- **Gabriel Schenker。サービス探索** \
  <https://lostechies.com/gabrielschenker/2016/01/27/service-discovery/>

>[!div class="step-by-step"]
>[前へ](maintain-microservice-apis.md)
>[次へ](microservice-based-composite-ui-shape-layout.md)
