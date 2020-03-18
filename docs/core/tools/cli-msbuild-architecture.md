---
title: .NET Core コマンドライン ツールのアーキテクチャ
description: .NET Core ツール レイヤーと最近のバージョンの変更内容について説明します。
ms.date: 03/06/2017
ms.openlocfilehash: fde1a0acb6af9dd65aa3466b4ea37473b2eab6fb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77092916"
---
# <a name="high-level-overview-of-changes-in-the-net-core-tools"></a>.NET Core ツールの変更の概要

このドキュメントでは、*project.json* から MSBuild への移行に関連する変更について、および *csproj* プロジェクト システムについて、.NET Core ツールのレイヤー化と CLI コマンド実装の変更に関する情報と共に説明します。 これらの変更は、2017 年 3 月 7 日の .NET Core SDK 1.0 および Visual Studio 2017 のリリース ([アナウンス](https://devblogs.microsoft.com/dotnet/announcing-net-core-tools-1-0/)をご覧ください) に伴って発生しましたが、.NET Core SDK Preview 3 のリリースで初めて実装されました。

## <a name="moving-away-from-projectjson"></a>project.json から移行する

.NET Core のツールの最大の違いは、プロジェクト システムが [project.json から csproj に移行](https://devblogs.microsoft.com/dotnet/changes-to-project-json/)されることです。 最新バージョンのコマンドライン ツールは *project.json* ファイルをサポートしていません。 これは、project.json ベースのアプリケーションやライブラリをビルド、実行、発行できないことを意味します。 このバージョンのツールを使用するには、既存のプロジェクトを移行するか、新しいプロジェクトを作成します。

この移行の一環として、project.json プロジェクトの構築用に開発されたカスタム ビルド エンジンが、[MSBuild](https://github.com/Microsoft/msbuild) と呼ばれる、成熟した完全な機能を持つビルド エンジンに置き換えられました。 MSBuild は、.NET コミュニティでよく知られているエンジンです。 これはプラットフォームの最初のリリース以来の重要なテクノロジです。 MSBuild では .NET Core アプリケーションをビルドする必要があるため、MSBuild は .NET Core に移植されており、.NET Core が動作するすべてのプラットフォームで使用できます。 .NET Core の最大の保証の 1 つは、これがクロスプラット フォームの開発スタックであるということです。この動きによってこれは保証され続けるように努めました。

> [!TIP]
> MSBuild を初めて使用する際に詳細を学習するには、まず、「[MSBuild の概念](/visualstudio/msbuild/msbuild-concepts)」という記事をお読みください。

## <a name="the-tooling-layers"></a>ツールの階層

ビルド エンジンにおける変更と既存のプロジェクト システムからの移行に伴い、当然ながらいくつかの疑問が生じます。 これらの変更によって .NET Core ツールのエコシステム全体の "レイヤー" に変更はあるのでしょうか? 小さなものからコンポーネントまで、新しいものはありますか?

次の図で Preview 2 のレイヤーを簡単に再確認してみましょう。

![Preview 2 のツールの概要](media/cli-msbuild-architecture/p2-arch.png)

Preview 2 ではツールのレイヤーは簡単です。 下部にある基盤は .NET Core CLI です。 Visual Studio または Visual Studio Code などのその他すべての上位レベルのツールは、プロジェクトのビルドや依存関係の復元などで CLI に依存しています。 たとえば Visual Studio で復元操作を行う場合、CLI の `dotnet restore` ([注を参照](#dotnet-restore-note)) コマンドを呼び出します。

新しいプロジェクト システムに移行すると、前の図は以下のように変わります。

![.NET Core SDK 1.0.0 のアーキテクチャの概要](media/cli-msbuild-architecture/p3-arch.png)

主な違いは、CLI が基本的なレイヤーではなくなり、この役割が "共有 SDK コンポーネント" に置き換えられたことです。 この共有 SDK コンポーネントは、一連のターゲットとそれに関連付けられている、コードのコンパイル、コードの発行、NuGet パッケージの作成などを担うタスクです。 .NET Core SDK はオープン ソースであり、GitHub の [SDK リポジトリ](https://github.com/dotnet/sdk)から入手できます。

> [!NOTE]
> "ターゲット" とは、MSBuild が呼び出すことのできる名前付きの操作を意味する MSBuild の用語です。 これは、通常ターゲットが行うことを期待されているいくつかのロジックを実行する 1 つ以上のタスクと連結されています。 MSBuild では、`Copy` や `Execute` などの多数の既製ターゲットをサポートしています。また、ユーザーがマネージド コードを使用して、独自のタスクを記述し、それらのタスクをターゲットに実行させるよう定義することも可能です。 詳細については、「[MSBuild タスク](/visualstudio/msbuild/msbuild-tasks)」を参照してください。

すべてのツールセットは、CLI を含む、共有 SDK コンポーネントとそのターゲットを消費します。 たとえば、Visual Studio 2019 では、.NET Core プロジェクトの依存関係を復元するために `dotnet restore` ([注を参照](#dotnet-restore-note)) コマンドは呼び出されません。 代わりに、"復元" のターゲットを直接使用します。 これらは MSBuild のターゲットであるため、これらの実行に未加工の MSBuild の [dotnet msbuild](dotnet-msbuild.md) コマンドを使用することも可能です。

### <a name="cli-commands"></a>CLI コマンド

共有 SDK コンポーネントとは、大多数の既存の CLI コマンドが MSBuild のタスクやターゲットとして再実装されたものです。 これは CLI コマンドやツールセットの使用にどのような意味があるのでしょうか?

使用の観点からは、CLI の使用方法には変わりはありません。 CLI には、.NET Core 1.0 Preview 2 リリースに存在していた主要なコマンドがまだあります。

- `new`
- `restore`
- `run`
- `build`
- `publish`
- `test`
- `pack`

引き続きこれらのコマンドを使用して、目的の動作 (新しいプロジェクトの作成、ビルド、発行、パッケージ作成など) を実行できます。 コマンドのヘルプ画面 (`dotnet <command> --help`を使用) またはこのサイトのドキュメントを参照して、それらの動作を理解することができます。

実行の観点からは、CLI コマンドにパラメーターを渡すと、必要なプロパティを設定して目的のターゲットを実行する "生" の MSBuild への呼び出しが作成されます。 次の図でこれを分かりやすく説明します。

   ```dotnetcli
   dotnet publish -o pub -c Release
   ```

このコマンドで、"Release" 構成を使用してアプリケーションを `pub` フォルダーに発行します。 このコマンドは、内部では次の MSBuild の呼び出しに変換されます。

   ```dotnetcli
   dotnet msbuild -t:Publish -p:OutputPath=pub -p:Configuration=Release
   ```

この規則の注目すべき例外は、`new` コマンドと `run` のコマンドです。 それらは MSBuild ターゲットとして実装されていません。

<a name="dotnet-restore-note"></a>
[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]
