---
title: WCF svcutil ツールの概要
description: .NET Framework プロジェクトの WCF svcutil ツールと同様に、.NET Core プロジェクトと ASP.NET Core プロジェクトの機能を追加する Microsoft WCF dotnet-svcutil ツールの概要。
author: mlacouture
ms.date: 02/22/2019
ms.openlocfilehash: 0607c73935f319f2cc0d8d9f92d96a4c71c54edf
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76920950"
---
# <a name="wcf-dotnet-svcutil-tool-for-net-core"></a>.NET Core 用 WCF dotnet-svcutil ツール

Windows Communication Foundation (WCF) **dotnet-svcutil** ツールは、ネットワークの場所にある Web サービスから、あるいは WSDL ファイルからメタデータを取得し、Web サービス操作にアクセスするクライアント プロキシ メソッドを含んだ WCF クラスを生成する .NET ツールです。

.NET Framework プロジェクトの [**ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)** ](../../framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) と同様に、**dotnet-svcutil** は、.NET Core プロジェクトおよび .NET Standard プロジェクトと互換性のある Web サービス参照を生成するためのコマンドライン ツールです。

**dotnet-svcutil** ツールは、Visual Studio 2017 バージョン 15.5 で最初に用意された Visual Studio 接続済みサービス プロバイダーである、[**WCF Web Service Reference**](wcf-web-service-reference-guide.md) に対する代わりのオプションです。 **dotnet-svcutil** ツールは、.NET ツールとして、Linux、macOS、Windows 上でクロスプラットフォームで利用できます。

> [!IMPORTANT]
> 信頼できるソースのサービスのみを参照してください。 信頼できないソースの参照を追加すると、セキュリティが損なわれる可能性があります。

## <a name="prerequisites"></a>必須コンポーネント

<!-- markdownlint-disable MD025 -->

# <a name="dotnet-svcutil-2x"></a>[dotnet-svcutil 2.x](#tab/dotnetsvcutil2x)

- [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 以降のバージョン
- 任意のコード エディター

# <a name="dotnet-svcutil-1x"></a>[dotnet-svcutil 1.x](#tab/dotnetsvcutil1x)

- [.NET Core 1.0.4 SDK](https://dotnet.microsoft.com/download) 以降のバージョン
- 任意のコード エディター

---

## <a name="getting-started"></a>作業の開始

次の例では、Web サービス参照を .NET Core Web プロジェクトに追加してサービスを呼び出すために必要な手順について説明します。 *HelloSvcutil* という名前の .NET Core Web アプリケーションを作成し、次のコントラクトを実装する Web サービスへの参照を追加します。

```csharp
[ServiceContract]
public interface ISayHello
{
    [OperationContract]
    string Hello(string name);
}
```

この例では、Web サービスが次のアドレスでホストされると仮定します: `http://contoso.com/SayHello.svc`

Windows、macOS、または Linux のコマンド ウィンドウから次の手順を実行します。

1. 次の例に示すように、_HelloSvcutil_ という名前のディレクトリをプロジェクト用に作成し、現在のディレクトリに指定します。

    ```console
    mkdir HelloSvcutil
    cd HelloSvcutil
    ```

2. 次に示すように、[`dotnet new`](../tools/dotnet-new.md) コマンドを使用して、このディレクトリに新しい C# Web プロジェクトを作成します。

    ```dotnetcli
    dotnet new web
    ```

3. CLI ツールとして [`dotnet-svcutil`NuGet パッケージ](https://nuget.org/packages/dotnet-svcutil)をインストールします。 <!-- markdownlint-disable MD023 -->
    # <a name="dotnet-svcutil-2x"></a>[dotnet-svcutil 2.x](#tab/dotnetsvcutil2x)

    ```dotnetcli
    dotnet tool install --global dotnet-svcutil
    ```

    # <a name="dotnet-svcutil-1x"></a>[dotnet-svcutil 1.x](#tab/dotnetsvcutil1x)
    `HelloSvcutil.csproj` プロジェクト ファイルをエディターで開いて `Project` 要素を編集し、次のコードを使用して [`dotnet-svcutil` NuGet パッケージ](https://nuget.org/packages/dotnet-svcutil)を CLI ツールの参照として追加します。

    ```xml
    <ItemGroup>
      <DotNetCliToolReference Include="dotnet-svcutil" Version="1.0.*" />
    </ItemGroup>
    ```

    その後、次に示すように、[`dotnet restore`](../tools/dotnet-restore.md) コマンドを使用して _dotnet-svcutil_ パッケージを復元します。

    ```dotnetcli
    dotnet restore
    ```

    ---

4. 次に示すように、_dotnet-svcutil_ コマンドを実行して、Web サービス参照ファイルを生成します。

    # <a name="dotnet-svcutil-2x"></a>[dotnet-svcutil 2.x](#tab/dotnetsvcutil2x)

    ```dotnetcli
    dotnet-svcutil http://contoso.com/SayHello.svc
    ```

    # <a name="dotnet-svcutil-1x"></a>[dotnet-svcutil 1.x](#tab/dotnetsvcutil1x)

    ```dotnetcli
    dotnet svcutil http://contoso.com/SayHello.svc
    ```

    ---

生成されたファイルは、_HelloSvcutil/ServiceReference/Reference.cs_ として保存されます。 また、_dotnet-svcutil_ ツールでは、プロキシ コードで必要な適切な WCF パッケージが、パッケージ参照としてプロジェクトに追加されます。

## <a name="using-the-service-reference"></a>サービス参照の使用

1. 次に示すように、[`dotnet restore`](../tools/dotnet-restore.md) コマンドを使用して WCF パッケージを復元します。

    ```dotnetcli
    dotnet restore
    ```

2. 使用するクライアント クラスと操作の名前を検索します。 `Reference.cs` には `System.ServiceModel.ClientBase` を継承するクラスが含まれており、そのメソッドを使用してサービスで操作を呼び出すことができます。 この例では、_SayHello_ サービスの _Hello_ 操作を呼び出します。 `ServiceReference.SayHelloClient` はクライアント クラスの名前であり、操作の呼び出しに使用できる `HelloAsync` という名前のメソッドが含まれます。

3. エディターで `Startup.cs` ファイルを開き、先頭にサービス参照名前空間に対する using ステートメントを追加します。

    ```csharp
    using ServiceReference;
    ```

4. Web サービスを呼び出すように、`Configure` メソッドを編集します。 これを行うには、`ClientBase` を継承するクラスのインスタンスを作成し、クライアント オブジェクトでメソッドを呼び出します。

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.Run(async (context) =>
        {
            var client = new SayHelloClient();
            var response = await client.HelloAsync();
            await context.Response.WriteAsync(response);
        });
    }

    ```

5. 次に示すように、[`dotnet run`](../tools/dotnet-run.md) コマンドを使用してアプリケーションを実行します。

    ```dotnetcli
    dotnet run
    ```

6. Web ブラウザーでコンソールに表示されている URL に移動します (たとえば、`http://localhost:5000`)。

次の出力が表示されます。"Hello dotnet-svcutil!"

`dotnet-svcutil` ツールのパラメーターの詳細な説明については、次に示すように、help パラメーターを渡してツールを呼び出してください。
# <a name="dotnet-svcutil-2x"></a>[dotnet-svcutil 2.x](#tab/dotnetsvcutil2x)

```dotnetcli
dotnet-svcutil --help
```

# <a name="dotnet-svcutil-1x"></a>[dotnet-svcutil 1.x](#tab/dotnetsvcutil1x)

```dotnetcli
dotnet svcutil --help
```

---

## <a name="feedback--questions"></a>フィードバックと質問

質問やフィードバックがありましたら、[GitHub で問題を提起してください](https://github.com/dotnet/wcf/issues/new)。 [GitHub の WCF リポジトリ](https://github.com/dotnet/wcf/issues?utf8=%E2%9C%93&q=is:issue%20label:tooling)で既存の質問や問題を確認することもできます。

## <a name="release-notes"></a>リリース ノート

- 既知の問題を含む最新のリリース情報については、[リリース ノート](https://github.com/dotnet/wcf/blob/master/release-notes/dotnet-svcutil-notes.md)のページを参照してください。

## <a name="information"></a>情報

- [dotnet-svcutil NuGet パッケージ](https://nuget.org/packages/dotnet-svcutil)
