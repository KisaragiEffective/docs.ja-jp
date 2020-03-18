---
title: Docker を使用してアプリをコンテナー化するチュートリアル
description: このチュートリアルでは、Docker を使って .NET Core アプリケーションをコンテナー化する方法を学習します。
ms.date: 01/09/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: e1904430a591b0e74a69d50a53869a130fc0a248
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78157831"
---
# <a name="tutorial-containerize-a-net-core-app"></a>チュートリアル: NET Core アプリのコンテナー化

このチュートリアルでは、.NET Core アプリケーションを含む Docker イメージをビルドする方法について説明します。 そのイメージを使って、ローカル開発環境、プライベート クラウド、またはパブリック クラウド用にコンテナーを作成することができます。

以下のことを学習します。

> [!div class="checklist"]
>
> - 簡単な .NET Core アプリを作成して発行する
> - .NET Core 用の Dockerfile を作成して構成する
> - Docker イメージの構築
> - Docker コンテナーを作成して実行する

.NET Core アプリケーション用に Docker コンテナーを構築してデプロイするタスクを理解できます。 "*Docker プラットフォーム*" では、"*Docker エンジン*" を使用して、すばやくアプリがビルドされ、"*Docker イメージ*" としてパッケージ化されます。 これらのイメージは、階層型コンテナーに展開されて実行される *Dockerfile* 形式で記述されています。

> [!TIP]
> 既存の ASP.NET Core アプリケーションを使用している場合は、[ASP.NET Core アプリケーションをコンテナー化する方法](/aspnet/core/host-and-deploy/docker/building-net-docker-images)に関するチュートリアルを参照してください。

## <a name="prerequisites"></a>必須コンポーネント

次の前提条件をインストールします。

- [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download)\
.NET Core をインストールしてある場合、どの SDK を使っているか確認するには、`dotnet --info` コマンドを使います。

- [Docker Community Edition](https://www.docker.com/products/docker-desktop)

- *Dockerfile* と .NET Core サンプル アプリ用の一時作業フォルダー。 このチュートリアルでは、作業フォルダーに *docker-working* の名前が使用されます。

## <a name="create-net-core-app"></a>.NET Core アプリを作成する

Docker コンテナーで実行される .NET Core アプリが必要です。 ターミナルを開き、作業フォルダーがまだない場合は作成して、移動します。 作業フォルダーで次のコマンドを実行し、*app* という名前のサブディレクトリに新しいプロジェクトを作成します。

```dotnetcli
dotnet new console -o app -n myapp
```

フォルダー ツリーは次のように表示されます。

```
docker-working
│
└───app
    │   myapp.csproj
    │   Program.cs
    │
    └───obj
            myapp.csproj.nuget.cache
            myapp.csproj.nuget.dgspec.json
            myapp.csproj.nuget.g.props
            myapp.csproj.nuget.g.targets
            project.assets.json
```

`dotnet new` コマンドでは、*app* という名前の新しいフォルダーが作成され、"Hello World" アプリが生成されます。 *app* フォルダーに移動し、コマンド `dotnet run` を実行します。 次のような出力が表示されます。

```console
> dotnet run
Hello World!
```

既定のテンプレートでは、端末に出力して終了するアプリが作成されます。 このチュートリアルでは、無限にループするアプリを使います。 テキスト エディターで *Program.cs* ファイルを開きます。 現在は次のようなコードになっているはずです。

```csharp
using System;

namespace myapp
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

1 秒ごとに数をカウントする次のコードにファイルを置き換えます。

```csharp
using System;

namespace myapp
{
    class Program
    {
        static void Main(string[] args)
        {
            var counter = 0;
            var max = args.Length != 0 ? Convert.ToInt32(args[0]) : -1;
            while (max == -1 || counter < max)
            {
                counter++;
                Console.WriteLine($"Counter: {counter}");
                System.Threading.Tasks.Task.Delay(1000).Wait();
            }
        }
    }
}
```

ファイルを保存し、`dotnet run` で再びプログラムをテストします。 このアプリは無限に実行されることに注意してください。 停止するにはキャンセル コマンド <kbd>Ctrl</kbd>+<kbd>C</kbd> を使用します。 次のような出力が表示されます。

```console
> dotnet run
Counter: 1
Counter: 2
Counter: 3
Counter: 4
^C
```

コマンド ラインでアプリに数値を渡すと、アプリはその値までカウント アップしただけで終了します。 `dotnet run -- 5` で 5 までカウントしてみてください。

> [!NOTE]
> `--` より後のパラメーターは `dotnet run` コマンドに渡されず、代わりにアプリケーションに渡されます。

## <a name="publish-net-core-app"></a>.NET Core アプリを発行する

.NET Core アプリを Docker イメージに追加する前に、アプリを発行します。 コンテナーの開始時に、発行されたバージョンのアプリが実行されることを確認する必要があります。

作業フォルダーから、サンプルのソース コードがある *app* フォルダーに移動し、次のコマンドを実行します。

```dotnetcli
dotnet publish -c Release
```

このコマンドでは、*publish* フォルダーにアプリがコンパイルされます。 作業フォルダーから *publish* フォルダーへのパスは `.\app\bin\Release\netcoreapp3.1\publish\` です

*app* フォルダーから、publish フォルダーのディレクトリ一覧を表示し、*myapp.dll* ファイルが作成されたことを確認します。

```console
> dir bin\Release\netcoreapp3.1\publish

    Directory:  C:\docker-working\app\bin\Release\netcoreapp3.1\publish

01/09/2020  11:41 AM    <DIR>          .
01/09/2020  11:41 AM    <DIR>          ..
01/09/2020  11:41 AM               407 myapp.deps.json
01/09/2020  12:15 PM             4,608 myapp.dll
01/09/2020  12:15 PM           169,984 myapp.exe
01/09/2020  12:15 PM               736 myapp.pdb
01/09/2020  11:41 AM               154 myapp.runtimeconfig.json
```

Linux または macOS を使用している場合は、`ls` コマンドを使用してディレクトリ一覧を表示し、*myapp.dll* ファイルが作成されたことを確認します。

```bash
me@DESKTOP:/docker-working/app$ ls bin/Release/netcoreapp3.1/publish
myapp.deps.json  myapp.dll  myapp.pdb  myapp.runtimeconfig.json
```

## <a name="create-the-dockerfile"></a>Dockerfile を作成する

*Dockerfile* ファイルは、コンテナー イメージを作成するために `docker build` コマンドによって使用されます。 このファイルは *Dockerfile* という名前のテキスト ファイルで、拡張子はありません。

ターミナルで、開始時に作成した作業フォルダーまでディレクトリを移動します。 *Dockerfile* という名前のファイルを作業フォルダーに作成し、テキスト エディターで開きます。 コンテナー化するアプリケーションの種類に応じて、ASP.NET Core ランタイムまたは .NET Core ランタイムのいずれかを選択します。 不明な場合は、ASP.NET Core ランタイム (.NET Core ランタイムが含まれています) を選択します。 このチュートリアルでは ASP.NET Core ランタイム イメージを使用しますが、前のセクションで作成したアプリケーションは .NET Core アプリケーションです。

- ASP.NET Core ランタイム

  ```dockerfile
  FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
  ```

- .NET Core ランタイム

  ```dockerfile
  FROM mcr.microsoft.com/dotnet/core/runtime:3.1
  ```

`FROM` コマンドは、指定したリポジトリから **3.1** というタグの付いたイメージをプルするよう Docker に指示します。 必ず、SDK のターゲットのランタイムと一致するランタイム バージョンをプルしてください。 たとえば、前のセクションで作成したアプリは .NET Core 3.1 SDK を使用しているため、*Dockerfile* で参照される基本イメージは **3.1** でタグ付けされます。

*Dockerfile* ファイルを保存します。 作業フォルダーのディレクトリ構造は次のようになります。 一部のより深いレベルのファイルとフォルダーは、スペース削減のためこの記事では省略されています。

```
docker-working
│   Dockerfile
│
└───app
    │   myapp.csproj
    │   Program.cs
    │
    ├───bin
    │   └───Release
    │       └───netcoreapp3.1
    │           └───publish
    │                   myapp.deps.json
    │                   myapp.exe
    │                   myapp.dll
    │                   myapp.pdb
    │                   myapp.runtimeconfig.json
    │
    └───obj
```

ターミナルから、次のコマンドを実行します。

```console
docker build -t myimage -f Dockerfile .
```

Docker により *Dockerfile* 内の各行が処理されます。 `docker build` コマンドの `.` は、現在のフォルダーで *Dockerfile* を検索するよう Docker に指示します。 このコマンドでは、イメージがビルドされて、そのイメージを指す **myimage** という名前のローカル リポジトリが作成されます。 このコマンドが終了したら、`docker images` を実行し、インストールされているイメージの一覧を表示します。

```console
> docker images
REPOSITORY                              TAG                 IMAGE ID            CREATED             SIZE
myimage                                 latest              38db0eb8f648        4 weeks ago         346MB
mcr.microsoft.com/dotnet/core/aspnet    3.1                 38db0eb8f648        4 weeks ago         346MB
```

2 つのイメージで同じ **IMAGE ID** 値が共有されていることに注意してください。 *Dockerfile* での唯一のコマンドが既存のイメージを新しいイメージの基にするものであったため、両方のイメージで値が同じになっています。 2 つのコマンドを *Dockerfile* に追加してみましょう。 各コマンドでは、**myimage** リポジトリ エントリが指し示すイメージを表す最後のコマンドで新しいイメージ レイヤーが作成されます。

```dockerfile
COPY app/bin/Release/netcoreapp3.1/publish/ app/

ENTRYPOINT ["dotnet", "app/myapp.dll"]
```

`COPY` コマンドは、自分のコンピューターの指定したフォルダーをコンテナー内のフォルダーにコピーするよう Docker に指示します。 この例では、*publish* フォルダーが、コンテナーの *app* という名前のフォルダーにコピーされます。

次のコマンド `ENTRYPOINT` は、実行可能ファイルとして実行するためにコンテナーを構成するよう Docker に指示します。 コンテナーの起動時に、`ENTRYPOINT` コマンドが実行されます。 このコマンドが終了すると、コンテナーは自動的に停止します。

端末から `docker build -t myimage -f Dockerfile .` を実行し、そのコマンドが終了したら `docker images` を実行します。

```console
> docker build -t myimage -f Dockerfile .
Sending build context to Docker daemon  1.624MB
Step 1/3 : FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
 ---> 38db0eb8f648
Step 2/3 : COPY app/bin/Release/netcoreapp3.1/publish/ app/
 ---> 37873673e468
Step 3/3 : ENTRYPOINT ["dotnet", "app/myapp.dll"]
 ---> Running in d8deb7b3aa9e
Removing intermediate container d8deb7b3aa9e
 ---> 0d602ca35c1d
Successfully built 0d602ca35c1d
Successfully tagged myimage:latest

> docker images
REPOSITORY                              TAG                 IMAGE ID            CREATED             SIZE
myimage                                 latest              0d602ca35c1d        4 seconds ago       346MB
mcr.microsoft.com/dotnet/core/aspnet    3.1                 38db0eb8f648        4 weeks ago         346MB
```

*Dockerfile* 内の各コマンドによってレイヤーが生成され、**IMAGE ID** が作成されます。 最後の **IMAGE ID** は **ddcc6646461b** であり (個々に異なります)、次にこのイメージを基にしてコンテナーを作成します。

## <a name="create-a-container"></a>コンテナーの作成

アプリを含むイメージができたので、コンテナーを作成することができます。 2 つの方法でコンテナーを作成することができます。 最初に、停止している新しいコンテナーを作成します。

```console
> docker create myimage
ceda87b219a4e55e9ad5d833ee1a7ea4da21b5ea7ce5a7d08f3051152e784944
```

上の `docker create` コマンドでは、**myimage** イメージに基づくコンテナーが作成されます。 そのコマンドの出力では、作成されたコンテナーの **CONTAINER ID** (個々に異なります) が示されます。 "*すべて*" のコンテナーの一覧を表示するには、`docker ps -a` コマンドを使います。

```console
> docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS        PORTS   NAMES
ceda87b219a4        myimage             "dotnet app/myapp.dll"   4 seconds ago       Created               gallant_lehmann
```

### <a name="manage-the-container"></a>コンテナーを管理する

各コンテナーにはランダムな名前が割り当てられており、それを使ってそのコンテナー インスタンスを参照できます。 たとえば、自動的に作成されたコンテナーでは **gallant_lehmann** という名前が選択されており (個々に異なります)、その名前を使ってコンテナーを起動できます。 `docker create --name` パラメーターを使って、自動的な名前を特定の名前でオーバーライドできます。

次の例では、`docker start` コマンドを使ってコンテナーを起動した後、`docker ps` コマンドを使って、実行されているコンテナーのみを表示します。

```console
> docker start gallant_lehmann
gallant_lehmann

> docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS         PORTS   NAMES
ceda87b219a4        myimage             "dotnet app/myapp.dll"   7 minutes ago       Up 8 seconds           gallant_lehmann
```

同様に、`docker stop` コマンドではコンテナーが停止されます。 次の例では、`docker stop` コマンドを使ってコンテナーを停止した後、`docker ps` コマンドを使って、実行されているコンテナーがないことを示します。

```console
> docker stop gallant_lehmann
gallant_lehmann

> docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS     PORTS   NAMES
```

### <a name="connect-to-a-container"></a>コンテナーへの接続

コンテナーが実行状態になった後、それに接続して出力を確認できます。 `docker start` コマンドを使ってコンテナーを起動し、`docker attach` コマンドを使って出力ストリームを表示します。 この例では、<kbd>Ctrl + C</kbd> キー入力を使用して、実行中のコンテナーからデタッチします。 このキー入力によりコンテナー内のプロセスが実際に終了する場合があり、コンテナーが停止します。 `--sig-proxy=false` パラメーターを指定すると、<kbd>Ctrl + C</kbd> キーを押してもコンテナー内のプロセスが停止しないことが保証されます。

コンテナーからデタッチした後、再アタッチして、まだ実行されていてカウントが行われていることを確認します。

```console
> docker start gallant_lehmann
gallant_lehmann

> docker attach --sig-proxy=false gallant_lehmann
Counter: 7
Counter: 8
Counter: 9
^C

> docker attach --sig-proxy=false gallant_lehmann
Counter: 17
Counter: 18
Counter: 19
^C
```

### <a name="delete-a-container"></a>コンテナーを削除する

この記事の目的では、何も行っていないコンテナーをそのままにするのは望ましくありません。 前に作成したコンテナーを削除します。 コンテナーが実行されている場合は、それを停止します。

```console
> docker stop gallant_lehmann
```

次の例では、すべてのコンテナーを一覧表示します。 次に、`docker rm` コマンドを使ってコンテナーを削除した後、もう一度実行中のコンテナーを確認します。

```console
> docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS     PORTS   NAMES
ceda87b219a4        myimage             "dotnet app/myapp.dll"   19 minutes ago      Exited             gallant_lehmann

> docker rm gallant_lehmann
gallant_lehmann

> docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS     PORTS    NAMES
```

### <a name="single-run"></a>単一実行

Docker では、1 つのコマンドとしてコンテナーを作成して実行するための `docker run` コマンドが提供されています。 このコマンドでは、`docker create` を実行してから `docker start` を実行する必要がありません。 コンテナーが停止したら自動的にコンテナーを削除するように、このコマンドを設定することもできます。 たとえば、`docker run -it --rm` を使うと 2 つのことが行われます。つまり、最初に現在の端末を使ってコンテナーに自動的に接続し、次にコンテナーが終了したらそれを削除します。

```console
> docker run -it --rm myimage
Counter: 1
Counter: 2
Counter: 3
Counter: 4
Counter: 5
^C
```

`docker run -it` では、<kbd>Ctrl + C</kbd> キーを押すと、コンテナーで実行されているプロセスが停止し、さらにコンテナーが停止されます。 `--rm` パラメーターを指定したので、プロセスが停止するとコンテナーは自動的に削除されます。 それが存在しないことを確認します。

```console
> docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS    PORTS   NAMES
```

### <a name="change-the-entrypoint"></a>ENTRYPOINT を変更する

`docker run` コマンドでは、*Dockerfile* から `ENTRYPOINT` コマンドを変更し、そのコンテナーに対してのみ何か他のことを実行することもできます。 たとえば、次のコマンドを使うと `bash` または `cmd.exe` を実行できます。 必要に応じて、コマンドを編集します。

#### <a name="windows"></a>Windows

この例では、`ENTRYPOINT` が `cmd.exe` に変更されています。 プロセスを終了してコンテナーを停止するには、<kbd>Ctrl</kbd>+<kbd>C</kbd> を押します。

```console
> docker run -it --rm --entrypoint "cmd.exe" myimage

Microsoft Windows [Version 10.0.17763.379]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\>dir
 Volume in drive C has no label.
 Volume Serial Number is 3005-1E84

 Directory of C:\

04/09/2019  08:46 AM    <DIR>          app
03/07/2019  10:25 AM             5,510 License.txt
04/02/2019  01:35 PM    <DIR>          Program Files
04/09/2019  01:06 PM    <DIR>          Users
04/02/2019  01:35 PM    <DIR>          Windows
               1 File(s)          5,510 bytes
               4 Dir(s)  21,246,517,248 bytes free

C:\>^C
```

#### <a name="linux"></a>Linux

この例では、`ENTRYPOINT` が `bash` に変更されています。 `quit` コマンドが実行されると、プロセスが終了してコンテナーが停止されます。

```bash
root@user:~# docker run -it --rm --entrypoint "bash" myimage
root@8515e897c893:/# ls app
myapp.deps.json  myapp.dll  myapp.pdb  myapp.runtimeconfig.json
root@8515e897c893:/# exit
exit
```

## <a name="essential-commands"></a>重要なコマンド

Docker には多種多様なコマンドがあり、コンテナーとイメージに対する操作がカバーされています。 コンテナーの管理に不可欠な Docker コマンドは次のとおりです。

- [docker build](https://docs.docker.com/engine/reference/commandline/build/)
- [docker run](https://docs.docker.com/engine/reference/commandline/run/)
- [docker ps](https://docs.docker.com/engine/reference/commandline/ps/)
- [docker stop](https://docs.docker.com/engine/reference/commandline/stop/)
- [docker rm](https://docs.docker.com/engine/reference/commandline/rm/)
- [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/)
- [docker image](https://docs.docker.com/engine/reference/commandline/image/)

## <a name="clean-up-resources"></a>リソースをクリーンアップする

このチュートリアルでは、コンテナーとイメージを作成しました。 必要な場合は、これらのリソースを削除します。 次のコマンドを使います

01. すべてのコンテナーを一覧表示します

    ```console
    > docker ps -a
    ```

02. 実行しているコンテナーを停止します。 `CONTAINER_NAME` は、コンテナーに自動的に割り当てられた名前を表します。

    ```console
    > docker stop CONTAINER_NAME
    ```

03. コンテナーを削除します

    ```console
    > docker rm CONTAINER_NAME
    ```

次に、コンピューターに残しておきたくないイメージを削除します。 *Dockerfile* によって作成されたイメージを削除した後、*Dockerfile* が基にした .NET Core イメージを削除します。 **IMAGE ID** または **REPOSITORY:TAG** の書式に設定された文字列を使うことができます。

```console
docker rmi myimage:latest
docker rmi mcr.microsoft.com/dotnet/core/aspnet:3.1
```

インストールされているイメージの一覧を表示するには、`docker images` コマンドを使います。

> [!NOTE]
> イメージ ファイルは大きくなることがあります。 普通、アプリのテスト中および開発中に作成した一時的なコンテナーは削除します。 通常、ランタイムがインストールされた基本イメージは、そのランタイムを基にして他のイメージをビルドする予定がある場合は、残しておきます。

## <a name="next-steps"></a>次の手順

- [ASP.NET Core アプリケーションをコンテナー化する方法を学習します。](/aspnet/core/host-and-deploy/docker/building-net-docker-images)
- [ASP.NET Core マイクロサービスのチュートリアルを試します。](https://dotnet.microsoft.com/learn/web/aspnet-microservice-tutorial/intro)
- [コンテナーをサポートする Azure サービスを確認します。](https://azure.microsoft.com/overview/containers/)
- [Dockerfile のコマンドについて読みます。](https://docs.docker.com/engine/reference/builder/)
- [Visual Studio 向けのコンテナー ツールを調べます](/visualstudio/containers/overview)
