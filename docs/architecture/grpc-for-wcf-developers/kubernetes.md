---
title: WCF 開発者向け Kubernetes-gRPC
description: Kubernetes クラスターで ASP.NET Core gRPC サービスを実行しています。
ms.date: 09/02/2019
ms.openlocfilehash: 22271343f8f0d0454469b2f35e717f5b7e939294
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74711288"
---
# <a name="kubernetes"></a>Kubernetes

Docker ホストでコンテナーを手動で実行することもできますが、信頼性の高い運用システムの場合は、コンテナーオーケストレーションエンジンを使用して、クラスター内の複数のサーバーで実行されている複数のインスタンスを管理することをお勧めします。 さまざまなコンテナーオーケストレーションエンジンが用意されています。これには、Kubernetes、Docker の群れ、Apache のシステムなどがあります。 しかし、これらのエンジンでは、Kubernetes ははるかに広く使用されているので、この章で重点的に説明します。

Kubernetes には、次の機能が含まれています。

- **スケジュール設定**では、クラスター内の複数のノードでコンテナーを実行し、使用可能なリソースのバランスの取れた使用状況を確保し、障害が発生した場合はコンテナーを実行したままにして、新しいバージョンのイメージまたは新しい構成へのローリング更新を処理します。
- **正常性チェック**は、コンテナーを監視してサービスを継続的に確認します。
- **DNS & サービス検出**は、クラスター内のサービス間のルーティングを処理します。
- 受信は、選択したサービスを外部**に公開し**、一般にこれらのサービスのインスタンス間で負荷分散を行います。
- **リソース管理**は、ストレージなどの外部リソースをコンテナーにアタッチします。

この章では、ASP.NET Core gRPC サービスと、サービスを使用する web サイトを Kubernetes クラスターにデプロイする方法について詳しく説明します。 使用されるサンプルアプリケーションは、GitHub の[dotnet/grpc-wcf 開発者](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/KubernetesSample)リポジトリで入手できます。

## <a name="kubernetes-terminology"></a>Kubernetes の用語

Kubernetes は*desired state configuration*を使用します。この API は*ポッド*、*デプロイ*、*サービス*などのオブジェクトを表すために使用され、*コントロールプレーン*は*クラスター*内のすべての*ノード*で目的の状態を実装します。 Kubernetes クラスターには、 *KUBERNETES API*を実行する*マスター*ノードがあります。このノードはプログラムによって、または `kubectl` コマンドラインツールを使用して通信できます。 `kubectl` は、コマンドライン引数を使用してオブジェクトを作成および管理できますが、Kubernetes オブジェクトの宣言データを含む YAML ファイルで最適に動作します。

### <a name="kubernetes-yaml-files"></a>Kubernetes YAML ファイル

すべての Kubernetes YAML ファイルには、少なくとも3つの最上位レベルのプロパティがあります。

```yaml
apiVersion: v1
kind: Namespace
metadata:
  # Object properties
```

`apiVersion` プロパティは、ファイルの対象となるバージョン (および API) を指定するために使用されます。 `kind` プロパティは、YAML が表すオブジェクトの種類を指定します。 `metadata` プロパティには、`name`、`namespace`、`labels`などのオブジェクトプロパティが含まれています。

ほとんどの Kubernetes YAML ファイルには、オブジェクトの作成に必要なリソースと構成を説明する `spec` セクションもあります。

### <a name="pods"></a>ポッド

ポッドは、Kubernetes での実行の基本単位です。 複数のコンテナーを実行できますが、1つのコンテナーを実行するためにも使用されます。 ポッドには、コンテナーに必要なすべてのストレージリソースと、ネットワークの IP アドレスも含まれています。

### <a name="services"></a>Services

サービスは、ポッド (またはポッドのセット) を記述し、クラスター内でそれらにアクセスする手段を提供するメタオブジェクトです。たとえば、クラスター DNS サービスを使用して、サービス名をポッド IP アドレスのセットにマップすることができます。

### <a name="deployments"></a>展開

展開は、ポッドに*必要な状態*オブジェクトです。 ポッドを手動で作成した場合は、終了時に再起動されません。 デプロイは、現在の時点で実行する必要があるポッド、およびそれらのポッドのレプリカの数をクラスターに伝えるために使用されます。

### <a name="other-objects"></a>その他のオブジェクト

ポッド、サービス、および展開は、最も基本的なオブジェクトの種類のうちの3つにすぎません。 Kubernetes クラスターによって管理されるオブジェクトの種類は他にも多数あります。 詳細については、 [Kubernetes の概念](https://kubernetes.io/docs/concepts/)に関するドキュメントを参照してください。

### <a name="namespaces"></a>名前空間

Kubernetes クラスターは、数百または数千のノードに拡張し、同様の数のサービスを実行するように設計されています。 オブジェクト名が競合しないようにするために、名前空間を使用して、大きなアプリケーションの一部としてオブジェクトをグループ化します。 Kubernetes の独自のサービスは `default` 名前空間で実行されます。 既定のオブジェクトまたはクラスター内の他のテナントとの潜在的な競合を避けるために、すべてのユーザーオブジェクトを独自の名前空間に作成する必要があります。

## <a name="get-started-with-kubernetes"></a>Kubernetes を使ってみる

Windows 用の Docker Desktop または Mac 用 Docker デスクトップを実行している場合、Kubernetes は既に使用可能です。 **[設定]** ウィンドウの **[Kubernetes]** セクションで有効にするだけです。

![Docker Desktop で Kubernetes を有効にする](media/kubernetes/enable-kubernetes-docker-desktop.png)

Linux でローカル Kubernetes クラスターを実行する場合は、Linux ディストリビューションで[スナップ](https://snapcraft.io/)がサポートされている場合は[Minikube](https://github.com/kubernetes/minikube)または[MicroK8s](https://microk8s.io/)を検討してください。

クラスターが実行されていて、アクセス可能であることを確認するには、`kubectl version` コマンドを実行します。

```console
kubectl version
Client Version: version.Info{Major:"1", Minor:"14", GitVersion:"v1.14.6", GitCommit:"96fac5cd13a5dc064f7d9f4f23030a6aeface6cc", GitTreeState:"clean", BuildDate:"2019-08-19T11:13:49Z", GoVersion:"go1.12.9", Compiler:"gc", Platform:"windows/amd64"}
Server Version: version.Info{Major:"1", Minor:"14", GitVersion:"v1.14.6", GitCommit:"96fac5cd13a5dc064f7d9f4f23030a6aeface6cc", GitTreeState:"clean", BuildDate:"2019-08-19T11:05:16Z", GoVersion:"go1.12.9", Compiler:"gc", Platform:"linux/amd64"}
```

この例では、`kubectl` CLI と Kubernetes サーバーの両方でバージョン1.14.6 が実行されています。 `kubectl` の各バージョンは、サーバーの以前のバージョンと次のバージョンをサポートすることが想定されています。そのため、`kubectl` 1.14 は、サーバーバージョン1.13 と1.15 でも動作します。

## <a name="run-services-on-kubernetes"></a>Kubernetes でサービスを実行する

サンプルアプリケーションには、3つの YAML ファイルを含む `kube` ディレクトリがあります。 `namespace.yml` ファイルは、`stocks`カスタム名前空間を宣言します。 `stockdata.yml` ファイルは、gRPC アプリケーションのデプロイとサービスを宣言し、`stockweb.yml` ファイルは、gRPC サービスを使用する ASP.NET Core 3.0 MVC web アプリケーションのデプロイとサービスを宣言します。

`kubectl`で `YAML` ファイルを使用するには、`apply -f` のコマンドを実行します。

```console
kubectl apply -f object.yml
```

`apply` コマンドは、YAML ファイルの有効性を確認し、API から受信したすべてのエラーを表示しますが、ファイルで宣言されたすべてのオブジェクトが作成されるまで待機しません。これは時間がかかることがあるためです。 `kubectl get` コマンドと関連するオブジェクトの種類を使用して、クラスターでのオブジェクトの作成を確認します。

### <a name="the-namespace-declaration"></a>名前空間の宣言

名前空間の宣言は単純であり、`name`の割り当てのみを必要とします。

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: stocks
```

`kubectl` を使用して、`namespace.yml` ファイルを適用し、名前空間が正常に作成されたことを確認します。

```console
> kubectl apply -f namespace.yml
namespace/stocks created

> kubectl get namespaces
NAME              STATUS   AGE
stocks            Active   2m53s
```

### <a name="the-stockdata-application"></a>StockData アプリケーション

`stockdata.yml` ファイルでは、配置とサービスという2つのオブジェクトを宣言します。

#### <a name="the-stockdata-deployment"></a>StockData のデプロイ

YAML ファイルの配置部分には、必要なレプリカの数や、デプロイによって作成および管理される Pod オブジェクトの `template` など、デプロイ自体の `spec` が用意されています。 配置オブジェクトは、メインの Kubernetes API ではなく `apiVersion`で指定されている `apps` API によって管理されることに注意してください。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: stockdata
  namespace: stocks
spec:
  selector:
    matchLabels:
      run: stockdata
  replicas: 1
  template:
    metadata:
      labels:
        run: stockdata
    spec:
      containers:
      - name: stockdata
        image: stockdata:1.0.0
        imagePullPolicy: Never
        resources:
          limits:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 80
```

`spec.selector` プロパティは、実行中のポッドをデプロイと照合するために使用されます。 ポッドの `metadata.labels` プロパティが `matchLabels` プロパティと一致している必要があります。一致しない場合、API 呼び出しは失敗します。

`template.spec` セクションでは、実行するコンテナーを宣言します。 Docker Desktop によって提供されるものなど、ローカル Kubernetes クラスターを使用している場合は、バージョンタグがある限り、ローカルでビルドされたイメージを指定できます。

> [!IMPORTANT]
> 既定では、Kubernetes は常にを確認し、新しいイメージをプルしようとします。 既知のリポジトリにイメージが見つからない場合、ポッドの作成は失敗します。 ローカルイメージを操作するには、`imagePullPolicy` を `Never`に設定します。

`ports` プロパティは、ポッドで公開するコンテナーポートを指定します。 `stockservice` イメージは標準の HTTP ポートでサービスを実行するので、ポート80が発行されます。

`resources` セクションでは、ポッド内で実行されているコンテナーにリソースの制限を適用します。 これは、個々のポッドがノード上の使用可能な CPU またはメモリをすべて消費しないようにするため、この方法が適しています。

> [!NOTE]
> ASP.NET Core 3.0 は、リソースが制限されたコンテナーで実行するように最適化およびチューニングされています。 `dotnet/core/aspnet` Docker イメージは、`dotnet` ランタイムにコンテナー内にあることを通知する環境変数を設定します。

#### <a name="the-stockdata-service"></a>StockData サービス

YAML ファイルのサービス部分では、クラスター内のポッドへのアクセスを提供するサービスを宣言します。

```yaml
apiVersion: v1
kind: Service
metadata:
  name: stockdata
  namespace: stocks
spec:
  ports:
  - port: 80
  selector:
    run: stockdata
```

サービス `spec` は、`selector` プロパティを使用して実行中の `Pods`を照合します。この例では、ラベル `run: stockdata`を持つポッドを探しています。 一致するポッドに対して指定された `port` は、名前付きサービスによって発行されます。 `stocks` 名前空間で実行されている他のポッドは、アドレスとして `http://stockdata` を使用して、このサービスの HTTP にアクセスできます。 他の名前空間で実行されているポッドは、`http://stockdata.stocks` ホスト名を使用できます。 [ネットワークポリシー](https://kubernetes.io/docs/concepts/services-networking/network-policies/)を使用して、名前空間間サービスアクセスを制御できます。

#### <a name="deploy-the-stockdata-application"></a>StockData アプリケーションをデプロイする

`kubectl` を使用して、`stockdata.yml` ファイルを適用し、展開とサービスが作成されたことを確認します。

```console
> kubectl apply -f .\stockdata.yml
deployment.apps/stockdata created
service/stockdata created

> kubectl get deployment stockdata --namespace stocks
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
stockdata   1/1     1            1           17s

> kubectl get service stockdata --namespace stocks
NAME        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
stockdata   ClusterIP   10.97.132.103   <none>        80/TCP    33s
```

### <a name="the-stockweb-application"></a>StockWeb アプリケーション

`stockweb.yml` ファイルは、MVC アプリケーションのデプロイとサービスを宣言します。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: stockweb
  namespace: stocks
spec:
  selector:
    matchLabels:
      run: stockweb
  replicas: 1
  template:
    metadata:
      labels:
        run: stockweb
    spec:
      containers:
      - name: stockweb
        image: stockweb:1.0.0
        imagePullPolicy: Never
        resources:
          limits:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 80
        env:
        - name: StockData__Address
          value: "http://stockdata"
        - name: DOTNET_SYSTEM_NET_HTTP_SOCKETSHTTPHANDLER_HTTP2UNENCRYPTEDSUPPORT
          value: "true"

---

apiVersion: v1
kind: Service
metadata:
  name: stockweb
  namespace: stocks
spec:
  type: NodePort
  ports:
  - port: 80
  selector:
    run: stockweb
```

#### <a name="environment-variables"></a>環境変数

Deployment オブジェクトの `env` セクションでは、`stockweb:1.0.0` イメージを実行しているコンテナーに設定する環境変数を指定します。

EnvironmentVariables 構成プロバイダーにより、 **`StockData__Address`** 環境変数が `StockData:Address` 構成設定にマップされます。 この設定では、名前の間に2つのアンダースコアを使用し、セクションを区切ります。 このアドレスは、同じ Kubernetes 名前空間で実行されている `stockdata` サービスのサービス名を使用します。

**`DOTNET_SYSTEM_NET_HTTP_SOCKETSHTTPHANDLER_HTTP2UNENCRYPTEDSUPPORT`** 環境変数は、<xref:System.Net.Http.HttpClient>に対して暗号化されていない HTTP/2 接続を有効にする <xref:System.AppContext> スイッチを設定します。 この環境変数は、次に示すように、コードでスイッチを設定するのと同じことを行います。

```csharp
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);
```

スイッチに環境変数を使用すると、アプリケーションが実行されているコンテキストに応じてコンテキストを簡単に変更できます。

#### <a name="service-types"></a>サービスの種類

`type: NodePort` プロパティは、クラスターの外部から web アプリケーションにアクセスできるようにするために使用されます。 このプロパティの種類を Kubernetes と、サービスのポート80がクラスターの外部ネットワークソケットの任意のポートに発行されます。 割り当てられたポートは、`kubectl get service` コマンドを使用して見つけることができます。

`stockdata` サービスにはクラスターの外部からアクセスできないため、既定の種類である `ClusterIP`が使用されます。

実稼働システムでは、通常、統合されたロードバランサーを使用して、パブリックアプリケーションを外部のコンシューマーに公開します。 この方法で公開されるサービスでは、`LoadBalancer` の種類を使用する必要があります。

サービスの種類の詳細については、 [Kubernetes Publishing Services](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types)のドキュメントを参照してください。

#### <a name="deploy-the-stockweb-application"></a>StockWeb アプリケーションをデプロイする

`kubectl` を使用して、`stockweb.yml` ファイルを適用し、展開とサービスが作成されたことを確認します。

```console
> kubectl apply -f .\stockweb.yml
deployment.apps/stockweb created
service/stockweb created

> kubectl get deployment stockweb --namespace stocks
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
stockweb   1/1     1            1           8s

> kubectl get service stockweb --namespace stocks
NAME       TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
stockweb   NodePort   10.106.141.5   <none>        80:32564/TCP   13s
```

`get service` コマンドの出力は、HTTP ポートが外部ネットワーク上のポート32564に発行されていることを示しています。 Docker Desktop の場合、これは localhost になります。 アプリケーションにアクセスするには、`http://localhost:32564`を参照してください。

### <a name="test-the-application"></a>アプリのテスト

StockWeb アプリケーションでは、単純な要求/応答サービスから取得された NASDAQ 株式の一覧が表示されます。 このデモでは、各行には、それを返したサービスインスタンスの一意の ID も表示されます。

![StockWeb スクリーンショット](media/kubernetes/stockweb-screenshot.png)

`stockdata` サービスのレプリカの数が増加した場合は、**サーバー**の値が行ごとに変更されることが予想されますが、実際には、100のすべてのレコードが常に同じインスタンスから返されます。 数秒ごとにページを更新した場合、サーバー ID は変わりません。 なぜですか。 ここでは2つの要素について説明します。

まず、Kubernetes Service discovery システムは、既定でラウンドロビンの負荷分散を使用します。 最初に DNS サーバーにクエリを実行すると、サービスに対して最初に一致する IP アドレスが返されます。 次に、リスト内の次の IP アドレスを返します。これは最後まで続きます。 その時点で、開始にループバックします。

次に、StockWeb アプリケーションの gRPC クライアントに使用される `HttpClient` が[ASP.NET Core HttpClientFactory](../microservices/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests.md)によって作成および管理され、このクライアントの1つのインスタンスがページの呼び出しごとに使用されます。 クライアントは DNS 参照を1つだけ行うため、すべての要求が同じ IP アドレスにルーティングされます。 また、パフォーマンス上の理由により、`HttpClientHandler` がキャッシュされているため、複数の要求が同時に同じ IP アドレスを使用します。キャッシュ*された*DNS エントリの有効期限が切れるか、ハンドラーインスタンスが何らかの理由で破棄されるまでです。

その結果、既定では、gRPC サービスへの要求は、クラスター内のそのサービスのすべてのインスタンスでバランスが取れていないことになります。 コンシューマーによって使用されるインスタンスは異なりますが、要求の配布やリソースのバランスの取れた使用は保証されません。

次の章「[サービスメッシュ](service-mesh.md)」では、この問題に対処します。

>[!div class="step-by-step"]
>[前へ](docker.md)
>[次へ](service-mesh.md)
