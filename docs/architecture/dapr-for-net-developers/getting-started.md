---
title: Dapr の概要
description: ローカル開発環境を準備し、Dapr を使用して初めての .NET アプリケーションを構築するためのガイドです。
author: amolenk
ms.date: 02/25/2021
ms.openlocfilehash: 1c60f731138911d7d22ff871c9a3849704d81dd6
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874382"
---
# <a name="get-started-with-dapr"></a>Dapr の概要

最初の 2 つの章では、Dapr に関する基本的な概念を学習しました。 いよいよ "*体験版*" を試してみましょう。 この章では、ローカル開発環境を準備し、2 つの Dapr .NET アプリケーションを構築する手順について説明します。

## <a name="install-dapr-into-your-local-environment"></a>ローカル環境に Dapr をインストールする

まず、お使いの開発用コンピューターに Dapr をインストールします。 それが完了すると、Dapr アプリケーションを[セルフホステッド モード](https://docs.dapr.io/operations/hosting/self-hosted/self-hosted-overview/)で構築して実行できます。

1. [Dapr CLI をインストールします](https://docs.dapr.io/getting-started/install-dapr-cli/)。 これにより、Dapr インスタンスを起動、実行、管理できるようになります。 また、デバッグのサポートも提供されます。

1. [Docker Desktop](https://docs.docker.com/get-docker/) をインストールします。 Windows で実行している場合、**Docker Desktop for Windows** が Linux コンテナーを使用するように構成されていることを確認します。

> [!NOTE]
> 既定では、Dapr で Docker コンテナーを使用すると、何もしなくても最適なエクスペリエンスが提供されます。 Docker の外部で Dapr を実行するには、このステップを省略して、["*スリム*" 初期化を実行する](https://docs.dapr.io/operations/hosting/self-hosted/self-hosted-no-docker/)ことができます。 この章の例では、Docker コンテナーを使用する必要があります。

1. [Dapr を初期化します](https://docs.dapr.io/getting-started/install-dapr/)。 このステップでは、最新の Dapr バイナリとコンテナー イメージをインストールして、開発環境を設定します。

1. [.NET Core 3.1 SDK をインストールします](https://dotnet.microsoft.com/download/dotnet/3.1)。

Dapr がインストールされたので、いよいよ最初の Dapr アプリケーションを構築します。

## <a name="build-your-first-dapr-application"></a>最初の Dapr アプリケーションを構築する

最初に、[Dapr 状態管理](state-management.md)ビルディング ブロックを使用する単純な .NET コンソール アプリケーションを作成します。

### <a name="create-the-application"></a>アプリケーションを作成する

1. 任意のコマンド シェルまたはターミナルを開きます。 [Visual Studio Code](https://code.visualstudio.com/) のターミナル機能を使用できます。 アプリケーションを構築するルート フォルダーに移動します。 そこで、次のコマンドを入力して、新しい .NET コンソール アプリケーションを作成します。

    ```dotnetcli
    dotnet new console -o DaprCounter
    ```

    このコマンドにより、単純な "Hello World" .NET Core アプリケーションがスキャフォールディングされます。

1. 次に、前のコマンドで作成された新しいディレクトリに移動します。

    ```console
    cd DaprCounter
    ```

1. `dotnet run` コマンドを使用して、新しく作成したアプリケーションを実行します。 これにより、"Hello World!" と コンソール画面に出力されます。

    ```dotnetcli
    dotnet run
    ```

### <a name="add-dapr-state-management"></a>Dapr 状態管理を追加する

次に、Dapr [状態管理構成ブロック](https://docs.dapr.io/developing-applications/building-blocks/state-management/state-management-overview/)を使用して、プログラムにステートフル カウンターを実装します。

Dapr による HTTP と gRPC のネイティブ サポートを使用して、任意の開発プラットフォームで Dapr API を呼び出すことができます。 ただし、.NET の開発者は、Dapr .NET SDK を使用するといっそう自然でわかりやすいと感じるでしょう。 Dapr API を呼び出すための、厳密に型指定された .NET クライアントが用意されています。 .NET SDK は ASP.NET Core とも緊密に統合されます。

1. ターミナル ウィンドウから、`Dapr.Client` NuGet パッケージをアプリケーションに追加します。

    ```dotnetcli
    dotnet add package Dapr.Client
    ```

    > [!NOTE]
    > Dapr のプレリリース版を使用している場合は、必ず `--prerelease` フラグをコマンドに追加してください。

1. 好みのエディターで `Program.cs` ファイルを開き、その内容を次のコードに更新します。

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Dapr.Client;

    namespace DaprCounter
    {
        class Program
        {
            static async Task Main(string[] args)
            {
                const string storeName = "statestore";
                const string key = "counter";

                var daprClient = new DaprClientBuilder().Build();
                var counter = await daprClient.GetStateAsync<int>(storeName, key);

                while (true)
                {
                    Console.WriteLine($"Counter = {counter++}");

                    await daprClient.SaveStateAsync(storeName, key, counter);
                    await Task.Delay(1000);
                }
            }
        }
    }
    ```

    更新されたコードにより、次の手順が実装されます。

    - 最初に、新しい `DaprClient` インスタンスがインスタンス化されます。 このクラスを使用して、Dapr サイドカーと対話できます。
    - `DaprClient.GetStateAsync` により、状態ストアから `counter` キーの値がフェッチされます。 キーが存在しない場合は、既定の `int` 値 (`0` です) が返されます。
    - その後、コードによって反復処理が行われ、`counter` の値がコンソールに出力されて、増分された値が状態ストアに保存されます。

1. Dapr CLI の `run` コマンドにより、アプリケーションが開始されます。 それによって基になる Dapr ランタイムが呼び出され、アプリケーションと Dapr サイドカーの両方が同時に実行できるようになります。 `app-id` を省略した場合は、Dapr によってアプリケーションの一意の名前が生成されます。 コマンドの最後のセグメント `dotnet run` では、Dapr ランタイムに対して .NET Core アプリケーションの実行が指示されます。

    > [!IMPORTANT]
    > 状態管理構成ブロックを使用する場合は、常に、明示的な `app-id` パラメーターを渡す必要があることに注意してください。 アプリケーション ID の値は、ブロックによって、キーと値の各ペアで状態キーの "*プレフィックス*" として使用されます。 アプリケーション ID が変わった場合、以前に保存された状態にアクセスできなくなります。

    ここで、次のコマンドを使用してアプリケーションを実行します。

    ```console
    dapr run --app-id DaprCounter dotnet run
    ```

    アプリケーションを停止して再起動してみてください。 カウンターがリセットされないことがわかります。 代わりに、前に保存された状態から継続されます。 Dapr の構成ブロックにより、アプリケーションはステートフルになります。

> [!IMPORTANT]
> サンプルのアプリケーションは事前に構成された状態コンポーネントと通信しているのであり、それへの直接的な依存関係はないということを理解しておくことが重要です。 Dapr によって依存関係は抽象化されます。 すぐにわかるように、基になる状態ストア コンポーネントは、簡単な構成の更新で変更できます。

状態が実際にどこに保存されているのか、疑問に思うかもしれません。

## <a name="component-configuration-files"></a>コンポーネント構成ファイル

ローカル環境用に Dapr を最初に初期化したときに、Redis コンテナーが自動的にプロビジョニングされています。 その後、Redis コンテナーは、Dapr により、`statestore.yaml` という名前のコンポーネント構成ファイルを使用して、既定の状態ストア コンポーネントとして構成されています。 その内容は次のようになります。

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
spec:
  type: state.redis
  version: v1
  metadata:
  - name: redisHost
    value: localhost:6379
  - name: redisPassword
    value: ""
  - name: actorStateStore
    value: "true"
```

> [!NOTE]
> 既定のコンポーネント構成ファイルは、`$HOME/.dapr/components` フォルダー (Linux と macOS の場合) および `%USERPROFILE%\.dapr\components` フォルダー (Windows の場合) に格納されています。

前記のコンポーネント構成ファイルの形式に注意してください。

- 各コンポーネントに名前があります。 上のサンプルでは、コンポーネントの名前は `statestore` です。 最初のコードの例で、使用するコンポーネントを Dapr サイドカーに指示するために、この名前を使用しました。
- 各コンポーネント構成ファイルには `spec` セクションがあります。 そこには、コンポーネントの種類を指定する `type` フィールドが含まれています。 `version` フィールドでは、コンポーネントのバージョンが指定されています。 `metadata` フィールドには、接続の詳細やその他の設定など、コンポーネントに必要な情報が含まれています。 メタデータの値は、コンポーネントの種類によって異なります。

Dapr サイドカーは、アプリケーションで構成されている任意の Dapr コンポーネントを使用できます。 しかし、アーキテクチャ上の理由で、コンポーネントへのアクセスを制限する必要がある場合はどうでしょう。 Redis コンポーネントを、運用環境で実行されている Dapr サイドカーだけに制限するにはどうすればよいでしょうか。

そのためには、運用環境の `namespace` を定義します。 `production` のような名前を使用します。 セルフホステッド モードの場合は、`NAMESPACE` 環境変数を設定することによって、Dapr サイドカーの名前空間を指定します。 このように構成すると、名前空間に一致するコンポーネントのみが、Dapr サイドカーによって読み込まれます。 Kubernetes の展開の場合は、Kubernetes の名前空間によって、読み込まれるコンポーネントが決まります。 次のサンプルは、`production` 名前空間に配置された Redis コンポーネントを示したものです。 `metadata` 要素内の `namespace` の宣言に注意してください。

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
  namespace: production
spec:
  type: state.redis
  version: v1
  metadata:
  - name: redisHost
    value: localhost:6379
  - name: redisPassword
    value: ""
  - name: actorStateStore
    value: "true"
```

> [!IMPORTANT]
> 名前空間を指定されたコンポーネントには、同じ名前空間で実行されているアプリケーションからのみアクセスできます。 Dapr アプリケーションでコンポーネントの読み込みが失敗する場合は、アプリケーションの名前空間がコンポーネントの名前空間と一致していることを確認してください。 これは、アプリケーションの名前空間が `NAMESPACE` 環境変数に格納されるセルフホステッド モードでは特に注意が必要です。

必要な場合は、コンポーネントを特定のアプリケーションにさらに限定することもできます。 `production` 名前空間内で、Redis キャッシュへのアクセスを `DaprCounter` アプリケーションのみに制限することができます。 これを行うには、コンポーネントの構成で `scopes` を指定します。 次に示すのは、Redis の `statestore` コンポーネントへのアクセスを、`production` 名前空間内の `DaprCounter` アプリケーションに制限する方法の例です。

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
  namespace: production
spec:
  type: state.redis
  version: v1
  metadata:
  - name: redisHost
    value: localhost:6379
  - name: redisPassword
    value: ""
  - name: actorStateStore
    value: "true"
  scopes:
  - DaprCounter
```

## <a name="build-a-multi-container-dapr-application"></a>マルチコンテナーの Dapr アプリケーションを構築する

最初の例では、Dapr サイドカーとサイドバイサイドで実行される簡単な .NET コンソール アプリケーションを作成しました。 しかし、最新の分散アプリケーションは、通常、多くの可動部分で構成されています。 それらにより、独立したマイクロサービスを同時に実行できます。 これらの最新のアプリケーションは、通常、コンテナー化されており、Docker Compose や Kubernetes といったコンテナー オーケストレーション ツールを必要とします。

次の例では、マルチコンテナー アプリケーションを作成します。 また、[Dapr サービス呼び出し](service-invocation.md)構成ブロックを使用して、サービス間の通信を行います。 このソリューションは、Web API から天気予報を取得する Web アプリケーションで構成されています。 それぞれが、Docker コンテナーで実行されます。 Docker Compose を使用して、コンテナーをローカル環境で実行し、デバッグ機能を有効にします。

ローカル環境を Dapr 用に構成したことと、[.NET Core 3.1 開発ツール](https://dotnet.microsoft.com/download/dotnet-core/3.1)をインストールしたことを確認します (手順については、この章の最初を参照してください)。

また、このサンプルを最後まで行うには、 **.NET Core クロスプラットフォームの開発** ワークロードがインストールされた [Visual Studio 2019](https://visualstudio.microsoft.com/downloads) を使用する必要があります。

### <a name="create-the-application"></a>アプリケーションを作成する

1. Visual Studio 2019 で、**ASP.NET Core Web アプリ** プロジェクトを作成します。

    :::image type="content" source="./media/getting-started/multicontainer-createproject.png" alt-text="新しいプロジェクトの作成のスクリーンショット":::

1. プロジェクトの名前を `DaprFrontEnd`、ソリューションの名前を `DaprMultiContainer` にします。

    :::image type="content" source="./media/getting-started/multicontainer-configureproject.png" alt-text="新しいプロジェクトの構成のスクリーンショット":::

1. **[Web アプリケーション]** を選択し、Razor Pages を使用して Web アプリケーションを作成します。 [Enable Docker Support]\(Docker サポートを有効にする\) は **選択しないで** ください。 Docker サポートは、後で追加します。

    :::image type="content" source="./media/getting-started/multicontainer-createwebapp.png" alt-text="新しい ASP.NET Core Web アプリケーションの作成のスクリーンショット":::

1. ASP.NET Core Web API プロジェクトを同じソリューションに追加し、_DaprBackEnd_ という名前にします。 プロジェクトの種類として **[API]** を選択します。 既定では、Dapr サイドカーでそのパブリック API へのアクセスを制限するには、ネットワーク境界が利用されます。 そのため、 **[HTTPS 用の構成]** チェック ボックスをオフにします。

    :::image type="content" source="./media/getting-started/multicontainer-createwebapi.png" alt-text="Web API の作成のスクリーンショット":::

### <a name="add-dapr-service-invocation"></a>Dapr のサービス呼び出しを追加する

ここでは、Dapr [サービス呼び出し構成ブロック](https://docs.dapr.io/developing-applications/building-blocks/service-invocation/service-invocation-overview/)を使用して、サービス間の通信を構成します。 Web アプリで Web API から天気予報を取得できるようにします。 サービス呼び出し構成ブロックには、多くの利点があります。 これには、サービスの検出、自動再試行、メッセージの暗号化 (mTLS を使用)、監視の向上などがあります。 Dapr サイドカーでサービス呼び出し API を呼び出すには、Dapr .NET SDK を使用します。

1. Visual Studio で、パッケージ マネージャー コンソールを開き ( **[ツール] > [NuGet パッケージ マネージャー] > [パッケージ マネージャー コンソール]** )、`DaprFrontEnd` が既定のプロジェクトであることを確認します。 コンソールで、`Dapr.AspNetCore` NuGet パッケージをプロジェクトに追加します。

    ```powershell
    Install-Package Dapr.AspNetCore
    ```

    > [!NOTE]
    > `Dapr.AspNetCore` のプレリリース バージョンを対象にする場合は、`-Prerelease` フラグを指定する必要があります。

1. `DaprFrontEnd` プロジェクトで *Startup.cs* ファイルを開き、`ConfigureServices` メソッドを次のコードに置き換えます。

    ```csharp
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers().AddDapr();
        services.AddRazorPages();
    }
    ```

    `AddDapr` を呼び出すと、ASP.NET Core の依存関係挿入システムに `DaprClient` クラスが登録されます。 クライアントが登録されたら、`DaprClient` のインスタンスをサービス コードに挿入して、Dapr サイドカー、構成ブロック、コンポーネントと通信できるようになります。

1. *WeatherForecast* という名前の新しい C# クラス ファイルを、`DaprFrontEnd` プロジェクトに追加します。

    ```csharp
    using System;

    namespace DaprFrontEnd
    {
        public class WeatherForecast
        {
            public DateTime Date { get; set; }

            public int TemperatureC { get; set; }

            public int TemperatureF { get; set; }

            public string Summary { get; set; }
        }
    }
    ```

1. *Pages* フォルダーにある *Index.cshtml.cs* を開いて、その内容を次のコードに置き換えます。

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Dapr.Client;
    using Microsoft.AspNetCore.Mvc.RazorPages;

    namespace DaprFrontEnd.Pages
    {
        public class IndexModel : PageModel
        {
            private readonly DaprClient _daprClient;

            public IndexModel(DaprClient daprClient)
            {
                _daprClient = daprClient ?? throw new ArgumentNullException(nameof(daprClient));
            }

            public async Task OnGet()
            {
                var forecasts = await _daprClient.InvokeMethodAsync<IEnumerable<WeatherForecast>>(
                    HttpMethod.Get,
                    "daprbackend",
                    "weatherforecast");

                ViewData["WeatherForecastData"] = forecasts;
            }
        }
    }
    ```

    `DaprClient` クラスを `IndexModel` コンストラクターに挿入することによって、Dapr の機能を Web アプリに追加します。 `OnGet` メソッドでは、Dapr のサービス呼び出し構成ブロックを使用して、API サービスを呼び出します。 `OnGet` メソッドは、ユーザーがホーム ページにアクセスするたびに呼び出されます。 `DaprClient.InvokeMethodAsync` メソッドを使用して、`daprbackend` サービスの `weatherforecast` メソッドを呼び出します。 後ほど、Dapr で実行するように Web API を構成するときに、アプリケーション ID として `daprbackend` を使用するように構成します。 最後に、サービスの応答がビュー データに保存されます。

1. *Pages* フォルダーの *Index.cshtml* ファイルの内容を、次のコードに置き換えます。 それによって、ビュー データに格納されている天気予報がユーザーに表示されます。

    ```razor
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }

    <div class="text-center">
        <h1 class="display-4">Welcome</h1>
        <p>Learn about <a href="https://docs.microsoft.com/aspnet/core">building Web apps with ASP.NET Core</a>.</p>
        @foreach (var forecast in (IEnumerable<WeatherForecast>)ViewData["WeatherForecastData"])
        {
            <p>The forecast for @forecast.Date is @forecast.Summary!</p>
        }
    </div>
    ```

### <a name="add-container-support"></a>コンテナー サポートを追加する

この例の最後の部分では、コンテナーのサポートを追加し、Docker Compose を使用してソリューションを実行します。

1. `DaprFrontEnd` プロジェクトを右クリックし、 **[追加]**  >  **[コンテナー オーケストレーター サポート]** を選択します。 **[コンテナー オーケストレーター サポートの追加]** ダイアログが表示されます。

    :::image type="content" source="./media/getting-started/multicontainer-addorchestrator.png" alt-text="コンテナー オーケストレーター サポートの追加のスクリーンショット":::

    **[Docker Compose]** を選択します。

1. 次のダイアログ ボックスで、ターゲット OS として **[Linux]** を選択します。

    :::image type="content" source="./media/getting-started/multicontainer-targetos.png" alt-text="Docker ターゲット OS の選択のスクリーンショット":::

    Visual Studio によって、ソリューションの **docker-compose** フォルダーに、*docker-compose.yml* ファイルと *.dockerignore* ファイルが作成されます。

    :::image type="content" source="./media/getting-started/multicontainer-dockersolution.png" alt-text="docker-compose プロジェクトのスクリーンショット":::

    *docker-compose.yml* ファイルは次のような内容です。

    ```yaml
    version: "3.4"

    services:
      daprfrontend:
        image: ${DOCKER_REGISTRY-}daprfrontend
        build:
          context: .
          dockerfile: DaprFrontEnd/Dockerfile
    ```

    *.dockerignore* ファイルには、Docker によってコンテナーに含める必要のないファイルの種類と拡張機能が含まれます。 これらのファイルは、開発中のアプリやサービスではなく、開発環境やソース管理に関連するものです。

    *DaprFrontEnd* プロジェクト ディレクトリのルートには、新しい *Dockerfile* が作成されています。 *Dockerfile* は、イメージをビルドするために使用される一連のコマンドです。 詳細については、[Dockerfile のリファレンス](https://docs.docker.com/engine/reference/builder)を参照してください。

    *Dockerfile* には、YAML が含まれています。

    ```yml
    FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
    WORKDIR /app
    EXPOSE 80
    EXPOSE 443

    FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
    WORKDIR /src
    COPY ["DaprFrontEnd/DaprFrontEnd.csproj", "DaprFrontEnd/"]
    RUN dotnet restore "DaprFrontEnd/DaprFrontEnd.csproj"
    COPY . .
    WORKDIR "/src/DaprFrontEnd"
    RUN dotnet build "DaprFrontEnd.csproj" -c Release -o /app/build

    FROM build AS publish
    RUN dotnet publish "DaprFrontEnd.csproj" -c Release -o /app/publish

    FROM base AS final
    WORKDIR /app
    COPY --from=publish /app/publish .
    ENTRYPOINT ["dotnet", "DaprFrontEnd.dll"]
    ```

    前の *Dockerfile* が呼び出されると、次の手順が順番に実行されます。

    1. `mcr.microsoft.com/dotnet/aspnet:3.1` イメージがプルされて、`base` という名前が設定されます。
    1. 作業ディレクトリが */app* に設定されます。
    1. ポート `80` と `443` が公開されます。
    1. `mcr.microsoft.com/dotnet/sdk:3.1` イメージがプルされて、`build` という名前が設定されます。
    1. 作業ディレクトリが */src* に設定されます。
    1. _DaprFrontEnd/DaprFrontEnd.csproj_ が *DaprFrontEnd/* という名前の新しいディレクトリにコピーされます。
    1. プロジェクトで [`dotnet restore`](../../core/tools/dotnet-restore.md) が呼び出されます。
    1. ルート ディレクトリからイメージのルートに、すべてのものがコピーされます。
    1. 作業ディレクトリが _/src/DaprFrontEnd_ に設定されます。
    1. プロジェクトで [`dotnet build`](../../core/tools/dotnet-build.md) が呼び出されます。
        - **Release** 構成がターゲットに設定され、出力が */app/build* に設定されます。
    1. 既存の `build` 基本イメージから新しいビルド ステージが初期化され、名前が `publish` に設定されます。
    1. プロジェクトで `dotnet publish` が呼び出されます。
        - **Release** 構成がターゲットに設定され、出力が */app/publish* に設定されます。
    1. 既存の `publish` 基本イメージから新しいビルド ステージが初期化され、名前が `final` に設定されます。
    1. 作業ディレクトリが */app* に設定されます。
    1. `publish` イメージから `final` イメージのルートに `/app/publish` ディレクトリがコピーされます。
    1. イメージとしてのエントリ ポイントが `dotnet` に設定され、引数として `DaprFrontEnd.dll` が渡されます。

1. `DaprBackEnd` Web API プロジェクトでプロジェクト ノードを右クリックして、 **[追加]**  >  **[コンテナー オーケストレーター サポート]** を選択します。 **[Docker Compose]** を選択し、ターゲット OS として **[Linux]** を再び選択します。

    _DaprBackEnd_ プロジェクト ディレクトリのルートには、新しい *Dockerfile* が作成されています。 *Dockerfile* には、次の YAML が含まれています。

    ```yml
    FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
    WORKDIR /app
    EXPOSE 80

    FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
    WORKDIR /src
    COPY ["DaprBackEnd/DaprBackEnd.csproj", "DaprBackEnd/"]
    RUN dotnet restore "DaprBackEnd/DaprBackEnd.csproj"
    COPY . .
    WORKDIR "/src/DaprBackEnd"
    RUN dotnet build "DaprBackEnd.csproj" -c Release -o /app/build

    FROM build AS publish
    RUN dotnet publish "DaprBackEnd.csproj" -c Release -o /app/publish

    FROM base AS final
    WORKDIR /app
    COPY --from=publish /app/publish .
    ENTRYPOINT ["dotnet", "DaprBackEnd.dll"]
    ```

    *docker-compose.yml* ファイルを再び開き、内容を確認します。 Visual Studio によって、**Docker Compose** ファイルが更新されています。 両方のサービスが含まれるようになっています。

    ```yaml
    version: '3.4'

    services:
      daprfrontend:
        image: ${DOCKER_REGISTRY-}daprfrontend
        build:
          context: .
          dockerfile: DaprFrontEnd/Dockerfile

      daprbackend:
        image: ${DOCKER_REGISTRY-}daprbackend
        build:
          context: .
          dockerfile: DaprBackEnd/Dockerfile
    ```

1. コンテナー化されたアプリケーション内から Dapr 構成ブロックを使用するには、Dapr サイドカー コンテナーを Compose ファイルに追加する必要があります。 次の例と一致するように、*docker-compose.yml* ファイルの内容を慎重に更新します。 書式設定とスペースに注意してください。タブは使用しないでください。

    ```yaml
    version: '3.4'

    services:
      daprfrontend:
        image: ${DOCKER_REGISTRY-}daprfrontend
        build:
          context: .
          dockerfile: DaprFrontEnd/Dockerfile
        ports:
          - "51000:50001"

      daprfrontend-dapr:
        image: "daprio/daprd:latest"
        command: [ "./daprd", "-app-id", "daprfrontend", "-app-port", "80" ]
        depends_on:
          - daprfrontend
        network_mode: "service:daprfrontend"

      daprbackend:
        image: ${DOCKER_REGISTRY-}daprbackend
        build:
          context: .
          dockerfile: DaprBackEnd/Dockerfile
        ports:
          - "52000:50001"

      daprbackend-dapr:
        image: "daprio/daprd:latest"
        command: [ "./daprd", "-app-id", "daprbackend", "-app-port", "80" ]
        depends_on:
          - daprfrontend
        network_mode: "service:daprbackend"
    ```

    更新されたファイルでは、`daprfrontend` および `daprbackend` サービス用に、それぞれ `daprfrontend-dapr` および `daprbackend-dapr` サイドカーが追加されています。 更新されたファイルでは、次の変更に注意してください。

    - サイドカーで、`daprio/daprd:latest` コンテナー イメージが使用されています。 `latest` タグの使用は、運用シナリオには推奨されません。 運用環境では、特定のバージョン番号を使用することをお勧めします。
    - Compose ファイルで定義されている各サービスには、ネットワークを分離するために独自のネットワーク名前空間があります。 サイドカーがアプリケーションと同じネットワーク名前空間で実行されるようにするため、`network_mode: "service:..."` が使用されています。 このようにすることで、サイドカーとアプリケーションは `localhost` を使用して通信できます。
    - Dapr サイドカーが相互に通信できるようにするには、サイドカーで gRPC 通信のリッスンに使用されるポート (既定では 50001) を、公開する必要があります。

1. ソリューションを実行して (<kbd>F5</kbd> キーまたは <kbd>Ctrl + F5</kbd> キー)、想定どおりに動作することを確認します。 すべてが正しく構成されている場合は、天気予報データが表示されます。

    :::image type="content" source="./media/getting-started/multicontainer-result.png" alt-text="天気予報データが示される最終的なソリューションのスクリーンショット":::

    Docker Compose と Visual Studio 2019 を使用してローカル環境で実行すると、ブレークポイントを設定してアプリケーションをデバッグできます。 運用シナリオの場合は、Kubernetes でアプリケーションをホストすることをお勧めします。 この記事には、Kubernetes にデプロイするためのスクリプトが収められている付属の参照アプリケーション [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr) が含まれています。

    このチュートリアルで使用されている Dapr のサービス呼び出し構成ブロックの詳細については、[第 6 章](service-invocation.md)を参照してください。

## <a name="summary"></a>まとめ

この章では、Dapr を "*試してみました*"。 Dapr .NET SDK を使用して、Dapr と .NET アプリケーション プラットフォームがどのように統合されるのかを見ました。

最初の例は、Dapr の状態管理構成ブロックを使用する、単純でステートフルな .NET コンソール アプリケーションでした。

2 番目の例には、Docker で実行されるマルチコンテナー アプリケーションが含まれました。 Visual Studio と Docker Compose を使用することにより、すべての .NET アプリで使い慣れた "*F5 デバッグ エクスペリエンス*" を利用できました。

また、Dapr のコンポーネント構成ファイルについても詳しく調べました。 これらにより、Dapr の構成ブロックによって使用される実際のインフラストラクチャ実装が構成されます。 名前空間とスコープを使用すると、コンポーネントへのアクセスを特定の環境やアプリケーションに制限できます。

この後の章では、Dapr によって提供される構成ブロックについて詳しく説明します。

### <a name="references"></a>リファレンス

- [Dapr ドキュメント - 概要](https://docs.dapr.io/getting-started)
- [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr)

> [!div class="step-by-step"]
> [前へ](dapr-at-20000-feet.md)
> [次へ](reference-application.md)
