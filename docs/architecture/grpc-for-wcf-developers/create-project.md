---
title: WCF 開発者向けの新しい ASP.NET Core gRPC プロジェクト-gRPC を作成する
description: Visual Studio またはコマンドラインを使用して gRPC プロジェクトを作成する方法について説明します。
ms.date: 09/02/2019
ms.openlocfilehash: fbcc598cf503a5baeca941803ff8fa0d5fc99671
ms.sourcegitcommit: cdf5084648bf5e77970cbfeaa23f1cab3e6e234e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76919409"
---
# <a name="create-a-new-aspnet-core-grpc-project"></a>新しい ASP.NET Core gRPC プロジェクトを作成する

.NET Core SDK には、`dotnet`の強力な CLI ツールが付属しています。このツールを使用すると、コマンドラインからプロジェクトとソリューションを作成および管理できます。 SDK は Visual Studio と密接に統合されているので、使い慣れたグラフィカルユーザーインターフェイスを使用してすべてを利用できます。 この章では、新しい ASP.NET Core gRPC プロジェクトを作成する両方の方法について説明します。

## <a name="create-the-project-by-using-visual-studio"></a>Visual Studio を使用してプロジェクトを作成する

> [!IMPORTANT]
> ASP.NET Core 3.0 アプリを開発するには、 **ASP.NET と web 開発**ワークロードがインストールされた Visual Studio 2019 バージョン16.3 以降が必要です。

空の*ソリューション*テンプレートから、 **TraderSys**という名前の空のソリューションを作成します。 `src`という名前のソリューションフォルダーを追加します。 次に、フォルダーを右クリックし、[**新しいプロジェクト**の**追加** > ] を選択します。 [テンプレート検索] ボックスに「`grpc`」と入力すると、`gRPC Service`という名前のプロジェクトテンプレートが表示されます。

![[新しいプロジェクトの追加] ダイアログボックスのスクリーンショット](media/create-project/new-grpc-project.png)

**[次]** へ を選択して、 **[新しいプロジェクトの構成]** ダイアログボックスに進みます。 プロジェクトに `TraderSys.Portfolios` という名前を指定し、その**場所**に `src` サブディレクトリを追加します。

![[新しいプロジェクトの構成] ダイアログボックスのスクリーンショット](media/create-project/configure-project.png)

**[次]** へ を選択して、 **[新しい Grpc サービスの作成]** ダイアログボックスに進みます。

![[新しい gRPC サービスの作成] ダイアログボックスのスクリーンショット](media/create-project/create-new-grpc-service.png)

現時点では、サービス作成のオプションは限られています。 Docker は後で導入されるため、このオプションをオフのままにしておきます。 [作成] を選択**する**だけです。 最初の ASP.NET Core 3.0 gRPC プロジェクトが生成され、ソリューションに追加されます。 `dotnet CLI`の操作について知りたくない場合は、「[コード例のクリーンアップ](#clean-up-the-example-code)」のセクションに進んでください。

## <a name="create-the-project-by-using-the-net-core-cli"></a>.NET Core CLI を使用してプロジェクトを作成します。

ここでは、コマンドラインからのソリューションとプロジェクトの作成について説明します。

次のコマンドに示すように、ソリューションを作成します。 `-o` (または `--output`) フラグは、現在のディレクトリに作成されている出力ディレクトリを指定します (まだ存在しない場合)。 このソリューションには、ディレクトリと同じ名前が付けられています: `TraderSys.sln`。 別の名前を指定するには、`-n` (または `--name`) フラグを使用します。

```dotnetcli
dotnet new sln -o TraderSys
cd TraderSys
```

ASP.NET Core 3.0 には、gRPC サービス用の CLI テンプレートが付属しています。 このテンプレートを使用して新しいプロジェクトを作成し、ASP.NET Core プロジェクトの従来のように `src` サブディレクトリに配置します。 `-n` フラグを使用して別の名前を指定しない限り、プロジェクトにはディレクトリ (`TraderSys.Portfolios.csproj`) の後に名前が付けられます。

```dotnetcli
dotnet new grpc -o src/TraderSys.Portfolios
```

最後に、`dotnet sln` コマンドを使用して、ソリューションにプロジェクトを追加します。

```dotnetcli
dotnet sln add src/TraderSys.Portfolios
```

> [!TIP]
> 特定のディレクトリに含まれるのは1つの `.csproj` ファイルのみであるため、ディレクトリだけを指定して、入力を保存することができます。

このソリューションは、Visual Studio 2019、Visual Studio Code、または任意のエディターで開くことができるようになりました。

## <a name="clean-up-the-example-code"></a>コード例をクリーンアップする

これで、このブックで既に確認されている gRPC テンプレートを使用して、サンプルサービスを作成できました。 これは、株価取引のコンテキストでは役に立たないので、最初のプロジェクトの内容を編集します。

### <a name="rename-and-edit-the-proto-file"></a>プロトコルファイルの名前を変更して編集する

`Protos/greet.proto` ファイルの名前を `Protos/portfolios.proto`に変更し、エディターで開きます。 `package` 行の後のすべてを削除します。 次に、`option csharp_namespace`、`package` と `service` の名前を変更し、既定の `SayHello` サービスを削除します。 コードは次のようになります。

```protobuf
syntax = "proto3";

option csharp_namespace = "TraderSys.Portfolios.Protos";

package PortfolioServer;

service Portfolios {
  // RPCs will go here
}
```

> [!TIP]
> テンプレートでは、既定では `Protos` 名前空間の部分は追加されませんが、これを追加すると、gRPC によって生成されたクラスと独自のクラスをコード内で明確に区別しやすくなります。

Visual Studio などの統合開発環境 (IDE) で `greet.proto` ファイルの名前を変更すると、このファイルへの参照が `.csproj` ファイルで自動的に更新されます。 ただし、Visual Studio Code などの他のエディターでは、この参照は自動的に更新されないため、プロジェクトファイルを手動で編集する必要があります。

GRPC ビルドターゲットには、`Protobuf` item 要素があります。この要素を使用して、コンパイルする `.proto` ファイルを指定できます。また、どの形式のコード生成が必要か (つまり、"Server" または "Client") があります。

```xml
<ItemGroup>
  <Protobuf Include="Protos\portfolios.proto" GrpcServices="Server" />
</ItemGroup>
```

### <a name="rename-the-greeterservice-class"></a>`GreeterService` クラスの名前を変更する

`GreeterService` クラスは `Services` フォルダーにあり、`Greeter.GreeterBase`から継承されます。 名前を `PortfolioService`に変更し、基本クラスを `Portfolios.PortfoliosBase`に変更します。 `override` メソッドを削除します。

```csharp
public class PortfolioService : Portfolios.PortfoliosBase
{
}
```

`Startup` クラスの `Configure` メソッドに `GreeterService` クラスへの参照がありました。 リファクタリングを使用してクラスの名前を変更した場合は、この参照が自動的に更新されている必要があります。 ただし、使用していない場合は、手動で編集する必要があります。

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapGrpcService<PortfolioService>();
    });
}
```

次のセクションでは、この新しいサービスに機能を追加します。

>[!div class="step-by-step"]
>[前へ](migrate-wcf-to-grpc.md)
>[次へ](migrate-request-reply.md)
