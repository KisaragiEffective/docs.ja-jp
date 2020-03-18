---
title: コンテナーとコンテナー オーケストレーターの活用
description: Azure での Docker コンテナーと Kubernetes Orchestrators 活用
ms.date: 06/30/2019
ms.openlocfilehash: 7b136ed2760ea471f42ff82d20298ff8714c6dee
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73841763"
---
# <a name="leveraging-containers-and-orchestrators"></a>コンテナーとコンテナー オーケストレーターの活用

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

コンテナーとオーケストレーター、モノリシックデプロイアプローチに共通する問題を解決するように設計されています。

## <a name="challenges-with-monolithic-deployments"></a>モノリシックデプロイに関する課題

従来、ほとんどのアプリケーションは1つのユニットとして展開されていました。 このようなアプリケーションは、モノリスと呼ばれます。 図3-1 に示すように、複数のモジュールまたはアセンブリで構成されている場合でも、アプリケーションを1つの単位としてデプロイする一般的な方法を次に示します。

![モノリシックアーキテクチャ。](./media/monolithic-architecture.png)

**図 3-1**. モノリシックアーキテクチャ。

単純化の利点はありますが、モノリシックアーキテクチャはいくつかの課題に直面します。

### <a name="deployments"></a>展開

モノリシックアプリケーションに配置するには、通常、1つの小さいモジュールだけを置換する場合でも、アプリケーション全体を再起動する必要があります。 アプリケーションをホストしているマシンの数によっては、デプロイ中にダウンタイムが発生する可能性があります。

### <a name="hosting"></a>ホスティング

モノリシックアプリケーションは、1台のコンピューターインスタンスで完全にホストされます。 これには、分散アプリケーションのどのモジュールよりも高い機能を必要とするハードウェアが必要になる場合があります。 また、アプリのいずれかの部分がボトルネックになった場合は、スケールアウトするために、アプリケーション全体を追加のマシンノードにデプロイする必要があります。

### <a name="environment"></a>環境

モノリシックアプリケーションは、通常、既存のホスティング環境 (オペレーティングシステム、インストールされているフレームワークなど) にデプロイされます。 この環境は、アプリケーションが開発またはテストされた環境と一致しない可能性があります。 アプリケーションの環境での不整合は、モノリシック配置における一般的な問題の原因です。

### <a name="coupling"></a>結合

モノリシックアプリケーションでは、アプリケーションのさまざまな部分と、アプリケーションとその環境との間に大きな結合が行われる可能性があります。 これにより、別の実装でスケーラビリティやスワップを向上させるために、特定のサービスまたは問題を後で考慮することが困難になる可能性があります。 この結合によって、システムへの変更に対する潜在的な影響も大きくなるため、大規模なアプリケーションで広範囲にわたるテストが必要になります。

### <a name="technology-choice"></a>テクノロジの選択

モノリシックアプリケーションは、1つの単位としてビルドおよび展開されます。 これにより、シンプルさと統一性が提供されますが、イノベーションの妨げになる可能性があります。 システムの新しい機能やモジュールは、より新しいプラットフォームまたはフレームワークに適している場合がありますが、一貫性を確保し、開発とデプロイを容易にするために、アプリケーションの現在のアプローチを使用して構築される可能性があります。

## <a name="what-are-the-benefits-of-containers-and-orchestrators"></a>コンテナーと orchestrators はどのような利点がありますか。

Docker は、最も一般的なコンテナー管理およびイメージングプラットフォームで、Linux および Windows 上のコンテナーをすばやく操作できます。 コンテナーは、任意のシステムで同じように動作する、再現可能な別のアプリケーション環境を提供します。 これにより、クラウドネイティブアプリケーションでのアプリケーションとアプリコンポーネントの開発とホストに最適です。 コンテナーは相互に分離されているため、同じホストハードウェア上の2つのコンテナーが異なるバージョンのソフトウェアとオペレーティングシステムをインストールでき、依存関係によって競合が発生することはありません。

さらに、コンテナーは、ソース管理にチェックインできる単純なファイルによって定義されます。 完全なサーバーとは異なり、更新プログラムの適用や追加のサービスのインストールを手動で行う必要がある仮想マシンであっても、コンテナーのインフラストラクチャを簡単にバージョン管理できます。 したがって、コンテナーで実行するように構築されたアプリは、ビルドパイプラインの一部として自動化ツールを使用して開発、テスト、および配置できます。

コンテナーは変更できません。 コンテナーの定義を作成したら、そのコンテナーを再作成して、まったく同じ方法で実行することができます。 この不変性は、コンポーネントベースの設計に役立ちます。 アプリケーションの一部が他の部分ほど頻繁に変更されない場合、最も頻繁に変更される部分を展開するだけで、アプリ全体を再展開するのはなぜですか。 アプリのさまざまな機能と横断的な問題を別々の単位に分割できます。 図3-2 は、モノリシックアプリが特定の機能を委任することによってコンテナーとマイクロサービスを利用する方法を示しています。 アプリ自体のその他の機能もコンテナー化されています。

バックエンドでマイクロサービスを使用するためのモノリシックアプリの分割 ![ます。](./media/breaking-up-monolith-with-backend-microservices.png)
**図 3-2**。 バックエンドでマイクロサービスを使用するようにモノリシックアプリを分割します。

個別のコンテナーを使用して構築されたクラウドネイティブアプリは、必要に応じてアプリケーションをほとんどまたはほとんど配置できます。 個々のサービスは、各サービスに適切なリソースを持つノードでホストできます。 各サービスが実行される環境は不変であり、開発、テスト、および運用の間で共有でき、簡単にバージョン管理できます。 アプリケーションのさまざまな領域間の結合は、モノリス内のコンパイル時の依存関係ではなく、サービス間の呼び出しまたはメッセージとして明示的に行われます。 また、アプリ全体の任意の部分で、アプリの残りの部分を変更することなく、その機能に最も適したテクノロジを選択できます。

## <a name="what-are-the-scaling-benefits"></a>スケーリングにはどのような利点がありますか。

コンテナー上に構築されたサービスは、Kubernetes などのオーケストレーションツールによって提供されるスケーリングの利点を活用できます。 設計コンテナーによって認識されるのは、それ自体だけです。 複数のコンテナーを連携させる必要がある場合は、より高いレベルでそれらを整理することをお勧めします。 多数のコンテナーとその共有依存関係 (ネットワーク構成など) を整理すると、その日を節約するためにオーケストレーションツールが提供されます。 Kubernetes は、コンテナー化アプリケーションのデプロイ、スケーリング、および管理を自動化するように設計されたコンテナーオーケストレーションプラットフォームです。 コンテナーのグループの上に抽象化レイヤーを作成し、*ポッド*に整理します。 ポッドは、*ノード*と呼ばれるワーカーマシン上で実行されます。 整理されたグループ全体は*クラスター*と呼ばれます。 図3-3 は、Kubernetes クラスターのさまざまなコンポーネントを示しています。

Kubernetes クラスターコンポーネントを ![します。](./media/kubernetes-cluster-components.png)
**図 3-3**。 クラスターコンポーネントを Kubernetes します。

Kubernetes には、需要に合わせてクラスターをスケーリングするためのサポートが組み込まれています。 コンテナー化されたマイクロサービスと組み合わせることで、クラウドネイティブアプリケーションは、必要に応じて追加のリソースを使用して、需要の急増に迅速かつ効率的に対応できるようになります。

### <a name="declarative-versus-imperative"></a>宣言型と命令型

Kubernetes は、宣言型と命令型の両方のオブジェクト構成をサポートしています。 命令型のアプローチには、各手順の実行方法を Kubernetes に指示するさまざまなコマンドを実行する必要があります。 このイメージを*実行*します。 このポッドを*削除*します。 このポートを*公開*します。 宣言型の方法では、*何を実行*するかについてではなく、*目的の内容*を記述した構成ファイルを使用し、目的の終了状態を達成するために何をするかを Kubernetes します。 命令型コマンドを使用して既にクラスターを構成している場合は、`kubectl get svc SERVICENAME -o yaml > service.yaml`を使用して宣言マニフェストをエクスポートできます。 これにより、次のようなマニフェストファイルが生成されます。

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: "2019-09-13T13:58:47Z"
  labels:
    component: apiserver
    provider: kubernetes
  name: kubernetes
  namespace: default
  resourceVersion: "153"
  selfLink: /api/v1/namespaces/default/services/kubernetes
  uid: 9b1fac62-d62e-11e9-8968-00155d38010d
spec:
  clusterIP: 10.96.0.1
  ports:
  - name: https
    port: 443
    protocol: TCP
    targetPort: 6443
  sessionAffinity: None
  type: ClusterIP
status:
  loadBalancer: {}
```

宣言型の構成を使用する場合は、構成ファイルが配置されているフォルダーに対して `kubectl diff -f FOLDERNAME` を使用してコミットする前に、変更をプレビューできます。 変更を適用することを確認したら、`kubectl apply -f FOLDERNAME`を実行します。 フォルダー階層を再帰的に処理する `-R` を追加します。

サービスに加えて、*配置*などの他の Kubernetes 機能に対して宣言型の構成を使用することもできます。 宣言型の配置は、配置コントローラーがクラスターリソースを更新するために使用されます。 デプロイは、新しい変更のロールアウト、負荷の増加に対応するためのスケールアップ、または以前のリビジョンへのロールバックに使用されます。 クラスターが不安定な場合、宣言型のデプロイによって、クラスターを自動的に目的の状態に戻すメカニズムが提供されます。

宣言型の構成を使用すると、アプリケーションコードと共にチェックインおよびバージョン管理できるコードとして、インフラストラクチャを表すことができます。 これにより、変更制御が改善され、ソース管理の変更に関連付けられたビルドおよび配置パイプラインを使用した継続的配置のサポートが向上します。

## <a name="what-scenarios-are-ideal-for-containers-and-orchestrators"></a>コンテナーと orchestrators 最適なシナリオ

次のシナリオは、コンテナーと orchestrators 使用する場合に最適です。

### <a name="applications-requiring-high-uptime-and-scalability"></a>高いアップタイムとスケーラビリティを必要とするアプリケーション

アップタイムとスケーラビリティの要件が高い個々のアプリケーションは、マイクロサービス、コンテナー、および orchestrators 使用したクラウドネイティブアーキテクチャに最適です。 これらのアプリケーションは、バージョン管理された環境を使用してコンテナーで開発でき、運用環境に移行する前に広範囲にわたってテストでき、ダウンタイムなしで運用環境にデプロイできます。 Kubernetes クラスターを使用することにより、このようなアプリはオンデマンドでスケールしたり、ノードの障害から自動的に回復したりすることができます。

### <a name="large-numbers-of-applications"></a>多数のアプリケーション

その後、多数のアプリケーションを展開する組織は、コンテナーと orchestrators 利用することでメリットを得ることができます。 コンテナー化された環境と Kubernetes クラスターを設定する前の作業は、主に固定コストです。 個々のアプリケーションの配置、保守、および更新には、管理する必要があるアプリケーションの数に応じてコストがかかります。 ごく少数のアプリケーションを除き、カスタムアプリケーションを手動で管理することの複雑さは、コンテナーと orchestrators 使用したソリューションの実装コストを超過します。

## <a name="when-should-you-avoid-using-containers-and-orchestrators"></a>コンテナーと orchestrators 使用する必要があるのはどのような場合ですか。

12要素アプリの原則に従ってアプリケーションを構築できない場合、または、コンテナーと orchestrators 避けることをお勧めします。 このような場合は、先に進むことが最適な場合があります。また、場合によっては、特定の機能を別のコンテナーまたはサーバーレス機能に移すことができるハイブリッドシステムを使用することもできます。

## <a name="development-resources"></a>開発リソース

このセクションでは、次のアプリケーションでコンテナーとオーケストレーター使用を開始する際に役立つ、開発リソースの簡単な一覧を示します。 クラウドネイティブマイクロサービスアーキテクチャアプリを設計する方法に関するガイダンスについては、「 [.Net マイクロサービス: コンテナー化された .Net アプリケーションのアーキテクチャ](https://aka.ms/microservicesebook)」を参照してください。

### <a name="local-kubernetes-development"></a>ローカル Kubernetes の開発

Kubernetes のデプロイは運用環境での優れた価値を提供しますが、ローカルで実行することもできます。 個々のアプリまたはマイクロサービスで個別に作業できるのはかなりの時間ですが、運用環境にデプロイしたときに実行するのと同じように、システム全体をローカルで実行できることが適切な場合もあります。 これを実現するにはいくつかの方法があります。2つは Minikube と Docker Desktop です。 Visual Studio には、Docker 開発用のツールも用意されています。

### <a name="minikube"></a>Minikube

Minikube とは Minikube プロジェクトでは、"Minikube は macOS、Linux、および Windows でローカルの Kubernetes クラスターを実装しています" と表示されます。 主な目的は、"ローカル Kubernetes アプリケーションの開発に最適なツールであり、Kubernetes のすべての機能をサポートすることです。" ということです。 Minikube のインストールは Docker とは分離されていますが、Minikube は Docker デスクトップでサポートされるのとは異なるハイパーバイザーをサポートします。 現在、Minikube では次の Kubernetes 機能がサポートされています。

- DNS
- NodePorts
- ConfigMaps とシークレット
- ダッシュボード
- コンテナーランタイム: Docker、rkt、CRI、および格納されている
- コンテナーネットワークインターフェイスを有効にする (CNI)
- フィルタリング

Minikube をインストールしたら、イメージをダウンロードしてローカル Kubernetes クラスターを開始する `minikube start` コマンドを実行して、すぐに使用を開始できます。 クラスターを起動したら、標準の Kubernetes `kubectl` コマンドを使用して、クラスターと対話します。

### <a name="docker-desktop"></a>Docker デスクトップ

Windows の Docker デスクトップから直接 Kubernetes を使用することもできます。 これは、Windows コンテナーを使用している場合の唯一のオプションであり、Windows 以外のコンテナーにも適しています。 Docker Desktop から実行される Kubernetes を構成するには、標準の Docker デスクトップ構成アプリを使用します。

![Docker Desktop での Kubernetes の構成](./media/docker-desktop-kubernetes.png)

**図 3-4**. Docker Desktop で Kubernetes を構成しています。

Docker Desktop は、コンテナー化されたアプリをローカルで構成して実行するための最も一般的なツールです。 Docker Desktop を使用する場合は、実稼働環境にデプロイする Docker コンテナーイメージのまったく同じセットに対してローカルで開発できます。 Docker Desktop は、コンテナー化されたアプリをローカルで "ビルド、テスト、および出荷" するように設計されています。 イメージが Azure Container Registry や Docker Hub などのイメージレジストリに配布されると、Azure Kubernetes Service (AKS) などのサービスは運用環境でアプリケーションを管理します。

### <a name="visual-studio-docker-tooling"></a>Visual Studio Docker ツール

Visual Studio では、web アプリケーションの Docker 開発をサポートしています。 新しい ASP.NET Core アプリケーションを作成するときに、図3-5 に示すように、プロジェクト作成プロセスの一部として Docker サポートを使用して構成するオプションが表示されます。

![Visual Studio による Docker サポートの有効化](./media/visual-studio-enable-docker-support.png)

**図 3-5**. Visual Studio による Docker サポートの有効化

このオプションを選択すると、プロジェクトはルートに `Dockerfile` を使用して作成され、Docker コンテナーでアプリをビルドしてホストするために使用できます。 図3-6 に、Dockerfile の例を示します。

```docker
FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-stretch-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.0-stretch AS build
WORKDIR /src
COPY ["WebApplication3/WebApplication3.csproj", "WebApplication3/"]
RUN dotnet restore "WebApplication3/WebApplication3.csproj"
COPY . .
WORKDIR "/src/WebApplication3"
RUN dotnet build "WebApplication3.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "WebApplication3.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "WebApplication3.dll"]
```

**図 3-6**. Visual Studio によって生成された Dockerfile

アプリを実行するときの既定の動作は、Docker も使用するように構成されます。 図3-7 は、Docker サポートを追加して作成された新しい ASP.NET Core プロジェクトで使用できるさまざまな実行オプションを示しています。

![Visual Studio Docker の実行オプション](./media/visual-studio-docker-run-options.png)

**図 3-7**. Visual Studio Docker の実行オプション

[Azure Dev Spaces](https://docs.microsoft.com/azure/dev-spaces/)には、ローカル開発に加えて、複数の開発者が Azure 内で独自の Kubernetes 構成を操作するための便利な方法が用意されています。 図3-7 に示すように、Azure Dev Spaces でアプリケーションを実行することもできます。

作成時に ASP.NET Core アプリケーションに Docker サポートを追加しない場合は、後でいつでも追加できます。 図3-8 に示すように、Visual Studio ソリューションエクスプローラーからプロジェクトを右クリックし、[Add > Docker Support] \ ( **Docker サポート**の**追加**\) を選択します。

![Visual Studio による Docker サポートの追加](./media/visual-studio-add-docker-support.png)

**図 3-8**. Visual Studio による Docker サポートの追加

Docker サポートに加えて、図3-8 に示すコンテナーオーケストレーションのサポートも追加できます。 既定では、orchestrator は Kubernetes とヘルムを使用します。 Orchestrator を選択すると、`azds.yaml` ファイルがプロジェクトルートに追加され、アプリケーションを構成して Kubernetes に配置するために使用されるヘルムグラフを含む `charts` フォルダーが追加されます。 図3-9 に、新しいプロジェクトで生成されるファイルを示します。

![Visual Studio の Orchestrator サポートの追加](./media/visual-studio-add-orchestrator-support.png)

**図 3-9**. Visual Studio の Orchestrator サポートの追加

## <a name="references"></a>参照

- [Kubernetes とは](https://blog.newrelic.com/engineering/what-is-kubernetes/)
- [Minikube を使用した Kubernetes のインストール](https://kubernetes.io/docs/setup/learning-environment/minikube/)
- [MiniKube vs Docker デスクトップ](https://medium.com/containers-101/local-kubernetes-for-windows-minikube-vs-docker-desktop-25a1c6d3b766)
- [Visual Studio Tools for Docker](https://docs.microsoft.com/dotnet/standard/containerized-lifecycle-architecture/design-develop-containerized-apps/visual-studio-tools-for-docker)

>[!div class="step-by-step"]
>[前へ](scale-applications.md)
>[次へ](leverage-serverless-functions.md)
