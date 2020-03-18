---
title: Blazor アプリのプロジェクト構造
description: Web フォームと Blazor プロジェクトのプロジェクト構造ASP.NET比較する方法について説明します。
author: danroth27
ms.author: daroth
ms.date: 09/11/2019
ms.openlocfilehash: 2c383e86ff22f5a3460476998992b66e9417cc11
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79401743"
---
# <a name="project-structure-for-blazor-apps"></a>Blazor アプリのプロジェクト構造

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

プロジェクト構造の違いは大きいにもかかわらず、ASP.NET Web フォームと Blazor は多くの類似概念を共有しています。 ここでは、Blazor プロジェクトの構造を見て、それを ASP.NET Web フォーム プロジェクトと比較します。

最初の Blazor アプリを作成するには[、Blazor の開始手順](/aspnet/core/blazor/get-started)の手順に従ってください。 指示に従って、Blazor Server アプリまたは ASP.NET Core でホストされている Blazor WebAssembly アプリを作成できます。 ホスト モデル固有のロジックを除き、両方のプロジェクトのコードの大部分は同じです。

## <a name="project-file"></a>プロジェクト ファイル

Blazor サーバー アプリは .NET Core プロジェクトです。 Blazor Server アプリのプロジェクト ファイルは、次のように簡単です。

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp3.0</TargetFramework>
  </PropertyGroup>

</Project>
```

Blazor WebAssembly アプリのプロジェクト ファイルは、少し複雑に見えます (正確なバージョン番号は異なる場合があります)。

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <RazorLangVersion>3.0</RazorLangVersion>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.Blazor" Version="3.1.0" />
    <PackageReference Include="Microsoft.AspNetCore.Blazor.Build" Version="3.1.0" PrivateAssets="all" />
    <PackageReference Include="Microsoft.AspNetCore.Blazor.HttpClient" Version="3.1.0" />
    <PackageReference Include="Microsoft.AspNetCore.Blazor.DevServer" Version="3.1.0" PrivateAssets="all" />
  </ItemGroup>

  <ItemGroup>
    <ProjectReference Include="..\Shared\BlazorWebAssemblyApp1.Shared.csproj" />
  </ItemGroup>

</Project>
```

Blazor WebAssembly プロジェクトは、WebAssembly ベースの .NET ランタイムでブラウザーで実行されるため、.NET Core ではなく .NET 標準を対象とします。 サーバーや開発者のコンピューターのように、.NET を Web ブラウザーにインストールすることはできません。 したがって、プロジェクトは個々のパッケージ参照を使用して Blazor フレームワークを参照します。

これに対し、Web フォーム プロジェクトASP.NET既定のファイルには *、.csproj*ファイルに約 300 行の XML が含まれており、そのほとんどはプロジェクトのさまざまなコードファイルとコンテンツ ファイルを明示的にリストしています。 NET Core ベースおよび .NET Standard ベースのプロジェクトの簡略化の多くは、SDK を参照してインポートされる既定の`Microsoft.NET.Sdk.Web`ターゲットとプロパティから取得されます。 Web SDK には、ワイルドカードなどの便利な機能が用意されており、プロジェクトにコードファイルやコンテンツファイルを簡単に含めることができ、その機能が簡単になります。 ファイルを明示的に一覧表示する必要はありません。 NET Core を対象とする場合、Web SDK は 、.NET Core と ASP.NET コアの両方の共有フレームワークにフレームワーク参照を追加します。 フレームワークは、[**ソリューション エクスプローラー** ] ウィンドウの **[依存関係** > **フレームワーク**] ノードから表示されます。 共有フレームワークは、.NET Core をインストールするときにコンピューターにインストールされたアセンブリのコレクションです。

これらのアセンブリはサポートされていますが、個々のアセンブリ参照は .NET Core プロジェクトではあまり一般的ではありません。 ほとんどのプロジェクトの依存関係は、NuGet パッケージ参照として処理されます。 最上位レベルのパッケージの依存関係を参照する必要があるのは、.NET Core プロジェクトだけです。 推移的な依存関係は自動的に含まれます。 パッケージを参照する web フォーム プロジェクトASP.NET一般的に見られる*packages.config*ファイルを使用する代わりに、パッケージ`<PackageReference>`参照は要素を使用してプロジェクト ファイルに追加されます。

```xml
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" Version="12.0.2" />
</ItemGroup>
```

## <a name="entry-point"></a>エントリ ポイント

Blazor Server アプリのエントリ ポイントは、コンソール アプリケーションで確認されるように *、Program.cs*ファイルで定義されます。 アプリが実行されると、Web アプリ固有の既定値を使用して Web ホスト インスタンスが作成され、実行されます。 Web ホストは Blazor Server アプリケーションのライフサイクルを管理し、ホストレベルのサービスを設定します。 このようなサービスの例としては、構成、ロギング、依存関係の注入、および HTTP サーバーがあります。 このコードは、ほとんどが定型であり、多くの場合、変更されません。

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

Blazor WebAssembly アプリでは *、Program.cs*のエントリ ポイントも定義します。 コードの外観が少し異なります。 コードは、アプリに同じホスト レベルのサービスを提供するようにアプリ ホストを設定するという点で似ています。 ただし、WebAssembly アプリ ホストはブラウザで直接実行されるため、HTTP サーバーをセットアップしません。

Blazor アプリには`Startup`*、Global.asax*ファイルではなく、アプリのスタートアップ ロジックを定義するクラスがあります。 この`Startup`クラスは、アプリとアプリ固有のサービスを構成するために使用されます。 Blazor Server アプリケーションでは、`Startup`このクラスを使用して、クライアントブラウザーとサーバー間で Blazor が使用するリアルタイム接続用のエンドポイントをセットアップします。 Blazor WebAssembly アプリでは、`Startup`クラスはアプリのルート コンポーネントと、レンダリングする場所を定義します。 `Startup` [「アプリのスタートアップ](./app-startup.md)」セクションのクラスを詳しく見ていきます。

## <a name="static-files"></a>静的ファイル

Web フォーム プロジェクトASP.NETとは異なり、Blazor プロジェクト内のすべてのファイルを静的ファイルとして要求できるわけではありません。 *wwwroot*フォルダ内のファイルのみが Web アドレス指定可能です。 このフォルダーは、アプリの 「web ルート」と呼ばれます。 アプリの Web*ルート以外*のものは、Web アドレス指定できません。 このセットアップでは、プロジェクト ファイルが Web 上で偶発的に公開されるのを防ぐセキュリティレベルが強化されます。

## <a name="configuration"></a>構成

通常、ASP.NET Web フォーム アプリの構成は、1 つ以上*の web.config*ファイルを使用して処理されます。 Blazor アプリには通常 *、web.config*ファイルはありません。 その場合、ファイルは IIS でホストされている場合にのみ IIS 固有の設定を構成するために使用されます。 代わりに、Blazor Server アプリはASP.NETコア構成の抽象化を使用します (Blazor WebAssembly アプリは現在、同じ構成抽象化をサポートしていませんが、これは将来追加される機能になる可能性があります)。 たとえば、デフォルトの Blazor サーバー アプリケーションは、いくつかの設定を*appsettings.json*に格納します。

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}
```

「構成」セクションの「ASP.NETコア プロジェクト」の構成について詳しく[説明](./config.md)します。

## <a name="razor-components"></a>Razor のコンポーネント

Blazor プロジェクトのほとんどのファイルは *.razor*ファイルです。 Razor は HTML と C# に基づくテンプレート言語で、Web UI を動的に生成するために使用されます。 *Razor*ファイルは、アプリの UI を構成するコンポーネントを定義します。 ほとんどの場合、コンポーネントは、Blazor サーバーと Blazor WebAssembly アプリケーションの両方で同じです。 Blazor のコンポーネントは、web フォームのユーザー コントロールASP.NET似ています。

各 Razor コンポーネント ファイルは、プロジェクトのビルド時に .NET クラスにコンパイルされます。 生成されたクラスは、コンポーネントの状態、レンダリング ロジック、ライフサイクル メソッド、イベント ハンドラー、およびその他のロジックをキャプチャします。 [「Blazor を使用して再利用可能な UI コンポーネントを構築](./components.md)する」セクションでコンポーネントの作成を見ていきます。

*_Imports.razor*ファイルは Razor コンポーネント ファイルではありません。 代わりに、同じフォルダーとそのサブフォルダー内の他の *.razor*ファイルにインポートする Razor ディレクティブのセットを定義します。 たとえば *、_Imports.razor*ファイルは、一般的に使用される`using`名前空間のステートメントを追加する従来の方法です。

```razor
@using System.Net.Http
@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.Forms
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.Web
@using Microsoft.JSInterop
@using BlazorApp1
@using BlazorApp1.Shared
```

## <a name="pages"></a>ページ

Blazor アプリのページはどこにありますか? Blazor では、アドレス指定可能なページ (ASP.NET Web フォーム アプリの *.aspx*ファイルなど) 用の別のファイル拡張子は定義されません。 代わりに、ページは、コンポーネントにルートを割り当てることによって定義されます。 ルートは通常、Razor ディレクティブ`@page`を使用して割り当てられます。 たとえば`Counter`*、Pages/Counter.razor*ファイルで作成されたコンポーネントは、次のルートを定義します。

```razor
@page "/counter"
```

Blazor でのルーティングは、サーバーではなくクライアント側で処理されます。 ユーザーがブラウザ内を移動すると、Blazor はナビゲーションをインターセプトし、一致するルートを持つコンポーネントをレンダリングします。

コンポーネント ルートは、*現在、.aspx*ページと同様に、コンポーネントのファイルの場所によって推測されていません。 この機能は、将来的に追加される可能性があります。 各ルートは、コンポーネントで明示的に指定する必要があります。 ルーティング可能なコンポーネントを*Pages*フォルダに格納することは、特別な意味を持たないため、純粋に規則です。

[「ページ、ルーティング、レイアウト](./pages-routing-layouts.md)」セクションの Blazor でのルーティングについて詳しく説明します。

## <a name="layout"></a>[レイアウト]

Web フォーム アプリASP.NETでは、共通のページ レイアウトはマスター ページ (*Site.Master*) を使用して処理されます。 Blazor アプリでは、ページ レイアウトはレイアウト コンポーネント (*共有/MainLayout.razor)* を使用して処理されます。 レイアウト コンポーネントについては、「[ページ、ルーティング、レイアウト](./pages-routing-layouts.md)」セクションで詳しく説明します。

## <a name="bootstrap-blazor"></a>ブートストラップブラゾール

Blazor をブートストラップするには、アプリが必要です。

- ページ上でルート コンポーネント (*App.Razor*) を表示する場所を指定します。
- 対応する Blazor フレームワークスクリプトを追加します。

Blazor Server アプリケーションでは、ルート コンポーネントのホスト ページは *_Host.cshtml*ファイルで定義されます。 このファイルは、コンポーネントではなく、Razor ページを定義します。 Razor ページは *、.aspx*ページと非常に似た、サーバーアドレス指定可能なページを定義するのに Razor の構文を使用します。 この`Html.RenderComponentAsync<TComponent>(RenderMode)`メソッドは、ルートレベルのコンポーネントをレンダリングする場所を定義するために使用されます。 この`RenderMode`オプションは、コンポーネントのレンダリング方法を示します。 次の表は、サポートされる`RenderMode`オプションの概要を示しています。

|オプション                        |説明       |
|------------------------------|------------------|
|`RenderMode.Server`           |ブラウザとの接続が確立されるとインタラクティブにレンダリング|
|`RenderMode.ServerPrerendered`|最初に事前レンダリングしてからインタラクティブにレンダリング|
|`RenderMode.Static`           |静的コンテンツとしてレンダリング|

*_framework/blazor.server.js*へのスクリプト参照は、サーバーとのリアルタイム接続を確立し、すべてのユーザー操作と UI の更新を処理します。

```razor
@page "/"
@namespace BlazorApp1.Pages
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>BlazorApp1</title>
    <base href="~/" />
    <link rel="stylesheet" href="css/bootstrap/bootstrap.min.css" />
    <link href="css/site.css" rel="stylesheet" />
</head>
<body>
    <app>
        @(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
</html>
```

Blazor WebAssembly アプリでは、ホスト ページは*wwwroot/index.html*下の単純な静的 HTML ファイルです。 要素`<app>`は、ルート コンポーネントをレンダリングする場所を示すために使用されます。

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>BlazorApp2</title>
    <base href="/" />
    <link href="css/bootstrap/bootstrap.min.css" rel="stylesheet" />
    <link href="css/site.css" rel="stylesheet" />
</head>
<body>
    <app>Loading...</app>

    <script src="_framework/blazor.webassembly.js"></script>
</body>
</html>
```

レンダリングする特定のコンポーネントは、アプリケーションの`Startup.Configure`メソッドで、コンポーネントのレンダリング先を示す対応する CSS セレクターを使用して構成されます。

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IComponentsApplicationBuilder app)
    {
        app.AddComponent<App>("app");
    }
}
```

## <a name="build-output"></a>ビルド出力

Blazor プロジェクトがビルドされると、すべての Razor コンポーネントとコード ファイルが 1 つのアセンブリにコンパイルされます。 Web フォーム プロジェクトASP.NETとは異なり、Blazor は UI ロジックの実行時コンパイルをサポートしていません。

## <a name="run-the-app"></a>アプリを実行する

Blazor サーバー アプリを実行するには`F5`、Visual Studio を押します。 Blazor アプリは、ランタイム コンパイルをサポートしていません。 コードとコンポーネントのマークアップの変更の結果を表示するには、デバッガーをアタッチしてアプリを再構築し、再起動します。 デバッガーをアタッチせずに実行する場合`Ctrl+F5`( )、Visual Studio はファイルの変更を監視し、変更が加えられた時点でアプリを再起動します。 変更が加えられた場合は、ブラウザを手動で更新します。

Blazor WebAssembly アプリを実行するには、次のいずれかの方法を選択します。

- 開発サーバーを使用してクライアント プロジェクトを直接実行します。
- ASP.NET Core でアプリをホストする場合は、サーバー プロジェクトを実行します。

Blazor WebAssembly アプリは、Visual Studio を使用したデバッグをサポートしていません。 アプリを実行するには、`Ctrl+F5`の`F5`代わりに を使用します。 代わりに、ブラウザで直接 Blazor WebAssembly アプリをデバッグできます。 詳細については[、「コア ブレイザASP.NETデバッグ](/aspnet/core/blazor/debug)」を参照してください。

>[!div class="step-by-step"]
>[前次](hosting-models.md)
>[Next](app-startup.md)
