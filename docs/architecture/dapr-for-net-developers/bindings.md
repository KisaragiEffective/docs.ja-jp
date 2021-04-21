---
title: Dapr のバインド構成ブロック
description: バインド構成ブロック、その機能、利点、適用方法の説明
author: edwinvw
ms.date: 02/17/2021
ms.openlocfilehash: d6f8b2aa90b15e5b9cd7b5c29938660d1b2907e9
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2021
ms.locfileid: "103623955"
---
# <a name="the-dapr-bindings-building-block"></a>Dapr のバインド構成ブロック

Azure Functions や AWS Lambda などのクラウドベースの "*サーバーレス*" オファリングが、分散アーキテクチャの分野全体で広く普及するようになっています。 多くの利点の中でも、それらを使用すると、マイクロサービスで外部システム "*からのイベントを処理すること*" や、外部システム "*でイベントを呼び出すこと*" ができるようになり、基盤の複雑さや仕組みの問題を解消できます。 多数の外部リソースがあり、プラットフォームやベンダーが異なるデータストア、メッセージ システム、Web リソースが含まれます。 [Dapr のバインド構成ブロック](https://docs.dapr.io/developing-applications/building-blocks/bindings/bindings-overview/)を使用すると、これらの同じリソースに、Dapr アプリケーションの入口へのバインド機能が提供されます。

## <a name="what-it-solves"></a>解決される内容

Dapr のリソース バインドを使用すると、サービスで、直接関係するアプリケーションの外部にある外部リソースのビジネス操作を統合できます。 外部システムからのイベントにより、サービスの操作をトリガーし、コンテキスト情報を渡すことができます。 その後、サービスでは、別の外部システムでイベントをトリガーすることによって操作を拡張し、コンテキスト ペイロード情報を渡すことができます。 サービスで外部リソースを結合したり認識したりしなくても、通信することができます。 仕組みは、事前に定義された Dapr コンポーネント内にカプセル化されます。 使用する Dapr コンポーネントは、コードを変更することなく実行時に簡単に入れ替えることができます。

たとえば、ユーザーがキーワードをツイートたびにイベントをトリガーする Twitter アカウントについて考えます。 サービスで、ツイートを受信して処理するイベント ハンドラーを公開します。 完了すると、サービスによって、外部の Twilio サービスを呼び出すイベントがトリガーされます。 Twilio により、ツイートを含む SMS メッセージが送信されます。 図 8-1 は、この操作の概念アーキテクチャを示したものです。

![入力バインド](media/bindings/bindings-architecture.png)

**図 8-1**。 Dapr によるリソース バインドの概念アーキテクチャ。

一見すると、リソース バインドの動作は、前の記事で説明した[パブリッシュ-サブスクライブ パターン](publish-subscribe.md)と似ているように見えることがあります。 両者には似ている点もありますが、違いがあります。 パブリッシュ-サブスクライブの焦点は、Dapr サービス間の非同期通信です。 リソース バインドはさらに広い範囲を対象としたものです。 ソフトウェア プラットフォーム間のシステムの相互運用性に焦点が当てられています。 マイクロサービス アプリケーションの外部にある異なるアプリケーション、データストア、サービスの間での情報交換です。

## <a name="how-it-works"></a>しくみ

Dapr のリソース バインドは、コンポーネント構成ファイルから始まります。 この YAML ファイルでは、構成設定と共にバインドするリソースの種類を記述します。 構成が完了すると、サービスでリソースからイベントを受信したり、イベントをトリガーしたりできます。

> [!NOTE]
> バインドの構成については、後の「*コンポーネント*」セクションで詳しく説明します。

### <a name="input-bindings"></a>入力バインディング

入力バインドにより、外部リソースからの受信イベントでコードがトリガーされます。 イベントとデータを受信するには、サービスから "*イベント ハンドラー*" になるパブリック エンドポイントを登録します。 図 8-2 にそのフローを示します。

![Dapr の入力バインドのフロー](media/bindings/input-binding-flow.png)

**図 8-2**。 Dapr の入力バインドのフロー。

図 8.2 では、外部の Twitter アカウントからイベントを受信する手順について説明します。

1. Dapr サイドカーにより、バインド構成ファイルがを読み取られ、外部リソースに対して指定されたイベントがサブスクライブされます。 この例のイベント ソースは Twitter アカウントです。
1. 一致するツイートが Twitter で発行されると、Dapr サイドカーで実行されているバインド コンポーネントによってそれが取得され、イベントがトリガーされます。
1. Dapr サイドカーによって、バインド用に構成されたエンドポイント (つまり、イベント ハンドラー) が呼び出されます。 この例では、サービスにより、ポート 6000 の `/tweet` エンドポイントで HTTP POST がリッスンされています。 これは HTTP POST 操作であるため、イベントの JSON ペイロードが要求本文で渡されます。
1. イベントを処理した後、サービスからは HTTP 状態コード `200 OK` が返されます。

次の ASP.NET Core コントローラーでは、Twitter のバインドによってトリガーされたイベントを処理する例が示されています。

```csharp
[ApiController]
public class SomeController : ControllerBase
{
    public class TwitterTweet
    {
        [JsonPropertyName("id_str")]
        public string ID {get; set; }

        [JsonPropertyName("text")]
        public string Text {get; set; }
    }

    [HttpPost("/tweet")]
    public ActionResult Post(TwitterTweet tweet)
    {
        // Handle tweet
        Console.WriteLine("Tweet received: {0}: {1}", tweet.ID, tweet.Text);

        // ...

        // Acknowledge message
        return Ok();
    }
}
```

操作をエラーにする必要がある場合は、適切な 400 または 500 レベルの HTTP 状態コードを返します。 "*少なくとも 1 回の配信*" が保証されているバインドの場合は、Dapr サイドカーによってトリガーが再試行されます。 "*少なくとも 1 回*" または "*厳密に 1 回だけ*" のどちらの配信が保証されているかを確認するには、[リソース バインドに関する Dapr のドキュメント][1]を参照してください。

### <a name="output-bindings"></a>出力バインディング

Dapr には、"*出力バインド*" の機能も含まれています。 それを使用すると、外部リソースを呼び出すイベントをサービスでトリガーできます。 ここでも、最初に、出力バインドを記述するバインド構成 YAML ファイルを構成します。 その後、アプリケーションの Dapr サイドカーでバインド API を呼び出すイベントをトリガーします。 図 8-3 は、出力バインドのフローを示したものです。

![Dapr の出力バインドのフロー](media/bindings/output-binding-flow.png)

**図 8-3**。 Dapr の出力バインドのフロー。

1. Dapr サイドカーにより、外部リソースへの接続方法に関する情報を使用して、バインド構成ファイルが読み取られます。 この例の外部リソースは、Twilio SMS アカウントです。
1. アプリケーションによって、Dapr サイドカーの `/v1.0/bindings/sms` エンドポイントが呼び出されます。 この場合、API の呼び出しには HTTP POST が使用されます。 gRPC を使用することもできます。
1. Dapr サイドカーで実行されているバインド コンポーネントにより、外部メッセージング システムが呼び出されて、メッセージが送信されます。 メッセージには、POST 要求で渡されたペイロードが含まれます。

たとえば、curl を使用して Dapr API を呼び出すことにより、出力バインドを呼び出すことができます。

```console
curl -X POST http://localhost:3500/v1.0/bindings/sms \
  -H "Content-Type: application/json" \
  -d '{
        "data": "Welcome to this awesome service",
        "metadata": {
          "toNumber": "555-3277"
        },
        "operation": "create"
      }'
```

HTTP ポートは、Dapr サイドカーによって使用されるものと同じであることに注意してください (この場合は、既定の Dapr HTTP ポート `3500`)。

ペイロードの構造 (つまり、送信されるメッセージ) はバインドごとに異なります。 上の例のペイロードには、メッセージを含む `data` 要素が格納されています。 他の種類の外部リソースへのバインドは、異なる場合があります (特に、送信されるメタデータについて)。 各ペイロードには、バインドによって実行される操作を定義する `operation` フィールドも含まれている必要があります。 上の例では、SMS メッセージを作成する `create` 操作が指定されています。 一般的な操作は次のとおりです。

- create
- get
- delete
- list

バインドでサポートする操作は、バインドの作成者が決定します。 使用可能な操作とそれらを呼び出す方法については、各バインドのドキュメントで説明されています。

## <a name="using-the-dapr-net-sdk"></a>Dapr .NET SDK を使用する

Dapr .NET SDK により、.NET Core 開発者に言語固有のサポートが提供されます。 次の例では、`HttpClient.PostAsync()` の呼び出しが、`DaprClient.InvokeBindingAsync()` メソッドに置き換えられています。 この特殊なメソッドにより、構成されている出力バインドの呼び出しが簡単になります。

```csharp
private async Task SendSMSAsync([FromServices] DaprClient daprClient)
{
    var message = "Welcome to this awesome service";
    var metadata = new Dictionary<string, string>
    {
      { "toNumber", "555-3277" }
    };
    await daprClient.InvokeBindingAsync("sms", "create", message, metadata);
}
```

このメソッドは、`metadata` と `message` の値を受け取ります。

バインドを呼び出すために使用すると、`DaprClient` により gRPC を使用して Dapr サイドカーの Dapr API が呼び出されます。

## <a name="binding-components"></a>バインド コンポーネント

内部的には、リソース バインドは Dapr のバインド コンポーネントで実装されます。 それらはコミュニティによって提供され、Go で記述されています。 Dapr のバインドがまだ存在しない外部リソースと統合する必要がある場合は、自分で作成できます。 バインドを投稿する方法については、[Dapr components-contrib リポジトリ](https://github.com/dapr/components-contrib)を参照してください。

> [!NOTE]
> Dapr とそのすべてのコンポーネントは、[Golang](https://golang.org/) (Go) 言語で記述されています。 Go は、最新のクラウドネイティブなプログラミング プラットフォームと考えることができます。

バインドを構成するには、YAML 構成ファイルを使用します。 Twitter バインド用の構成の例を次に示します。

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: twitter-mention
  namespace: default
spec:
  type: bindings.twitter
  version: v1
  metadata:
  - name: consumerKey
    value: "****" # twitter api consumer key, required
  - name: consumerSecret
    value: "****" # twitter api consumer secret, required
  - name: accessToken
    value: "****" # twitter api access token, required
  - name: accessSecret
    value: "****" # twitter api access secret, required
  - name: query
    value: "dapr" # your search query, required
```

各バインド構成には、一般的な `metadata` 要素と `name` および `namespace` フィールドが含まれます。 Dapr により、構成されている `name` フィールドに基づいて、サービスを呼び出すエンドポイントが決定されます。 上の例では、イベントが発生すると、Dapr により、サービス内の `/twitter-mention` という注釈が付けられたメソッドが呼び出されます。

`spec` 要素では、バインド固有の `metadata` と共に、バインドの `type` を指定します。 この例では、API を使用して Twitter アカウントにアクセスするための資格情報が指定されています。 メタデータは、入力バインドと出力バインドで異なる場合があります。 たとえば、Twitter を入力バインドとして使用するには、`query` フィールドを使用して、ツイートで検索するテキストを指定する必要があります。 一致するツイートが送信されるたびに、Dapr サイドカーによってサービスの `/twitter-mention` エンドポイントが呼び出されます。 また、ツイートの内容も配信されます。

バインドは、入力と出力の一方または両方に対して構成できます。 興味深いことに、バインドで入力または出力の構成が明示的に指定されることはありません。 代わりに、方向は、バインドの使用方法と構成値によって推論されます。

使用可能なバインドの完全な一覧とその具体的な構成設定については、[リソース バインドに関する Dapr のドキュメント][1]を参照してください。

### <a name="cron-binding"></a>Cron バインド

Dapr の Cron バインドに細心の注意を払ってください。 それは、外部システムのイベントをサブスクライブするものではありません。 そうではなく、このバインドは、構成可能な間隔スケジュールを使用して、アプリケーションをトリガーするために使用します。 このバインドを使用すると、バックグラウンド ワーカーを実装して、一定の間隔で何らかの処理を実行するための簡単な方法が提供されます。これにより、構成可能な遅延でエンドレス ループを実装する必要がなくなります。 Cron バインドの構成の例を次に示します。

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: checkOrderBacklog
  namespace: default
spec:
  type: bindings.cron
  version: v1
  metadata:
  - name: schedule
    value: "@every 30m"
```

この例では、Dapr により 30 分ごとに `/checkOrderBacklog` エンドポイントが呼び出されて、サービスがトリガーされます。 `schedule` の値を指定するには、複数のパターンを使用できます。 詳細については、[Cron バインドのドキュメント](https://docs.dapr.io/operations/components/setup-bindings/supported-bindings/cron/)を参照してください。

## <a name="reference-application-eshopondapr"></a>参照アプリケーション: eShopOnDapr

付属する eShopOnDapr 参照アプリケーションで、出力バインドの例が実装されています。 新しい注文が行われると、Dapr の [SendGrid](https://docs.dapr.io/operations/components/setup-bindings/supported-bindings/sendgrid/) バインドがトリガーされて、ユーザーにメールが送信されます。 このバインドは、コンポーネント フォルダー内の `eshop-email.yaml` ファイルにあります。

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: sendmail
  namespace: eshop
spec:
  type: bindings.twilio.sendgrid
  version: v1
  metadata:
  - name: apiKey
    secretKeyRef:
      name: sendGridAPIKey
auth:
  secretStore: eshop-secretstore
```

この構成には、[Twilio SendGrid](https://github.com/dapr/components-contrib/tree/master/bindings/twilio) バインド コンポーネントが使用されます。 サービスに接続するための API キーでの、Dapr シークレット参照の使用方法に注意してください。 この方法により、シークレットが構成ファイルの外部に保持されます。 Dapr のシークレットの詳細については、[シークレット構成ブロックに関する章](secrets.md)を参照してください。

バインド構成で指定されているバインド コンポーネントは、Dapr サイドカーの `/sendmail` エンドポイントを使用して呼び出すことができます。 注文が開始されるたびにメールを送信するコード スニペットを次に示します。

```csharp
public Task Handle(OrderStartedDomainEvent notification, CancellationToken cancellationToken)
{
    var string message = CreateEmailBody(notification);
    var metadata = new Dictionary<string, string>
    {
        {"emailFrom", "eShopOn@dapr.io"},
        {"emailTo", notification.UserName},
        {"subject", $"Your eShopOnDapr order #{notification.Order.Id}"}
    };
    return _daprClient.InvokeBindingAsync("sendmail", "create", message, metadata, cancellationToken);
}
```

この例でわかるように、メッセージ本文は `message` に含まれます。 `CreateEmailBody` メソッドは、本文のテキストで文字列の書式を設定するだけです。 `metadata` により、メールの送信者、受信者、メール メッセージの件名が指定されます。 これらの値が静的である場合は、構成ファイルのメタデータ フィールドに含めることもできます。 呼び出すバインドの名前は `sendmail` であり、操作は `create` です。

## <a name="summary"></a>まとめ

Dapr のリソース バインドを使用すると、ライブラリや SDK に依存することなく、さまざまな外部リソースやシステムと統合できます。 これらの外部システムは、必ずしもサービス バスやメッセージ ブローカーなどのメッセージング システムである必要はありません。 また、データストアや Web リソース (Twitter や SendGrid など) のためのバインドも存在します。

入力バインド (トリガー) は、外部システムで発生するイベントに反応します。 それにより、アプリケーションで事前に構成されているパブリック HTTP エンドポイントが呼び出されます。 アプリケーションで呼び出すエンドポイントを決定するには、構成内のバインドの名前が Dapr によって使用されます。

出力バインドにより、外部システムにメッセージが送信されます。 出力バインドをトリガーするには、Dapr サイドカーの `/v1.0/bindings/<binding-name>` エンドポイントで HTTP POST を実行します。 gRPC を使用してバインドを呼び出すこともできます。 .NET SDK には、gRPC を使用して Dapr バインドを呼び出すための `InvokeBindingAsync` メソッドが用意されています。

バインドは Dapr コンポーネントで実装します。 これらのコンポーネントはコミュニティによって提供されています。 各バインド コンポーネントの構成には、それによって抽象化される外部システムに固有のメタデータが含まれています。 また、それによってサポートされるコマンドと、ペイロードの構造は、バインド コンポーネントごとに異なります。

### <a name="references"></a>リファレンス

- [リソース バインドに関する Dapr のドキュメント](https://docs.dapr.io/operations/components/setup-bindings/supported-bindings/)

>[!div class="step-by-step"]
>[前へ](publish-subscribe.md)
>[次へ](observability.md)
