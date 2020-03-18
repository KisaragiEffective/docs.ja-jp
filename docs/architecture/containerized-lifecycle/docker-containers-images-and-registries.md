---
title: Docker コンテナー、イメージ、レジストリ
description: Docker のアプリケーションのデプロイ方法において、レジストリが全体的に果たす主な役割について説明します。
ms.date: 02/15/2019
ms.openlocfilehash: bfef21cab7be89abaf33b89366d7cff2115a7cc6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72770930"
---
# <a name="docker-containers-images-and-registries"></a>Docker コンテナー、イメージ、レジストリ

Docker を使用する場合、アプリケーションやサービスを作成し、それとその依存関係をコンテナー イメージにパッケージ化します。 イメージとは、アプリケーションまたはサービスと、その構成と依存関係の静的な表現です。

アプリケーションまたはサービスを実行するために、アプリケーションのイメージはインスタンス化され、コンテナーが作成されます。このコンテナーが Docker ホスト上で実行されます。 コンテナーは、最初に開発環境または PC でテストされます。

イメージをレジストリに保存します。レジストリはイメージのライブラリとして機能します。 実稼働環境のオーケストレーターにデプロイする場合にレジストリが必要です。 Docker は [Docker Hub](https://hub.docker.com/) 経由で公開レジストリを保守します。他のベンダーは、[Azure Container Registry](https://azure.microsoft.com/services/container-registry/)を含むさまざまなイメージのコレクション用のレジストリを用意しています。 また、企業は自社の Docker イメージ用に非公開のレジストリをオンプレミスに持つことができます。

図 1-4 は、Docker のイメージとレジストリが他のコンポーネントとどのように関係しているかを示しています。 また、ベンダーから提供される複数のレジストリも示しています。

![Docker の基本的な分類を示す図。](./media/docker-containers-images-and-registries/taxonomy-docker-terms-concepts.png)

**図 1-4** Docker の用語と概念の分類

レジストリはイメージが格納される本棚のようなものであり、サービスや Web アプリを実行するコンテナーを構築するためにプルして使用できます。 オンプレミスとパブリック クラウド上にプライベート Docker レジストリがあります。 Docker Hub はエンタープライズ級のソリューションである Docker Trusted Registry と共に Docker によって管理されるパブリック レジストリであり、Azure では Azure Container Registry を提供しています。 AWS や Google などにもコンテナー レジストリがあります。

レジストリにイメージを配置することによって、フレームワーク レベルで、すべての依存関係を含む静的で不変なアプリケーション ビットを格納できます。 それにより、複数の環境でイメージをバージョン管理し、デプロイできるため、一貫したデプロイ ユニットを提供できます。

オンプレミスまたはクラウドでホストされる非公開のイメージ レジストリは、次の場合に推奨されます。

- 機密保持のためにイメージを一般公開して共有することができない場合。

- イメージと選択した展開環境間のネットワーク待機時間を最小限に抑えたい場合。 たとえば、実稼働環境が Azure の場合は、[Azure Container Registry](https://azure.microsoft.com/services/container-registry/) にイメージを保存して、ネットワークの待機時間を最小限に抑える方法があります。 同様に、実稼働環境がオンプレミスの場合は、同じローカル ネットワーク内でオンプレミスの Docker Trusted Registry を使用できるようにする方法があります。

>[!div class="step-by-step"]
>[前へ](docker-terminology.md)
>[次へ](road-to-modern-applications-based-on-containers.md)
