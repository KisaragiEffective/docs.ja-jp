---
title: Docker アプリの開発ワークフロー
description: Docker ベースのアプリケーションを開発するためのワークフローの詳細を理解します。 まず、段階的に見ていき、Dockerfile の最適化について詳しく確認し、最終的には Visual Studio を使用する際に利用できる簡略化されたワークフローを理解します。
ms.date: 01/30/2020
ms.openlocfilehash: c58ea2436027968143777a19286a1a0a72107717
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79401509"
---
# <a name="development-workflow-for-docker-apps"></a>Docker アプリの開発ワークフロー

アプリケーション開発のライフ サイクルは、開発者のコンピューターから始まります。そこで、開発者の好みの言語を使用してアプリケーションをコーディングし、ローカルでテストします。 このワークフローでは、選択した言語、フレームワークおよびプラットフォームにかかわらず、常に Docker コンテナーをローカルで開発およびテストします。

各コンテナー (Docker イメージのインスタンス) には、次のコンポーネントが含まれています。

- 選択したオペレーティング システム (Linux ディストリビューション、Windows Nano Server、Windows Server Core など)。

- 開発時に追加したファイル (ソース コードおよびアプリケーション バイナリなど)。

- 構成情報 (環境の設定および依存関係など)。

## <a name="workflow-for-developing-docker-container-based-applications"></a>Docker のコンテナー ベースのアプリケーションを開発するためのワークフロー

このセクションでは、Docker のコンテナー ベースのアプリケーションの*内側のループ*の開発ワークフローについて説明します。 内側のループのワークフローは、運用への展開まで含めることができる DevOps のより広範なワークフローを考慮していないということではなく、開発者のコンピューター上で実行される開発作業のみに重点が置かれています。 環境を設定する初期手順は、一度のみ実行されるものなので含まれていません。

アプリケーションは、自分のサービスと追加のライブラリ (依存関係) で構成されます。 Docker アプリケーションを構築するときの基本手順を次の図 5-1 に示します。

:::image type="complex" source="./media/docker-app-development-workflow/life-cycle-containerized-apps-docker-cli.png" alt-text="コンテナー化されたアプリを作成するために必要な 7 つの手順を示す図。":::
Docker アプリの開発プロセス:1 - アプリをコーディングする、2- Dockerfile を書き込む、3 - Dockerfile で定義されているイメージを作成する、4 - (省略可能) docker-compose.yml ファイルにサービスを作成する、5 - コンテナーまたは docker-compose アプリを実行する、6 - アプリまたはマイクロサービスをテストする、7 - リポジトリにプッシュして繰り返す。
:::image-end:::

**図 5-1** Docker のコンテナー化されたアプリケーションを開発するための詳細なワークフロー

このセクションでは、このプロセス全体について詳細に記載されており、主要な各手順は、Visual Studio 環境に重点を置いて説明されています。

エディター/CLI 開発アプローチ (Visual Studio Code と macOS または Windows 上の Docker CLI など) を使用する場合、Visual Studio を使用する場合よりも、通常はより詳細にすべての手順を把握している必要があります。 CLI 環境での作業の詳細については、電子書籍「[Containerized Docker Application lifecycle with Microsoft Platforms and Tools](https://aka.ms/dockerlifecycleebook/)」 (Microsoft のプラットフォームおよびツールとコンテナー化された Docker アプリケーションのライフサイクル) を参照してください。

Visual Studio 2019 を使用している場合、これらの手順の多くは自動処理されるので、生産性が大幅に向上します。 これは、Visual Studio 2019 を使用しており、複数のコンテナー アプリケーションをターゲットとしている場合に特に当てはまります。 たとえば、マウスを 1 回クリックするだけで、Visual Studio によって、アプリケーションに適した構成の `Dockerfile` と `docker-compose.yml` ファイルがプロジェクトに追加されます。 そのアプリケーションを Visual Studio で実行すると、Docker イメージが構築され、Docker で直接マルチコンテナー アプリケーションが実行されます。また、一度に複数のコンテナーをデバッグすることもできます。 これらの機能により、開発の速度が向上します。

しかし、Visual Studio によって、これらの手順が自動化されるからといって、Docker の背後で何が起こっているのか知らなくてもよいというわけではありません。 したがって、次のガイダンスではすべての手順について詳しく説明します。

![手順 1 の画像。](./media/docker-app-development-workflow/step-1-code-your-app.png)

## <a name="step-1-start-coding-and-create-your-initial-application-or-service-baseline"></a>手順 1. コーディングを開始して、初期アプリケーションまたはサービス ベースラインを作成します。

Docker アプリケーションの開発方法は、Docker を使用しないアプリケーションの開発方法と似ています。 違いは、Docker 用の開発中に、ローカル環境において Docker コンテナー内で実行するアプリケーションまたはサービスを展開およびテストするということです (Docker による Linux VM のセットアップ、または Windows コンテナーを使用する場合は直接 Windows で)。

### <a name="set-up-your-local-environment-with-visual-studio"></a>Visual Studio でのローカル環境のセットアップ

最初に、次の手順で説明する、Windows 用の [Docker Community Edition (CE)](https://docs.docker.com/docker-for-windows/) がインストールされていることを確認します。

[Get started with Docker CE for Windows](https://docs.docker.com/docker-for-windows/) (Windows 用の Docker CE の概要)

さらに、図 5-2 に示すように、 **.NET Core クロスプラットフォーム開発**ワークロードがインストールされた Visual Studio 2019 バージョン 16.4 以降が必要です。

![.NET Core クロスプラットフォーム開発の選択のスクリーンショット。](./media/docker-app-development-workflow/dotnet-core-cross-platform-development.png)

**図 5-2** Visual Studio 2019 のセットアップ時の **.NET Core クロスプラットフォーム開発**ワークロードの選択

アプリケーションのコーディングは、アプリケーションで Docker を有効にし、Docker で展開してテストする前から、単純な .NET で開始することができます (コンテナーを使用する計画がある場合、通常は .NET Core で)。 ただし、実際の環境となることに加え、問題が早く検出されるため、できるだけ早く Docker で作業を開始することをお勧めします。 Docker は、Visual Studio では、ほとんど透過的で、使用しやすくなっているため、このようにすることが推奨されます。この最良の例は、Visual Studio からのマルチコンテナー アプリケーションのデバッグです。

### <a name="additional-resources"></a>その他の技術情報

- **Windows 用の Docker CE の概要** \
  <https://docs.docker.com/docker-for-windows/>

- **Visual Studio 2019** \
  [https://visualstudio.microsoft.com/downloads/](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

![手順 2 の画像。](./media/docker-app-development-workflow/step-2-write-dockerfile.png)

## <a name="step-2-create-a-dockerfile-related-to-an-existing-net-base-image"></a>手順 2. 既存の .NET 基本イメージに関連する Dockerfile を作成します。

Dockerfile は、構築するカスタム イメージごとに必要です。また、Visual Studio から自動的に展開する場合も、Docker CLI (docker run および docker-compose コマンド) を使用して手動で展開する場合も、展開するコンテナーごとに Dockerfile が必要です。 アプリケーションにカスタム サービスが 1 つ含まれている場合、Dockerfile が 1 つ必要です。 (マイクロサービス アーキテクチャの場合のように) アプリケーションに複数のサービスが含まれる場合、サービスごとに Dockerfile が 1 つ必要です。

Dockerfile は、アプリケーションまたはサービスのルート フォルダーにあります。 これには、コンテナーでアプリケーションまたはサービスを設定して実行する方法を Docker に指示するコマンドが含まれています。 手動でコードに Dockerfile を作成し、.NET の依存関係と共にプロジェクトに追加できます。

Visual Studio と Docker 用のツールでは、このタスクはマウスを何度かクリックするのみで実行できます。 Visual Studio 2019 で新しいプロジェクトを作成する場合、図 5-3 に示すように **[Docker サポートを有効にする]** というオプションがあります。

![[Docker サポートを有効にする] チェックボックスを示すスクリーンショット。](./media/docker-app-development-workflow/enable-docker-support-check-box.png)

**図 5-3** Visual Studio 2019 で新しい ASP.NET Core プロジェクトを作成するときの Docker サポートの有効化

また、図 5-4 に示すように、**ソリューション エクスプローラー**でプロジェクトを右クリックし、 **[追加]**  >  **[Docker サポート...]** を選択することで、既存の ASP.NET Core Web アプリ プロジェクトに対して Docker サポートを有効にすることもできます。

![[追加] メニューの [Docker サポート] オプションを示すスクリーンショット。](./media/docker-app-development-workflow/add-docker-support-option.png)

**図 5-4** 既存の Visual Studio 2019 プロジェクトでの Docker サポートの有効化

この操作で、必要な構成のプロジェクトに *Dockerfile* が追加され、ASP.NET Core プロジェクトでのみ使用できるようになります。

同じように、Visual Studio で **[追加] > [コンテナー オーケストレーターのサポート]** オプションを使用して、ソリューション全体の `docker-compose.yml` ファイルを追加することもできます。手順 4 で、このオプションについてさらに詳しく説明します。

### <a name="using-an-existing-official-net-docker-image"></a>既存の公式の .NET Docker イメージの使用

通常、[Docker Hub](https://hub.docker.com/) レジストリなどの公式のリポジトリから取得する基本イメージ上に、コンテナーのカスタム イメージをビルドします。 Visual Studio で Docker のサポートを有効にした場合、背後ではこれがまさに発生します。 Dockerfile では、既存の `dotnet/core/aspnet` イメージを使用します。

選択した OS とフレームワークによって、使用できる Docker イメージとリポジトリについては、既に説明しています。 たとえば、ASP.NET Core (Linux または Windows) を使用したい場合は、`mcr.microsoft.com/dotnet/core/aspnet:3.1` のイメージを使用します。 この場合は、コンテナーに使用する基本の Docker イメージのみを指定する必要があります。 これは、`FROM mcr.microsoft.com/dotnet/core/aspnet:3.1` を Dockerfile に追加することで行います。 これは、Visual Studio が自動的に実行しますが、バージョンを更新する場合は、この値を更新する必要があります。

バージョン番号を使用して Docker Hub の公式の .NET イメージ リポジトリを使うと、同じ言語機能が (開発、テスト、および実稼働環境などの) すべてのコンピューターで使用できるようになります。

次の例では、ASP.NET Core コンテナー用のサンプルの Dockerfile を示しています。

```Dockerfile
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
ARG source
WORKDIR /app
EXPOSE 80
COPY ${source:-obj/Docker/publish} .
ENTRYPOINT ["dotnet", " MySingleContainerWebApp.dll "]
```

この場合、イメージは、公式の ASP.NET Core Docker イメージ (Linux および Windows 用マルチアーキテクチャ) のバージョン 3.1 に基づいています。 この設定は、`FROM mcr.microsoft.com/dotnet/core/aspnet:3.1` です。 (この基本イメージの詳細については、「[.NET Core Docker Image](https://hub.docker.com/_/microsoft-dotnet-core/)」 (.NET Core Docker イメージ) ページを参照してください)。Dockerfile では、実行時に使用する、リッスンする TCP ポートを、Docker に指示する必要があります (ここでは、EXPOSE 設定で構成したポート 80)。

使用している言語とフレームワークによっては、Dockerfile に別の構成設定を指定できます。 たとえば、`["dotnet", "MySingleContainerWebApp.dll"]` の ENTRYPOINT 行では、.NET Core アプリケーションを実行するように Docker に指示します。 SDK と .NET Core CLI (dotnet CLI) を使用して、.NET アプリケーションをビルドおよび実行している場合、この設定は異なります。 つまり、アプリケーションに選択した言語とプラットフォームにより、ENTRYPOINT 行とその他の設定は別になります。

### <a name="additional-resources"></a>その他の技術情報

- **.NET Core アプリケーションの Docker イメージのビルド** \
  [https://docs.microsoft.com/dotnet/core/docker/building-net-docker-images](/aspnet/core/host-and-deploy/docker/building-net-docker-images)

- **Build your own image (独自のイメージのビルド)** 。 Docker の公式なドキュメント内にあります。\
  <https://docs.docker.com/engine/tutorials/dockerimages/>

- **.NET コンテナー イメージを最新の状態に保つ** \
  <https://devblogs.microsoft.com/dotnet/staying-up-to-date-with-net-container-images/>

- **.NET と Docker を組み合わせて使用する - DockerCon 2018 の更新** \
  <https://devblogs.microsoft.com/dotnet/using-net-and-docker-together-dockercon-2018-update/>

### <a name="using-multi-arch-image-repositories"></a>マルチアーキテクチャ イメージ リポジトリの使用

単一のリポジトリには、Linux イメージや Windows イメージなどのプラットフォーム バリアントを含めることができます。 この機能では、Microsoft (基本イメージの作成者) などのベンダーが、複数のプラットフォーム (つまり、Linux および Windows) に対応できるリポジトリを 1 つ作成できます。 たとえば、Docker Hub レジストリにある [dotnet/core](https://hub.docker.com/_/microsoft-dotnet-core/) リポジトリでは、同じリポジトリ名を使用して Linux および Windows Nano Server をサポートしています。

タグを指定する場合、次の場合のように、明示的にプラットフォームを指定できます。

- `mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim` \
  ターゲット: Linux の .NET Core 3.1 ランタイムのみ

- `mcr.microsoft.com/dotnet/core/aspnet:3.1-nanoserver-1909` \
  ターゲット: Windows Nano Server の .NET Core 3.1 ランタイムのみ

しかし、同じイメージ名を指定した場合、タグが同じでも、次の例に示すように、マルチアーキテクチャ イメージ (`aspnet` イメージなど) では、展開する Docker ホスト OS に応じて、Linux または Windows バージョンが使用されます。

- `mcr.microsoft.com/dotnet/core/aspnet:3.1` \
  マルチアーキテクチャ: Docker ホスト OS に応じて、Linux または Windows Nano Server の .NET Core 3.1 ランタイムのみ

この場合、Windows ホストからイメージをプルした場合、Windows バリアントがプルされ、同じイメージ名が Linux ホストからプルされた場合、Linux バリアントがプルされます。

### <a name="multi-stage-builds-in-dockerfile"></a>Dockerfile のマルチステージ ビルド

Dockerfile はバッチ スクリプトに似ています。 コマンド ラインからコンピューターを設定する必要があった場合に行う操作と似ています。

これは初期コンテキストを設定する基本イメージから始まり、ホスト OS 上にある、スタートアップ ファイル システムのようなものです。 これは OS ではありませんが、コンテナー内の OS "のようなもの" と考えることができます。

コマンド ラインを実行するたびに、以前のものからの変更が適用された新しいレイヤーがファイル システムに作成されるため、コンパイル時に結果のファイル システムが生成されます。

新しいすべてのレイヤーは以前のものの上に "配置され"、コマンドが実行されるたびに結果のイメージのサイズが増えるため、イメージにアプリケーションのビルドと発行に必要な SDK などを含める必要がある場合、そのイメージが非常に大きくなる可能性があります。

ここで、マルチステージ ビルドの (Docker 17.05 以降の) プロットに入り、その機能を利用します。

核となる概念は、Dockerfile の実行プロセスを複数のステージに分けられることです。ステージは後に 1 つ以上のコマンドが続く初期イメージで、最後のステージで最終イメージのサイズが決定されます。

つまり、マルチステージ ビルドでは、作成作業を異なる "フェーズ" に分割し、中間ステージから関連するディレクトリのみを取得する最終イメージを組み立てることができます。 この機能を使用する一般的な戦略は次のとおりです。

1. アプリケーションをビルドし、フォルダーに発行するために必要なすべてのものを含む、基本的な SDK イメージ (大きさに関係なく) を使用します

2. その後、基本的な小さいランタイムのみのイメージを使用し、前のステージから発行フォルダーをコピーして、小さな最終イメージを生成します。

マルチステージを理解する最善の方法はおそらく、Dockerfile を詳しく見ていくことであるため、Docker サポートをプロジェクトに追加するときに Visual Studio によって作成される初期の Dockerfile から始め、後でいくつかの最適化について説明します。

初期の Dockerfile は、次のようになります。

```Dockerfile
 1  FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base
 2  WORKDIR /app
 3  EXPOSE 80
 4
 5  FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
 6  WORKDIR /src
 7  COPY src/Services/Catalog/Catalog.API/Catalog.API.csproj …
 8  COPY src/BuildingBlocks/HealthChecks/src/Microsoft.AspNetCore.HealthChecks …
 9  COPY src/BuildingBlocks/HealthChecks/src/Microsoft.Extensions.HealthChecks …
10  COPY src/BuildingBlocks/EventBus/IntegrationEventLogEF/ …
11  COPY src/BuildingBlocks/EventBus/EventBus/EventBus.csproj …
12  COPY src/BuildingBlocks/EventBus/EventBusRabbitMQ/EventBusRabbitMQ.csproj …
13  COPY src/BuildingBlocks/EventBus/EventBusServiceBus/EventBusServiceBus.csproj …
14  COPY src/BuildingBlocks/WebHostCustomization/WebHost.Customization …
15  COPY src/BuildingBlocks/HealthChecks/src/Microsoft.Extensions …
16  COPY src/BuildingBlocks/HealthChecks/src/Microsoft.Extensions …
17  RUN dotnet restore src/Services/Catalog/Catalog.API/Catalog.API.csproj
18  COPY . .
19  WORKDIR /src/src/Services/Catalog/Catalog.API
20  RUN dotnet build Catalog.API.csproj -c Release -o /app
21
22  FROM build AS publish
23  RUN dotnet publish Catalog.API.csproj -c Release -o /app
24
25  FROM base AS final
26  WORKDIR /app
27  COPY --from=publish /app .
28  ENTRYPOINT ["dotnet", "Catalog.API.dll"]
```

行ごとの詳細は次のとおりです。

- **行番号 1:** "小さな" ランタイムのみの基本イメージを使用するステージを開始します。参照用にこれを**基本**と呼びます。

- **行番号 2:** イメージに **/app** ディレクトリを作成します。

- **行番号 3:** ポート **80** を公開します。

- **行番号 5:** ビルド/発行用の "大きな" イメージを使用して、新しいステージを開始します。 参照用にこれを **build** と呼びます。

- **行番号 6:** イメージに **/src** ディレクトリを作成します。

- **行番号 7:** 16 行目まで、参照する **.csproj** プロジェクト ファイルをコピーし、後でパッケージを復元できるようにします。

- **行番号 17:** **Catalog.API** プロジェクトと参照プロジェクト用のパッケージを復元します。

- **行番号 18:** イメージの **/src** に ( **.dockerignore** ファイルに含まれているファイルとディレクトリを除く) **ソリューション用のすべてのディレクトリ ツリー**をコピーします。

- **行番号 19:** 現在のフォルダーを **Catalog.API** プロジェクトに変更します。

- **行番号 20:** プロジェクト (およびその他のプロジェクトの依存関係) をビルドし、イメージ内で **/app** ディレクトリに出力します。

- **行番号 22:** ビルドに続き、新しいステージを開始します。 参照用にこれを **publish** と呼びます。

- **行番号 23:** プロジェクト (および依存関係) を発行し、イメージの **/app** ディレクトリに出力します。

- **行番号 25:** **base** に続き、新しいステージを開始し、これを **final** と呼びます。

- **行番号 26:** 現在のディレクトリを **/app** に変更します。

- **行番号 27:** **publish** ステージから **/app** ディレクトリを現在のディレクトリにコピーします。

- **行番号 28:** コンテナーの開始時に実行するコマンドを定義します。

次は、プロセス全体のパフォーマンスを向上させるための最適化についていくつか見ていきます。eShopOnContainers の場合、Linux コンテナーの完全なソリューションをビルドするには約 22 分以上かかることになります。

非常にシンプルな Docker のレイヤー キャッシュ機能を利用します。基本イメージとコマンドが、前に実行したいくつかと同じである場合は、結果のレイヤーを使用するだけで済みます。コマンドを実行する必要はないため、時間を節約できます。

次は、**ビルド** ステージについて詳しく見ていきます。5 行目から 6 行目まではほとんど同じですが、eShopOnContainers からのすべてのサービスについては、7 行目から 17 行目までが異なります。したがって、毎回実行する必要があります。しかし、7 行目から 16 行目を次のように変更したとします。

```Dockerfile
COPY . .
```

その場合、すべてのサービスについてまったく同じになり、ソリューション全体がコピーされ、大きなレイヤーが作成されます。

1. しかし、初回時 (およびファイルが変更された場合は、再ビルド時) にのみ、コピー プロセスが実行され、他のすべてのサービスではキャッシュが使用されます

2. 中間ステージでより大きなイメージが使用されるため、最終イメージ サイズには影響しません。

次の大幅な最適化は、17 行目で実行される `restore` コマンドに関係します。これも、eShopOnContainers のすべてサービスについて異なります。 その行を次のように変更したとします。

```Dockerfile
RUN dotnet restore
```

その場合、ソリューション全体のパッケージが復元されます。しかし、ここでも、現在の方法では 15 回ではなく、1 回だけ行われます。

しかし、フォルダーに 1 つのプロジェクトまたはソリューション ファイルがある場合にのみ、`dotnet restore` が実行されるため、これを実現するのは少し複雑になります。これを解決するための方法を簡単に説明すると、次のようになります。

1. 次の行を **.dockerignore** に追加します。

   - `*.sln`。メイン フォルダー ツリーのすべてのソリューション ファイルを無視します

   - `!eShopOnContainers-ServicesAndWebApps.sln`。このソリューション ファイルのみを含めます。

2. `dotnet restore` への `/ignoreprojectextensions:.dcproj` 引数を含めます。したがって、docker-compose プロジェクトも無視され、eShopOnContainers-ServicesAndWebApps ソリューションのパッケージのみが復元されます。

最終的な最適化では、20 行目が余分になります。23 行目でもアプリケーションがビルドされ、本質的には 20 行目のすぐ後に続くため、別のコマンドで時間がかかることになります。

結果のファイルは次のようになります。

```Dockerfile
 1  FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base
 2  WORKDIR /app
 3  EXPOSE 80
 4
 5  FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS publish
 6  WORKDIR /src
 7  COPY . .
 8  RUN dotnet restore /ignoreprojectextensions:.dcproj
 9  WORKDIR /src/src/Services/Catalog/Catalog.API
10  RUN dotnet publish Catalog.API.csproj -c Release -0 /app
11
12  FROM base AS final
13  WORKDIR /app
14  COPY --from=publish /app
15  ENTRYPOINT ["dotnet", "Catalog.API.dll"]
```

### <a name="creating-your-base-image-from-scratch"></a>一からの基本イメージの作成

独自の Docker 基本イメージを一から作成することができます。 このシナリオは、Docker を初めて使用するユーザーには推奨されませんが、独自の基本イメージの特定のビットを設定したい場合にそれを行うことができます。

### <a name="additional-resources"></a>その他の技術情報

- **マルチアーキテクチャの .NET Core イメージ**。\
  <https://github.com/dotnet/announcements/issues/14>

- **Create a base image** (基本イメージを作成する) Docker の公式なドキュメント。\
  <https://docs.docker.com/develop/develop-images/baseimages/>

![手順 3 の画像。](./media/docker-app-development-workflow/step-3-create-dockerfile-defined-images.png)

## <a name="step-3-create-your-custom-docker-images-and-embed-your-application-or-service-in-them"></a>手順 3. カスタマイズした Docker イメージを作成し、それにアプリケーションまたはサービスを埋め込みます。

アプリケーションの各サービスには、関連イメージを作成する必要があります。 アプリケーションが 1 つのサービスまたは Web アプリケーションで構成されている場合、イメージは 1 つのみ必要です。

Docker イメージは、Visual Studio が自動的に構築することに注意してください。 次の手順は、エディター/CLI ワークフローにのみ必要であり、背後で何が起こっているのか説明するために示しています。

開発者は、完全な機能をプッシュできるか、(GitHub などの) ソース管理システムに変更するまで、ローカルで開発およびテストする必要があります。 つまり、Docker イメージを作成してローカルの Docker ホスト (Windows または Linux VM) にコンテナーを展開し、それらのローカルのコンテナーに対して実行、テスト、およびデバッグをする必要があることを意味します。

Docker CLI と Dockerfile を使用して、ローカルの環境にカスタム イメージを作成するには、図 5-5 のとおり、docker build コマンドを使用します。

![docker build コマンドのコンソール出力を示すスクリーンショット。](./media/docker-app-development-workflow/run-docker-build-command.png)

**図 5-5** カスタム Docker イメージの作成

必要に応じて、プロジェクト フォルダーから docker build を直接実行する代わりに、`dotnet publish` を実行して必要な .NET ライブラリとバイナリを使用して、展開可能なフォルダーをまず生成してから、`docker build` コマンドを使用します。

これで、`cesardl/netcore-webapi-microservice-docker:first` という名前の Docker イメージが作成されます。 このとき、:first は特定のバージョンを表すタグです。 この手順は、構成した Docker アプリケーション用に作成する必要のある各カスタム イメージで繰り返します。

アプリケーションが複数のコンテナーで構成されている場合 (つまり、マルチコンテナー アプリケーションの場合)、`docker-compose up --build` コマンドを使って、関連する docker-compose.yml ファイルで公開されているメタデータを使用して、1 つのコマンドですべての関連イメージをビルドすることもできます。

図 5-6 の docker images コマンドを使用すると、ローカル リポジトリの既存のイメージを検索できます。

![コマンド docker イメージからのコンソール出力。既存のイメージを確認できます。](./media/docker-app-development-workflow/view-existing-images-with-docker-images.png)

**図 5-6** docker images コマンドを使用した既存のイメージの表示

### <a name="creating-docker-images-with-visual-studio"></a>Visual Studio での Docker イメージの作成

Visual Studio を使用して、Docker のサポートでプロジェクトを作成する場合、イメージは明示的には作成されません。 代わりに、**F5** (または **Ctrl + F5**) キーを押し、Docker でコンテナー化されたアプリケーションまたはサービスを実行することによって、イメージが作成されます。 この手順は Visual Studio で自動的に実行されるので、処理内容を見ることはできませんが、内部の処理内容を知ることは重要です。

![省略可能な手順 4 の画像。](./media/docker-app-development-workflow/step-4-define-services-docker-compose-yml.png)

## <a name="step-4-define-your-services-in-docker-composeyml-when-building-a-multi-container-docker-application"></a>手順 4. マルチ コンテナー Docker アプリケーションを構築するときの docker-compose.yml へのサービスの定義

[docker-compose.yml](https://docs.docker.com/compose/compose-file/) ファイルでは、展開用のコマンドを使用して構成済みのアプリケーションとして関連サービスのセットを定義できます。 また、その依存関係と実行時の構成が行われます。

docker-compose.yml ファイルを使用する場合、メインまたはルート ソリューション フォルダーに、次の例と類似した内容でファイルを作成する必要があります。

```yml
version: '3.4'

services:

  webmvc:
    image: eshop/web
    environment:
      - CatalogUrl=http://catalog-api
      - OrderingUrl=http://ordering-api
    ports:
      - "80:80"
    depends_on:
      - catalog-api
      - ordering-api

  catalog-api:
    image: eshop/catalog-api
    environment:
      - ConnectionString=Server=sqldata;Port=1433;Database=CatalogDB;…
    ports:
      - "81:80"
    depends_on:
      - sqldata

  ordering-api:
    image: eshop/ordering-api
    environment:
      - ConnectionString=Server=sqldata;Database=OrderingDb;…
    ports:
      - "82:80"
    extra_hosts:
      - "CESARDLBOOKVHD:10.0.75.1"
    depends_on:
      - sqldata

  sqldata:
    image: mssql-server-linux:latest
    environment:
      - SA_PASSWORD=Pass@word
      - ACCEPT_EULA=Y
    ports:
      - "5433:1433"
```

この docker-compose.yml ファイルは、簡略化され、結合されたバージョンです。 これには、常に必要な (カスタム イメージ名などの) 各コンテナー用の静的な構成データと、展開環境によっては異なる場合のある、接続文字列などの構成情報が含まれています。 後半のセクションでは、複数の docker-compose ファイルに docker-compose.yml 構成を分割し、環境と実行の種類 (デバッグまたはリリース) に応じて、値をオーバーライドする方法について学習します。

docker-compose.yml ファイルの例では、`webmvc` サービス (Web アプリケーション)、2 つのマイクロサービス (`ordering-api` と `basket-api`)、および 1 つのデータ ソース コンテナー (コンテナーとして実行される Linux 用 SQL Server に基づく `sqldata`) というの 4 つのサービスが定義されています。 各サービスはコンテナーとして展開されるので、それぞれに Docker イメージが必要です。

docker-compose.yml ファイルでは、どのコンテナーが使用されているかのみでなく、それらがどのように個々に構成されるかが指定されています。 たとえば、.yml ファイルの `webmvc` コンテナーの定義は次のようになります。

- ビルド済みの `eshop/web:latest` イメージを使用します。 ただし、docker-compose の実行の一部として、docker-compose ファイルの build: セクションの追加構成を加え、イメージがビルドされるように構成することもできます。

- 2 つの環境変数 (CatalogUrl および OrderingUrl) を初期化します。

- コンテナー上の公開されているポート 80 を、ホスト コンピューター上の外部ポート 80 に転送します。

- depends_on 設定を使用して、カタログと順序付けサービスに Web アプリをリンクします。 これにより、サービスは、それらのサービスが開始されるまで待機するようになります。

docker-compose.yml ファイルについては、マイクロサービスとマルチコンテナー アプリを実装する方法について説明する後のセクションで、再確認します。

### <a name="working-with-docker-composeyml-in-visual-studio-2019"></a>Visual Studio 2019 での docker-compose.yml の操作

Dockerfile をプロジェクトに追加するだけでなく、前述のとおり、Visual Studio 2017 (バージョン 15.8 以降) では、ソリューションに Docker Compose のオーケストレーター サポートを追加できます。

図 5-7 に示すように、コンテナー オーケストレーターのサポートを最初に追加するときに、Visual Studio でプロジェクト用の Dockerfile が作成され、いくつかのグローバル `docker-compose*.yml` ファイルを含むソリューションに新しい (サービス セクション) プロジェクトが作成されてから、それらのファイルにプロジェクトが追加されます。 その後、docker-compose.yml ファイルを開き、追加機能で更新することができます。

docker-compose.yml ファイルに含めるプロジェクトごとに、この操作フォームを繰り返す必要があります。

この記事の執筆時点では、Visual Studio で **Docker Compose** オーケストレーターと **Kubernetes/Helm** オーケストレーターがサポートされています。

![プロジェクト コンテキスト メニューの [コンテナー オーケストレーター サポート] オプションを示すスクリーンショット。](./media/docker-app-development-workflow/add-container-orchestrator-support-option.png)

**図 5-7** ASP.NET Core プロジェクトの右クリックでの Visual Studio 2019 への Docker サポートの追加

Visual Studio でソリューションにオーケストレーター サポートを追加すると、図 5-8 のとおり、追加された docker-compose.yml ファイルが含まれた新しいノード (`docker-compose.dcproj` プロジェクト ファイル) もソリューション エクスプローラーに表示されます。

![ソリューション エクスプローラーの docker-compose ノードのスクリーンショット。](./media/docker-app-development-workflow/docker-compose-tree-node.png)

**図 5-8**. Visual Studio 2019 のソリューション エクスプローラーに追加された **docker-compose** ツリー ノード

`docker-compose up` コマンドを使用すると、1 つの docker-compose.yml ファイルで、マルチコンテナー アプリケーションを展開することができます。 しかし、Visual Studio でそれらのグループが追加されるため、環境 (開発または運用) と実行の種類 (リリースまたはデバッグ) に応じて、値をオーバーライドできます。 この機能は、後のセクションで説明します。

![手順 5 の画像。](./media/docker-app-development-workflow/step-5-run-containers-compose-app.png)

## <a name="step-5-build-and-run-your-docker-application"></a>手順 5. Docker アプリケーションのビルドと実行

アプリケーションにコンテナーが 1 つしかない場合、Docker ホスト (仮想マシンまたは物理サーバー) に展開して実行することができます。 ただし、アプリケーションに複数のサービスが含まれている場合、単一の CLI コマンド (`docker-compose up)`) を使用するか、背後でそのコマンドを使用する Visual Studio を使用して、構成されたアプリケーションとして展開することができます。 別のオプションを見てみましょう。

### <a name="option-a-running-a-single-container-application"></a>オプション A:単一コンテナー アプリケーションの実行

#### <a name="using-docker-cli"></a>Docker CLI の使用

図 5-9 に示すように、`docker run` コマンドを使用して Docker コンテナーを実行できます。

```console
docker run -t -d -p 80:5000 cesardl/netcore-webapi-microservice-docker:first
```

上記のコマンドでは、実行されるたびに、指定されたイメージから新しいコンテナー インスタンスが作成されます。 `--name` パラメーターを使用してコンテナーに名前を付けてから、`docker start {name}` (またはコンテナー ID あるいは自動名) を使って、既存のコンテナー インスタンスを実行することができます。

![docker run コマンドを使用して Docker コンテナーを実行しているスクリーンショット。](./media/docker-app-development-workflow/use-docker-run-command.png)

**図 5-9** docker run コマンドを使用した Docker コンテナーの実行

この場合、このコマンドによって、コンテナーの内部ポート 5000 がホスト コンピューターのポート 80 にバインドされます。 つまり、ホストはポート 80 をリッスンし、コンテナーのポート 5000 に転送することを意味します。

表示されるハッシュはコンテナー ID であり、`--name` オプションが指定されていない場合は、ランダムな読み取り可能な名前も割り当てられます。

#### <a name="using-visual-studio"></a>Visual Studio の使用

コンテナー オーケストレーターのサポートを追加していない場合は、Visual Studio で **Ctrl + F5** キーを押して単一コンテナー アプリを実行することもできます。また、**F5** キーを使用して、コンテナー内でアプリケーションをデバッグすることもできます。 コンテナーは、docker run を使用してローカルで実行されます。

### <a name="option-b-running-a-multi-container-application"></a>オプション B:マルチコンテナー アプリケーションの実行

多くのエンタープライズ シナリオでは、Docker アプリケーションは複数のサービスで構成されています。つまり、図 5-10 のようにマルチコンテナーのアプリケーションを実行する必要があります。

![いくつかの Docker コンテナーを含む VM](./media/docker-app-development-workflow/vm-with-docker-containers-deployed.png)

**図 5-10** Docker コンテナーが展開された VM

#### <a name="using-docker-cli"></a>Docker CLI の使用

Docker CLI を使用してマルチコンテナー アプリケーションを実行するには、`docker-compose up` コマンドを使用します。 このコマンドでは、マルチコンテナー アプリケーションを展開するためにソリューション レベルにある **docker compose.yml** ファイルを使用します。 図 5-11 は、docker-compose.yml ファイルを含む、メイン ソリューション ディレクトリからコマンドを実行した結果を示しています。

![docker-compose up コマンドの実行時の画面表示](./media/docker-app-development-workflow/results-docker-compose-up.png)

**図 5-11** docker-compose up コマンドの実行結果の例

docker-compose up コマンドを実行すると、図 5-10 に示すように、アプリケーションとその関連コンテナーが Docker ホストに展開されます。

#### <a name="using-visual-studio"></a>Visual Studio の使用

Visual Studio 2019 を使用したマルチコンテナー アプリケーションの実行は非常に簡単です。 **Ctrl + F5** キーを押して実行するか、**F5** キーを押してデバッグするだけです。その場合、通常どおり、**docker-compose** をスタートアップ プロジェクトとして設定します。  必要なすべてのセットアップは Visual Studio によって処理されるため、既に接続されているデバッガーを使用して、通常どおりブレークポイントを作成し、最終的に "リモート サーバー" で実行される独立したプロセスになるものを簡単に デバッグできます。

既に説明したとおり、ソリューション内のプロジェクトに Docker ソリューションのサポートを追加するたびに、そのプロジェクトは、グローバル (ソリューション レベル) docker-compose.yml ファイルに構成され、一度にソリューション全体を実行またはデバッグできるようになります。 Visual Studio では、Docker ソリューションのサポートが有効になっているプロジェクトごとに 1 つのコンテナーが起動され、内部のすべての手順をユーザーのために実行してくれます (dotnet publish、docker build など)。

面倒な作業をすべて見てみる場合は、以下のファイルを参照します。

`{root solution folder}\obj\Docker\docker-compose.vs.debug.g.yml`

ここで重要なのは、図 5-12 に示すように、Visual Studio 2019 には、F5 のアクション用に追加の **Docker** コマンドがあることです。 このオプションでは、ソリューション レベルで docker-compose.yml ファイルに定義されているすべてのコンテナーを実行して、マルチコンテナー アプリケーションを実行またはデバッグできます。 マルチコンテナー ソリューションをデバッグできるということは、異なるプロジェクト (コンテナー) にそれぞれブレークポイントを設定でき (いくつかブレークポイントを設定でき)、Visual Studio でデバッグする際、別のプロジェクトに定義され、別のコンテナーで実行されているブレークポイントで停止されることを意味します。

![docker-compose プロジェクトを実行しているデバッグ ツールバーのスクリーンショット。](./media/docker-app-development-workflow/debug-toolbar-docker-compose-project.png)

**図 5-12** Visual Studio 2019 でのマルチコンテナー アプリの実行

### <a name="additional-resources"></a>その他の技術情報

- **リモート Docker ホストに ASP.NET コンテナーを配置する** \
  <https://docs.microsoft.com/azure/vs-azure-tools-docker-hosting-web-apps-in-docker>

### <a name="a-note-about-testing-and-deploying-with-orchestrators"></a>オーケストレーターを使用したテストと展開に関する注意事項

docker-compose up および docker run コマンド (または Visual Studio でのコンテナーの実行およびデバッグ) を使用して、開発環境でコンテナーを十分にテストできます。 しかし、[Kubernetes](https://kubernetes.io/) や [Service Fabric](https://azure.microsoft.com/services/service-fabric/) などのオーケストレーターをターゲットにする必要がある、運用での展開にはこの方法を使用しないでください。 Kubernetes を使用している場合は、[ポッド](https://kubernetes.io/docs/concepts/workloads/pods/pod/)を使ってコンテナーと[サービス](https://kubernetes.io/docs/concepts/services-networking/service/)を整理し、それらをネットワーク接続する必要があります。 [展開](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)を使用して、ポッドの作成と変更を整理することもできます。

![手順 6 の画像。](./media/docker-app-development-workflow/step-6-test-app-microservices.png)

## <a name="step-6-test-your-docker-application-using-your-local-docker-host"></a>手順 6. ローカル Docker ホストを使用した Docker アプリケーションのテスト

この手順は、アプリケーションで何が実行されているかによって異なります。 単一のコンテナーやサービスとして展開された単純な .NET Core Web アプリケーションでは、図 5-13 に示すように、Docker ホストでブラウザーを開き、サイトに移動して、サービスにアクセスできます。 (Dockerfile の構成で、コンテナーがホストの 80 以外のポートにマップされる場合、この URL にホスト ポストを含めます。)

![localhost/API/values からの応答のスクリーンショット。](./media/docker-app-development-workflow/test-docker-app-locally-localhost.png)

**図 5-13** localhost を使用したローカルでの Docker アプリケーションのテスト例

localhost が Docker ホスト IP をポイントしていない場合 (Docker CE を使用している場合、既定では、ポイントしている必要があります)、サービスに移動するには、コンピューターのネットワーク カードの IP アドレスを使用します。

なお、ブラウザーのこの URL では、説明している特定のコンテナーの例に、ポート 80 を使用しています。 ただし、前の手順で説明したとおり、docker run コマンドで展開されたとおり、要求は内部でポート 5000 にリダイレクトされています。

図 5-14 のとおり、ターミナルからカールを使用して、アプリケーションをテストすることも可能です。 Windows の Docker インストールでは、既定の Docker ホスト IP は常に、コンピューターの実際の IP アドレスに 10.0.75.1 を加えたものとなります。

![CURL で http://10.0.75.1/API/values を取得したときのコンソール出力。](./media/docker-app-development-workflow/test-docker-app-locally-curl.png)

**図 5-14** カールを使用したローカルでの Docker アプリケーションのテスト例

### <a name="testing-and-debugging-containers-with-visual-studio-2019"></a>Visual Studio 2019 を使用したコンテナーのテストとデバッグ

Visual Studio 2019 でコンテナーを実行またはデバッグする場合、コンテナーを使用しないでデバッグするのとほぼ同じ方法で .NET アプリケーションのデバッグを実行できます。

### <a name="testing-and-debugging-without-visual-studio"></a>Visual Studio を使用しないデバッグおよびテスト

エディター/CLI アプローチを使用して開発する場合、コンテナーのデバッグはより難しくなるため、トレースを生成してデバッグすることをお勧めします。

### <a name="additional-resources"></a>その他の技術情報

- **ローカルの Docker コンテナーでのアプリのデバッグ** \
  [https://docs.microsoft.com/visualstudio/containers/edit-and-refresh](/visualstudio/containers/edit-and-refresh)

- **作成者: Steve Lasker。Docker を使用した ASP.NET Core アプリのビルド、デバッグおよび展開。** ビデオ。 \
  <https://channel9.msdn.com/Events/Visual-Studio/Visual-Studio-2017-Launch/T115>

## <a name="simplified-workflow-when-developing-containers-with-visual-studio"></a>Visual Studio でのコンテナー開発の簡略ワークフロー

Visual Studio を使用するワークフローは、エディター/CLI アプローチを使用するワークフローよりも、実際はるかに簡単になります。 Dockerfile と docker-compose.yml ファイルに関係する Docker で必要な多くの手順は、図 5-15 のとおり、Visual Studio では背後で実行されるか、簡略化されます。

:::image type="complex" source="./media/docker-app-development-workflow/simplified-life-cycle-containerized-apps-docker-cli.png" alt-text="アプリを作成するために実行する 5 つの簡単な手順を示す図。":::
Docker アプリの開発プロセス:1 - アプリをコーディングする、2- Dockerfile を書き込む、3 - Dockerfile で定義されているイメージを作成する、4 - (省略可能) docker-compose.yml ファイルにサービスを作成する、5 - コンテナーまたは docker-compose アプリを実行する、6 - アプリまたはマイクロサービスをテストする、7 - リポジトリにプッシュして繰り返す。
:::image-end:::

**図 5-15**。 Visual Studio での開発の簡略ワークフロー

これに加え、(プロジェクトに Docker のサポートを追加する) 手順 2 を、一度のみ実行する必要があります。 つまり、ワークフローは、他の任意の開発に .NET を使用する場合の通常の開発タスクと似たものになります。 背後で何が起こっているのか (イメージのビルド プロセス、使用している基本イメージ、コンテナーの展開など) を理解しておく必要があり、動作をカスタマイズするのに Dockerfile または docker-compose.yml ファイルを編集する必要がある場合もあります。 しかし、多くの作業は Visual Studio を使用することで大幅に簡略化され、ユーザーの生産性を向上させます。

### <a name="additional-resources"></a>その他の技術情報

- **Steve Lasker。Visual Studio (2017) を使用した .NET Docker の開発** \
  <https://channel9.msdn.com/Events/Visual-Studio/Visual-Studio-2017-Launch/T111>

## <a name="using-powershell-commands-in-a-dockerfile-to-set-up-windows-containers"></a>Windows コンテナーを設定するための PowerShell コマンドの使用

[Windows コンテナー](https://docs.microsoft.com/virtualization/windowscontainers/about/index)は、既存の Windows アプリケーションを Docker イメージに変換して、Docker エコシステムの残りと同じツールで展開できます。 Windows コンテナーを使用するには、次の例のように、Dockerfile で PowerShell コマンドを実行します。

```Dockerfile
FROM mcr.microsoft.com/windows/servercore
LABEL Description="IIS" Vendor="Microsoft" Version="10"
RUN powershell -Command Add-WindowsFeature Web-Server
CMD [ "ping", "localhost", "-t" ]
```

この場合は、Windows Server Core 基本イメージ (FROM 設定) を使用して、PowerShell コマンド (RUN 設定) で IIS をインストールしています。 同様に、PowerShell コマンドを使用して、ASP.NET 4.x、.NET 4.6、またはその他の任意の Windows ソフトウェアなどの追加コンポーネントを設定することもできます。 たとえば、Dockerfile の次のコマンドでは、ASP.NET 4.5 が設定されます。

```Dockerfile
RUN powershell add-windowsfeature web-asp-net45
```

### <a name="additional-resources"></a>その他の技術情報

- **aspnet-docker/Dockerfile.** Windows の機能を含めるために dockerfile から実行する PowerShell コマンドの例。\
  <https://github.com/Microsoft/aspnet-docker/blob/master/4.7.1-windowsservercore-ltsc2016/runtime/Dockerfile>

>[!div class="step-by-step"]
>[前へ](index.md)
>[次へ](../multi-container-microservice-net-applications/index.md)
