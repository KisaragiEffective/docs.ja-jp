---
title: Docker の概要
description: この記事では、.NET Core アプリケーションのコンテキストでの Docker の基本と概要について説明します。
ms.date: 03/20/2019
ms.custom: mvc
ms.openlocfilehash: eedfd1e7c1b361beb9d4f271e739657ef5e894a6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78157792"
---
# <a name="introduction-to-net-and-docker"></a>.NET および Docker の概要

.NET Core は Docker コンテナー内で簡単に実行できます。 コンテナーは、システムの残りの部分からアプリケーションを分離するための簡単な方法です。カーネルのみを共有し、アプリケーションに提供されたリソースを使います。 Docker を初めて使用する場合は、Docker の[概要ドキュメント](https://docs.docker.com/engine/docker-overview/)に関するページを読むことを強くお勧めします。

Docker をインストールする方法について詳しくは、次のダウンロード ページをご覧ください: [Docker Desktop:Community Edition](https://www.docker.com/products/docker-desktop)。

## <a name="docker-basics"></a>Docker の基礎

いくつかの概念を理解しておく必要があります。 Docker クライアントには、イメージとコンテナーの管理に使用できる CLI が用意されています。 前述のように、[Docker の概要](https://docs.docker.com/engine/docker-overview/)に関するドキュメントを読むための時間を取ることをお勧めします。

### <a name="images"></a>イメージ

イメージは、コンテナーの基礎を形成する、ファイル システムの変更の順序付きコレクションです。 イメージは状態を持たず、読み取り専用です。 多くの場合、イメージは別のイメージに基づき、いくつかのカスタマイズを含んでいます。 たとえば、アプリケーションの新しいイメージは、.NET Core ランタイムが既に含まれている既存のイメージに基づいて作成します。

コンテナーはイメージから作成されるため、イメージにはコンテナーの起動時に実行される一連の実行パラメーター (開始実行可能ファイルなど) が含まれています。

### <a name="containers"></a>コンテナー

コンテナーは、イメージの実行可能なインスタンスです。 イメージをビルドするときに、アプリケーションと依存関係をデプロイします。 これで、それぞれが互いに分離した複数のコンテナーをインスタンス化できます。 各コンテナー インスタンスには、独自のファイル システム、メモリ、およびネットワーク インターフェイスが含まれます。

### <a name="registries"></a>レジストリ

コンテナー レジストリはイメージ リポジトリのコレクションです。 レジストリのイメージに基づいて自分のイメージを作成できます。 レジストリ内のイメージから直接コンテナーを作成できます。 [Docker コンテナー、イメージ、およびレジストリの相互の関係](../../architecture/microservices/container-docker-introduction/docker-containers-images-registries.md)は、[コンテナー化されたアプリケーションやマイクロサービスの設計および構築](../../architecture/microservices/architect-microservice-container-applications/index.md)を行う際に重要となる概念です。 この手法を用いると開発および配置にかかる時間を大幅に短縮できます。

Docker には、自分が使用できる [Docker Hub](https://hub.docker.com/) でホストされているパブリック レジストリが含まれます。 [.NET Core 関連イメージ](https://hub.docker.com/_/microsoft-dotnet-core/)は Docker Hub に一覧表示されます。

Microsoft Container Registry (MCR) は、Microsoft が提供するコンテナー イメージの公式ソースです。 MCR は Azure CDN に基づいて構築され、グローバルにレプリケートされたイメージを備えています。 ただし、MCR には一般公開された Web サイトがありません。Microsoft が提供するコンテナー イメージについて学習する主な方法は、[Microsoft Docker Hub のページ](https://hub.docker.com/_/microsoft-dotnet-core/)を使うことです。

### <a name="dockerfile"></a>Dockerfile

**Dockerfile** は、イメージを作成する一連の手順を定義したファイルです。 **Dockerfile** 内の各手順で、イメージ内のレイヤーを作成します。 ほとんどの場合、イメージをリビルドすると、変更されたレイヤーのみがリビルドされます。 **Dockerfile** は他のユーザーに配布できます。他のユーザーは、これを使って、自分が作成したときと同じ方法で新しいイメージを再作成できます。 これにより、イメージを作成する方法に関する "*手順*" を配布できますが、自分のイメージを配布するための主要な方法はそれをレジストリに公開することです。

## <a name="net-core-images"></a>.NET Core イメージ

公式の .NET Core Docker イメージは Microsoft Container Registry (MCR) に公開され、[Microsoft .NET Core の Docker Hub リポジトリ](https://hub.docker.com/_/microsoft-dotnet-core/)で見つけられます。 各リポジトリには、.NET (SDK またはランタイム) と自分が使用できる OS のさまざまな組み合わせのイメージが含まれています。

Microsoft は、特定のシナリオに対応したイメージを用意しています。 たとえば、[ASP.NET Core リポジトリ](https://hub.docker.com/_/microsoft-dotnet-core-aspnet/)には、運用環境での ASP.NET Core アプリの実行用にビルドされたイメージが用意されています。

## <a name="azure-services"></a>Azure サービス

さまざまな Azure サービスがコンテナーに対応しています。 アプリケーションの Docker イメージを作成して、次のサービスのいずれかにデプロイできます。

- [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/)\
Kubernetes を使用して Linux コンテナーのスケールと調整を行います。

- [Azure App Service](https://azure.microsoft.com/services/app-service/containers/)\
PaaS 環境で Linux コンテナーを使用して Web アプリまたは API をデプロイします。

- [Azure Container Instances](https://azure.microsoft.com/services/container-instances/)\
クラウドでコンテナーをホストしますが、高度な管理サービスは何もありません。

- [Azure Batch](https://azure.microsoft.com/services/batch/)\
コンテナーを使用して反復的なコンピューティング ジョブを実行します。

- [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/)\
Windows Server コンテナーを使用して、.NET アプリケーションをマイクロサービスにリフト、シフト、および最新化します。

- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)\
すべての種類の Azure デプロイでコンテナー イメージを格納し、管理します。

## <a name="next-steps"></a>次の手順

- [.NET Core アプリケーションをコンテナー化する方法について説明します。](build-container.md)
- [ASP.NET Core アプリケーションをコンテナー化する方法を学習します。](/aspnet/core/host-and-deploy/docker/building-net-docker-images)
- [Learn の ASP.NET Core マイクロサービスのチュートリアルをお試しください。](https://dotnet.microsoft.com/learn/web/aspnet-microservice-tutorial/intro)
- [Visual Studio でのコンテナー ツールについて学習します](/visualstudio/containers/overview)
