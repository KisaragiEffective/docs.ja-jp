---
title: .NET Core CLI を使用して NuGet パッケージを作成する
description: ’dotnet pack’ コマンドを使用して NuGet パッケージを作成する方法を説明します。
author: cartermp
ms.date: 06/20/2016
ms.technology: dotnet-cli
ms.openlocfilehash: 3f8e75a501cfc48e1c416f71e91290cab1a4ffae
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76920917"
---
# <a name="how-to-create-a-nuget-package-with-the-net-core-cli"></a>.NET Core CLI を使用して NuGet パッケージを作成する方法

> [!NOTE]
> 以下は、Unix を使用する場合のコマンド ライン サンプルです。 ここに示されている `dotnet pack` コマンドは Windows でも同じように機能します。

.NET Standard ライブラリと .NET Core ライブラリは NuGet パッケージとして配布されることが期待されています。 実際に、.NET Standard ライブラリはすべてそのように配布され、使用されています。 `dotnet pack` コマンドを使用して行うのが最も簡単です。

たとえば、NuGet 経由で配布する新しい優れたライブラリを作成したとします。 クロス プラットフォーム ツールを使用して NuGet パッケージを作成すれば、正確に実行できます。 次の例では、**をターゲットとする**SuperAwesomeLibrary`netstandard1.0` というライブラリを想定します。

推移的依存関係がある (つまり、別のパッケージに依存するプロジェクトがある) 場合、NuGet パッケージを作成する前に、`dotnet restore` コマンドを使用してソリューション全体のパッケージを必ず復元します。 そうしないと、`dotnet pack` コマンドが正しく機能しません。

[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]

パッケージが復元されたことを確認したら、以下のコマンドを実行してライブラリがあるディレクトリに移動できます。

```console
cd src/SuperAwesomeLibrary
```

その後、コマンド ラインから以下の 1 つのコマンドのみを実行します。

```dotnetcli
dotnet pack
```

これで */bin/Debug* フォルダーは次のようになります。

```console
$ ls bin/Debug
netstandard1.0/
SuperAwesomeLibrary.1.0.0.nupkg
SuperAwesomeLibrary.1.0.0.symbols.nupkg
```

これにより、デバッグ可能なパッケージが生成されます。 リリース バイナリと共に NuGet パッケージをビルドする場合、必要なのは、`--configuration` (または `-c`) スイッチを追加し、引数として `release` を使用することだけです。

```dotnetcli
dotnet pack --configuration release
```

これで、 */bin* フォルダーに、NuGet パッケージとリリース バイナリを含む *release* フォルダーが生成されます。

```console
$ ls bin/release
netstandard1.0/
SuperAwesomeLibrary.1.0.0.nupkg
SuperAwesomeLibrary.1.0.0.symbols.nupkg
```

これで、NuGet パッケージを発行するために必要なファイルが準備できました。

## <a name="dont-confuse-dotnet-pack-with-dotnet-publish"></a>`dotnet pack`と `dotnet publish` を混同しないようにしてください

ここで `dotnet publish` コマンドを使用しても意味がありません。 `dotnet publish` コマンドは、同じバンドルにすべての依存関係があるアプリケーションを配置するためのものであり、NuGet 経由で配布して使用する NuGet パッケージを生成するためのものではありません。

## <a name="see-also"></a>参照

- [クイック スタート: パッケージの作成と公開](/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli)
