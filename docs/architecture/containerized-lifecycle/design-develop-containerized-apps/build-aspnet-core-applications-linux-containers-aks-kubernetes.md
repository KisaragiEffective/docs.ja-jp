---
title: Linux コンテナーとして AKS/Kubernetes クラスターにデプロイされる ASP.NET Core 2.2 アプリケーションをビルドする
description: Microsoft プラットフォームとツールでコンテナー化された Docker アプリケーションのライフサイクル
ms.date: 02/25/2019
ms.openlocfilehash: ab64a0423ceceb8285c159af276d6d97e12379d8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "70848753"
---
# <a name="build-aspnet-core-22-applications-deployed-as-linux-containers-into-an-akskubernetes-orchestrator"></a>Linux コンテナーとして AKS/Kubernetes オーケストレーターにデプロイされる ASP.NET Core 2.2 アプリケーションをビルドする

Azure Kubernetes Services (AKS) は、Azure のマネージド Kubernetes オーケストレーション サービスであり、コンテナーのデプロイと管理を簡略化します。

AKS の主な機能は次のとおりです。

- Azure でホストされるコントロール プレーン
- 自動アップグレード
- 自己復旧
- ユーザーが構成可能なスケーリング
- 開発者とクラスター オペレーターの両方のよりシンプルなユーザー エクスペリエンス。

次の例では、Linux で実行され、Azure の AKS クラスターにデプロイされる、ASP.NET Core 2.2 アプリケーションの作成について確認しますが、開発は Visual Studio 2017 を使用して行われています。

## <a name="creating-the-aspnet-core-22-project-using-visual-studio-2017"></a>Visual Studio 2017 を使用する ASP.NET Core 2.2 プロジェクトの作成

ASP.NET Core は、GitHub で Microsoft および .NET コミュニティによって管理される汎用開発プラットフォームです。 これはクロスプラットフォームであり、Windows、macOS、Linux がサポートされ、デバイス、クラウド、および埋め込み/IoT シナリオで使用できます。

この例では、Visual Studio Web API テンプレートに基づくシンプルなプロジェクトを使用します。したがって、サンプルを作成するための追加の知識は必要ありません。 ASP.NET Core 2.2 テクノロジを使用し、REST API で小さなプロジェクトを実行するための要素をすべて含む、標準テンプレートを使ってプロジェクトを作成するだけで済みます。

![ASP.NET Core Web アプリケーションが選択されている、Visual Studio の新しいプロジェクトの追加ウィンドウ。](media/create-aspnet-core-application.png)

**図 4-36**. ASP.NET Core アプリケーションの作成

Visual Studio でサンプル プロジェクトを作成するには、 **[ファイル]**  >  **[新規]**  >  **[プロジェクト]** の順に選択し、左側のウィンドウでプロジェクトの種類として **[Web]** を選び、その後に **[ASP.NET Core Web アプリケーション]** を選択します。

Visual Studio に Web プロジェクト用のテンプレートがリストされます。 この例では、 **[API]** を選択して、ASP.NET Web API アプリケーションを作成します。

ASP.NET Core 2.2 をフレームワークとして選択したことを確認します。 .NET Core 2.2 は最新バージョンの Visual Studio 2017 に含まれており、Visual Studio 2017 のインストール時に自動的にインストールされ、構成されます。

![API オプションが選択された状態の、ASP.NET Core Web アプリケーションの種類を選択するための Visual Studio ダイアログ。](media/create-web-api-application.png)

**図 4-37**. ASP.NET Core 2.2 と Web API プロジェクトの種類の選択

以前のバージョンの .NET Core をお持ちの場合は、<https://dotnet.microsoft.com/download> から 2.2 のバージョンをダウンロードしてインストールすることができます。

プロジェクトの作成時またはその後に、Docker サポートを追加できるため、いつでもプロジェクトを Docker でコンテナー化することができます。 プロジェクトの作成後に Docker サポートを追加するには、ソリューション エクスプローラーでプロジェクト ノードを右クリックし、コンテキスト メニューで **[追加]**  >  **[Docker サポート]** の順に選択します。

![既存のプロジェクトに Docker サポートを追加するためのコンテキスト メニュー オプション:(プロジェクトを) 右クリック > [追加] > [Docker サポート]。](media/add-docker-support-to-project.png)

**図 4-38**. 既存のプロジェクトへの Docker サポートの追加

Docker サポートの追加を完了するために、Windows または Linux を選択できます。 この場合は、 **[Linux]** を選択します。これは、AKS で Windows コンテナーがサポートされないためです (2018 年末時点)。

![Dockerfile のターゲット OS を選択するためのオプション ダイアログ。](media/select-linux-docker-support.png)

**図 4-39**. Linux コンテナーの選択。

これらのシンプルな手順では、Linux コンテナーで ASP.NET Core 2.2 アプリケーションを実行しています。

おわかりのように、Visual Studio 2017 と Docker 間の統合は完全に開発者の生産性を重視して行われています。

これで、**F5** キーまたは **[再生]** ボタンを使用して、アプリケーションを実行できます。

プロジェクトを実行した後、`docker images` コマンドを使用してイメージをリストすることができます。 Visual Studio 2017 でのプロジェクトの自動デプロイによって作成された `mssampleapplication` イメージが表示されるはずです。

```console
docker images
```

![docker images コマンドからのコンソール出力には、以下のものを含むリストが表示されます:リポジトリ、タグ、イメージ ID、作成 (日)、およびサイズ。](media/docker-images-command.png)

**図 4-40**. Docker イメージのビュー

## <a name="register-the-solution-in-the-azure-container-registry"></a>Azure Container Registry にソリューションを登録する

[Azure Container Registry (ACR)](https://azure.microsoft.com/services/container-registry/) や Docker Hub などの任意の Docker レジストリにイメージをアップロードし、イメージをそのレジストリから AKS クラスターにデプロイできるようにします。 この場合、イメージを Azure Container Registry にアップロードします。

### <a name="create-the-image-in-release-mode"></a>リリース モードでイメージを作成する

ここでは、図 4-41 に示すように、 **[リリース]** に変更し、前と同じようにアプリケーションを実行して、**リリース**モード (運用環境の準備ができている) でイメージを作成します。

![リリース モードでビルドするための VS のツール バー オプション。](media/select-release-mode.png)

**図 4-41**. リリース モードの選択

`docker image` コマンドを実行すると、作成された両方のイメージが表示されます。1 つは `debug` 用で、もう 1 つは `release` モード用です。

### <a name="create-a-new-tag-for-the-image"></a>イメージの新しいタグを作成する

各コンテナー イメージは、レジストリの名前 `loginServer` でタグ付けする必要があります。 このタグは、イメージ レジストリにコンテナー イメージをプッシュするときに、ルーティングするために使用されます。

Azure Container Registry から情報を取得し、Azure portal から `loginServer` という名前を表示できます

![右上に Azure Container Registry の名前が示されているブラウザー ビュー。](media/loginServer-name.png)

**図 4-42**. レジストリの名前のビュー

または、次のコマンドを実行します。

```console
az acr list --resource-group MSSampleResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

![上記のコマンドからのコンソール出力。](media/az-cli-loginServer-name.png)

**図 4-43**. PowerShell を使用してレジストリの名前を取得する

いずれの場合も、名前を取得します。 この例では、`mssampleacr.azurecr.io` です。

これで、コマンドを使用し、最新のイメージ (リリース イメージ) を取得して、イメージにタグを付けることができます。

```console
docker tag mssampleaksapplication:latest mssampleacr.azurecr.io/mssampleaksapplication:v1
```

`docker tag` コマンドを実行した後、`docker images` コマンドを使ってイメージをリストすると、新しいタグが付いたイメージが表示されるはずです。

![docker images コマンドからのコンソール出力。](media/tagged-docker-images-list.png)

**図 4-44**. タグが付けられたイメージのビュー

### <a name="push-the-image-into-the-azure-acr"></a>Azure ACR にイメージをプッシュする

Azure Container Registry にログインします

```console
az acr login --name mssampleacr
```

次のコマンドを使用して、Azure ACR にイメージをプッシュします。

```console
docker push mssampleacr.azurecr.io/mssampleaksapplication:v1
```

このコマンドでのイメージのアップロードにはしばらく時間がかかりますが、プロセス時にフィードバックが提供されます。

![docker push コマンドからのコンソール出力: レイヤーごとの文字ベースの進行状況が示されています。](media/uploading-image-to-acr.png)

**図 4-45**. ACR へのイメージのアップロード

プロセスの完了時に示される結果は以下のとおりです。

![docker push コマンドからのコンソール出力。すべてのレイヤーまたはノードで完了したことが示されています。](media/uploading-docker-images-complete.png)

**図 4-46**. ノードのビュー

次の手順では、AKS Kubernetes クラスターにコンテナーをデプロイします。 そのためには、以下を含むファイル ( **.yml デプロイ ファイル**) が必要です。

```yml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: mssamplesbook
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: mssample-kub-app
    spec:
      containers:
        - name: mssample-services-app
          image: mssampleacr.azurecr.io/mssampleaksapplication:v1
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
    name: mssample-kub-app
spec:
  ports:
    - name: http-port
      port: 80
      targetPort: 80
  selector:
    app: mssample-kub-app
  type: LoadBalancer
```

> [!NOTE]
> Kubernetes を使用するデプロイの詳細については、<https://kubernetes.io/docs/reference/kubectl/cheatsheet/> を参照してください

これで、**Kubectl** を使用してデプロイする準備がほぼ整いましたが、最初にこのコマンドを使用して、AKS クラスターに対する資格情報を取得する必要があります。

```console
az aks get-credentials --resource-group MSSampleResourceGroupAKS --name mssampleclusterk801
```

![上記のコマンドからのコンソール出力:Merged "MSSampleK8Cluster as current context in  /root/.kube/config](media/getting-aks-credentials.png)

**図 4-47**. 資格情報の取得

次に、`kubectl create` コマンドを使用して、デプロイを起動します。

```console
kubectl create -f mssample-deploy.yml
```

![上記のコマンドからのコンソール出力: deployment "mssamplesbook" created。 service "mssample-kub-app" created。](media/kubectl-create-command.png)

**図 4-48**. Kubernetes へのデプロイ

デプロイが完了したら、次のコマンドで、一時的にアクセスできるローカル プロキシを使用して Kubernetes コンソールにアクセスできます。

```console
az aks browse --resource-group MSSampleResourceGroupAKS --name mssampleclusterk801
```

その後、URL `http://127.0.0.1:8001` にアクセスします。

![デプロイ、ポッド、レプリカ セット、サービスを示す、Kubernetes ダッシュボードのブラウザー ビュー。](media/kubernetes-cluster-information.png)

**図 4-49**. Kubernetes クラスターの情報を表示する

これで、Linux コンテナーと AKS Kubernetes クラスターを使用して、Azure にアプリケーションをデプロイしました。 Azure portal から取得できる、サービスのパブリック IP を参照するアプリにアクセスできます。

> [!NOTE]
> このガイドの「[**Azure Kubernetes Service (AKS) へのデプロイ**](deploy-azure-kubernetes-service.md)」セクションで、このサンプル用の AKS クラスターを作成する方法を確認できます。

>[!div class="step-by-step"]
>[前へ](set-up-windows-containers-with-powershell.md)
>[次へ](../docker-devops-workflow/index.md)
