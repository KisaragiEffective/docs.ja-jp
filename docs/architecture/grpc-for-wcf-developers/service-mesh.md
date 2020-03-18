---
title: サービスメッシュ-WCF 開発者向け gRPC
description: サービスメッシュを使用して、Kubernetes クラスター内の gRPC サービスに要求をルーティングおよび分散します。
ms.date: 09/02/2019
ms.openlocfilehash: a29d6893e585c7eb60c847cef0149afeeaebcdab
ms.sourcegitcommit: f38e527623883b92010cf4760246203073e12898
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77503390"
---
# <a name="service-meshes"></a>サービスメッシュ

サービスメッシュは、ネットワーク内のルーティングサービス要求を制御するインフラストラクチャコンポーネントです。 サービスメッシュは、Kubernetes クラスター内のあらゆる種類のネットワークレベルの問題を処理できます。これには次のものが含まれる。

- サービスの検出
- 負荷分散
- フォールト トレランス
- 暗号化
- 監視

Kubernetes サービスメッシュは、メッシュに含まれる各ポッドにサイドカー*プロキシ*と呼ばれる追加のコンテナーを追加することによって機能します。 プロキシは、すべての受信および送信ネットワーク要求の処理を引き継ぎます。 その後、ネットワークの構成と管理は、アプリケーションコンテナーとは別に行うことができます。 多くの場合、この分離によってアプリケーションコードを変更する必要はありません。

[前の章の例](kubernetes.md#test-the-application)では、web アプリケーションからの grpc 要求はすべて、grpc サービスの1つのインスタンスにルーティングされていました。 これは、サービスのホスト名が IP アドレスに解決され、その IP アドレスが `HttpClientHandler` インスタンスの有効期間にわたってキャッシュされているために発生します。 DNS 参照を手動で処理したり、複数のクライアントを作成したりすることによって、この問題を回避できる場合があります。 ただし、この回避策を使用すると、ビジネスや顧客の価値を追加することなく、アプリケーションコードが複雑になります。

サービスメッシュを使用すると、アプリケーションコンテナーからの要求がサイドカープロキシに送信されます。 その後、サイドカープロキシは、他のサービスのすべてのインスタンスに対してインテリジェントな配信を行うことができます。 メッシュは次のようにすることもできます。

- サービスの個々のインスタンスの障害にシームレスに対応できます。
- 失敗した呼び出しまたはタイムアウトの再試行セマンティクスを処理します。
- クライアントアプリケーションに戻らずに、別のインスタンスに失敗した要求を再ルーティングします。

次のスクリーンショットは、Linkerd サービスメッシュで実行されている StockWeb アプリケーションを示しています。 アプリケーションコードが変更されていないため、Docker イメージが使用されていません。 必要な変更は、`stockdata` および `stockweb` サービスの YAML ファイルの配置に注釈を追加することだけでした。

![サービスメッシュを使用した StockWeb](media/service-mesh/stockweb-servicemesh-screenshot.png)

アプリケーションコードの1つの `HttpClient` インスタンスから発生した場合でも、StockWeb アプリケーションからの要求が Stockweb service の両方のレプリカにルーティングされていることを、 **[サーバー]** 列から確認できます。 実際には、コードを確認すると、同じ `HttpClient` インスタンスを使用して、StockData サービスに対する100要求がすべて同時に実行されていることがわかります。 サービスメッシュでは、これらの要求は分散されますが、多くのサービスインスタンスを使用できます。

サービスメッシュは、クラスター内のトラフィックにのみ適用されます。 外部クライアントの場合は、次の章「[負荷分散](load-balancing.md)」を参照してください。

## <a name="service-mesh-options"></a>サービスメッシュオプション

Kubernetes では、3つの汎用サービスメッシュ実装を現在使用できます。[Istio](https://istio.io)、 [Linkerd](https://linkerd.io)、 [consul Connect](https://consul.io/mesh.html)。 3つすべては、要求のルーティング/プロキシ、トラフィックの暗号化、回復性、ホストからホストへの認証、およびトラフィック制御を提供します。

サービスメッシュの選択は、次の複数の要因に依存します。

- コスト、コンプライアンス、有償サポートプランなどに関する組織固有の要件。
- クラスターの特性、サイズ、デプロイされたサービスの数、およびクラスターネットワーク内のトラフィックの量。
- メッシュのデプロイと管理を容易にし、サービスで使用できます。

## <a name="example-add-linkerd-to-a-deployment"></a>例:デプロイへの Linkerd の追加

この例では、[前のセクション](kubernetes.md)の*StockKube*アプリケーションで Linkerd service メッシュを使用する方法について説明します。
この例を実行するには、 [LINKERD CLI をインストール](https://linkerd.io/2/getting-started/#step-1-install-the-cli)する必要があります。 Windows バイナリは、GitHub リリースを一覧にしたセクションからダウンロードできます。 エッジリリースの1つではなく、最新の*安定*したリリースを使用してください。

Linkerd CLI がインストールされている状態で、[はじめに](https://linkerd.io/2/getting-started/index.html)の指示に従って、Kubernetes クラスターに Linkerd コンポーネントをインストールします。 手順は簡単であり、インストールにはローカル Kubernetes インスタンスで数分しかかかりません。

### <a name="add-linkerd-to-kubernetes-deployments"></a>Linkerd を Kubernetes デプロイに追加する

Linkerd CLI には、必要なセクションとプロパティを Kubernetes ファイルに追加するための `inject` コマンドが用意されています。 コマンドを実行し、出力を新しいファイルに書き込むことができます。

```console
linkerd inject stockdata.yml > stockdata-with-mesh.yml
linkerd inject stockweb.yml > stockweb-with-mesh.yml
```

新しいファイルを調べて、どのような変更が加えられたかを確認できます。 配置オブジェクトの場合、メタデータ注釈が追加され、Linkerd が作成時にサイドカープロキシコンテナーをポッドに挿入するように指示します。

パイプを使用して `linkerd inject` コマンドの出力を直接 `kubectl` にすることもできます。 次のコマンドは、PowerShell または任意の Linux シェルで動作します。

```console
linkerd inject stockdata.yml | kubectl apply -f -
linkerd inject stockweb.yml | kubectl apply -f -
```

### <a name="inspect-services-in-the-linkerd-dashboard"></a>Linkerd ダッシュボードでサービスを検査する

`linkerd` CLI を使用して、Linkerd ダッシュボードを開きます。

```console
linkerd dashboard
```

ダッシュボードには、メッシュに接続されているすべてのサービスに関する詳細情報が表示されます。

![StockKube アプリケーションを示す Linkerd ダッシュボード](media/service-mesh/linkerd-screenshot.png)

次の例に示すように StockData gRPC サービスのレプリカの数を増やした場合、ブラウザーで Stockdata ページを更新すると、 **[サーバー]** 列に id が混在していることがわかります。 このミックスは、使用可能なすべてのインスタンスが要求を処理していることを示します。

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
  replicas: 2 # Increase the target number of instances
  template:
    metadata:
      annotations:
        linkerd.io/inject: enabled
      creationTimestamp: null
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

>[!div class="step-by-step"]
>[前へ](kubernetes.md)
>[次へ](load-balancing.md)
