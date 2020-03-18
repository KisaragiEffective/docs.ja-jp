---
title: ドッカー - WCF 開発者向け gRPC
description: コア gRPC アプリケーションASP.NET用の Docker イメージの作成
ms.date: 09/02/2019
ms.openlocfilehash: e67c43f9486fbfe9a5d3e84e3b74770eb621f604
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79148115"
---
# <a name="create-docker-images"></a>ドッカーイメージの作成

このセクションでは、Docker、Kubernetes、またはその他のコンテナー環境で実行できる、ASP.NETコア gRPC アプリケーション用の Docker イメージの作成について説明します。 ASP.NETコア MVC Web アプリと gRPC サービスで使用されるサンプル アプリケーションは、GitHub の[ドットネット アーキテクチャ/grpc-for-wcf 開発者](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/KubernetesSample)リポジトリで利用できます。

## <a name="microsoft-base-images-for-aspnet-core-applications"></a>ASP.NET コア アプリケーションのマイクロソフト ベース イメージ

マイクロソフトでは、.NET Core アプリケーションを構築および実行するためのさまざまな基本イメージを提供しています。 core 3.0 イメージASP.NET作成するには、次の 2 つの基本イメージを使用します。

- アプリケーションをビルドして公開する SDK イメージ。
- 配置用のランタイム イメージ。

| Image | 説明 |
| ----- | ----------- |
| [mcr.microsoft.com/dotnet/core/sdk](https://hub.docker.com/_/microsoft-dotnet-core-sdk/) | を使用してアプリケーション`docker build`を構築する場合 生産時には使用しないでください。 |
| [mcr.microsoft.com/dotnet/core/aspnet](https://hub.docker.com/_/microsoft-dotnet-core-aspnet/) | ランタイムおよびASP.NETコア依存関係が含まれます。 生産のため。 |

各イメージには、タグによって区別される、異なる Linux ディストリビューションに基づく 4 つのバリエーションがあります。

| イメージ タグ | Linux | Notes |
| --------- | ----- | ----- |
| 3.0バスター、3.0 | Debian 10 | OS バリアントが指定されていない場合のデフォルトイメージ。 |
| 3.0-アルパイン | アルパイン 3.9 | アルパインベースイメージは Debian や Ubuntu のイメージよりはるかに小さいです。 |
| 3.0-ディスコ | Ubuntu 19.04 | |
| 3.0-バイオニック | Ubuntu 18.04 | |

アルパインベースイメージは、Debian と Ubuntu のイメージの場合は 200 MB に対して約 100 MB です。 一部のソフトウェア パッケージまたはライブラリは、Alpine のパッケージ管理で使用できない場合があります。 どのイメージを使えばいいのかわからない場合は、デフォルトの Debian を選択する必要があります。

> [!IMPORTANT]
> ビルドとランタイムに同じバージョンの Linux を使用していることを確認します。 あるバリアントでビルドおよび発行されたアプリケーションは、別のバリアントでは動作しない場合があります。

## <a name="create-a-docker-image"></a>Docker イメージを作成します。

Docker イメージは Docker*ファイル*によって定義されます。 これは、アプリケーションをビルドし、アプリケーションのビルドまたは実行に必要な依存関係をインストールするために必要なすべてのコマンドを含むテキスト ファイルです。 次の例は、ASP.NET Core 3.0 アプリケーションの最も単純な Dockerfile を示しています。

```dockerfile
# Application build steps
FROM mcr.microsoft.com/dotnet/core/sdk:3.0 as builder

WORKDIR /src

COPY . .

RUN dotnet restore

RUN dotnet publish -c Release -o /published src/StockData/StockData.csproj

# Runtime image creation
FROM mcr.microsoft.com/dotnet/core/aspnet:3.0

# Uncomment the line below if running with HTTPS
# ENV ASPNETCORE_URLS=https://+:443

WORKDIR /app

COPY --from=builder /published .

ENTRYPOINT [ "dotnet", "StockData.dll" ]
```

Dockerfile には 2 つの部分があります:`sdk`最初の部分では、ベース イメージを使用してアプリケーションをビルドおよび発行します。2 番目の場合は、ベース`aspnet`からランタイム イメージを作成します。 これは、ランタイム`sdk`イメージの場合は約 200 MB のイメージに比べて約 900 MB であり、その内容のほとんどが実行時に不要であるためです。

### <a name="the-build-steps"></a>ビルドステップ

| 手順 | 説明 |
| ---- | ----------- |
| `FROM ...` | 基本イメージを宣言し、エイリアスを`builder`割り当てます。 |
| `WORKDIR /src` | ディレクトリを`/src`作成し、現在の作業ディレクトリとして設定します。 |
| `COPY . .` | ホスト上の現在のディレクトリの下にあるすべてのものを、イメージ上の現在のディレクトリにコピーします。 |
| `RUN dotnet restore` | 外部パッケージを復元します (Core 3.0 フレームワークが SDK と共にプリインストールASP.NET)。 |
| `RUN dotnet publish ...` | リリース ビルドをビルドして発行します。 フラグは`--runtime`必須ではありません。 |

### <a name="the-runtime-image-steps"></a>ランタイムイメージのステップ

| 手順 | 説明 |
| ---- | ----------- |
| `FROM ...` | 新しい基本イメージを宣言します。 |
| `WORKDIR /app` | ディレクトリを`/app`作成し、現在の作業ディレクトリとして設定します。 |
| `COPY --from=builder ...` | 最初`FROM`の行のエイリアスを使用して、公開されたアプリケーション`builder`を前のイメージからコピーします。 |
| `ENTRYPOINT [ ... ]` | コンテナーの開始時に実行するコマンドを設定します。 ランタイム`dotnet`イメージのコマンドでは、DLL ファイルのみを実行できます。 |

### <a name="https-in-docker"></a>ドッカーの HTTPS

Docker の Microsoft ベース`ASPNETCORE_URLS`イメージは`http://+:80`、環境変数を に設定し、Kestrel がそのポートで HTTPS なしで実行されることを意味します。 カスタム証明書 ([自己ホスト型 gRPC アプリケーション](self-hosted.md)の説明) で HTTPS を使用している場合は、これをオーバーライドする必要があります。 Dockerfile のランタイム イメージ作成部分で環境変数を設定します。

```dockerfile
# Runtime image creation
FROM mcr.microsoft.com/dotnet/core/aspnet:3.0

ENV ASPNETCORE_URLS=https://+:443
```

### <a name="the-dockerignore-file"></a>.docker無視ファイル

ファイルを`.gitignore`ソース管理から除外するファイルと同様に、ファイルを`.dockerignore`使用して、ビルド中にファイルやディレクトリをイメージにコピーしないようにすることができます。 これはコピー時間を節約するだけでなく、PCの`obj`ディレクトリがイメージにコピーされるというエラーを回避できます。 少なくとも、ファイルのエントリと`bin``obj`ファイルにエントリを追加`.dockerignore`する必要があります。

```console
bin/
obj/
```

## <a name="build-the-image"></a>イメージをビルドする

単一のアプリケーション、つまり単一の Dockerfile を持つソリューションの場合、Dockerfile を基本ディレクトリに配置するのが最も簡単です。 つまり、ファイルと同じディレクトリに`.sln`置きます。 その場合、イメージをビルドするには、Dockerfile を`docker build`含むディレクトリから次のコマンドを使用します。

```console
docker build --tag stockdata .
```

紛らわしい名前`--tag`のフラグ ( に短縮できる`-t`) は、イメージの名前全体を指定します。 `.`最後に、ビルドが実行されるコンテキストを指定します。Dockerfile 内のコマンド`COPY`の現在の作業ディレクトリ。

1 つのソリューション内に複数のアプリケーションがある場合は、各アプリケーションの Dockerfile を、ファイルの横にある`.csproj`独自のフォルダーに保持できます。 ベース ディレクトリからコマンド`docker build`を実行して、ソリューションとすべてのプロジェクトがイメージにコピーされるようにする必要があります。 (または`-f`) フラグを使用して、現在のディレクトリの`--file`下に Dockerfile を指定できます。

```console
docker build --tag stockdata --file src/StockData/Dockerfile .
```

## <a name="run-the-image-in-a-container-on-your-machine"></a>マシン上のコンテナでイメージを実行する

ローカル Docker インスタンスでイメージを実行するには、コマンド`docker run`を使用します。

```console
docker run -ti -p 5000:80 stockdata
```

この`-ti`フラグは、現在のターミナルをコンテナのターミナルに接続し、対話モードで実行します。 localhost ネットワーク インターフェイスのポート 5000 にコンテナー上の`-p 5000:80`パブリッシュ (リンク) ポート 80。

## <a name="push-the-image-to-a-registry"></a>イメージをレジストリにプッシュする

イメージが動作することを確認したら、Docker レジストリにプッシュして他のシステムで使用できるようにします。 内部ネットワークは、Docker レジストリをプロビジョニングする必要があります。 これは[Docker 独自`registry`のイメージ (Docker](https://docs.docker.com/registry/deploying/)レジストリーは Docker コンテナーで実行される) を実行するのと同じくらい簡単ですが、さまざまなより包括的なソリューションが利用できます。 外部共有およびクラウド使用の場合は[、Azure コンテナー レジストリ](https://docs.microsoft.com/azure/container-registry/)や[Docker Hub](https://docs.docker.com/docker-hub/repos/)など、さまざまなマネージ レジストリを利用できます。

Docker Hub にプッシュするには、イメージ名の前にユーザー名または組織名を付けます。

```console
docker tag stockdata myorg/stockdata
docker push myorg/stockdata
```

プライベート レジストリにプッシュするには、イメージ名の前にレジストリ ホスト名と組織名を付けます。

```console
docker tag stockdata internal-registry:5000/myorg/stockdata
docker push internal-registry:5000/myorg/stockdata
```

イメージがレジストリに格納された後、個々の Docker ホストまたは Kubernetes などのコンテナー オーケストレーション エンジンに展開できます。

>[!div class="step-by-step"]
>[前次](self-hosted.md)
>[Next](kubernetes.md)
