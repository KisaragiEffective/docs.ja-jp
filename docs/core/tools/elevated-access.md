---
title: dotnet コマンドの特権アクセス
description: 特権アクセスを必要とする dotnet コマンドのベスト プラクティスについて説明します。
author: wli3
ms.date: 06/26/2019
ms.openlocfilehash: 4aff9badfa8ad9b83adc4496d4ebd6df29252e36
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78156765"
---
# <a name="elevated-access-for-dotnet-commands"></a>dotnet コマンドの特権アクセス

ソフトウェア開発のベスト プラクティスでは、最小限の特権を要求するソフトウェアを記述するように開発者に指導しています。 しかしながら、パフォーマンス監視ツールなど、一部のソフトウェアでは、オペレーティング システムのルールに起因し、管理者権限を必要とします。 次のガイダンスでは、そのようなソフトウェアを .NET Core で開発する場合にサポートされるシナリオを紹介しています。

次のコマンドは管理者特権で実行できます。

- [dotnet tool install](dotnet-tool-install.md) など、`dotnet tool` コマンド。
- `dotnet run --no-build`

その他のコマンドを管理者特権で実行することはお勧めしません。 具体的には、[dotnet restore](dotnet-restore.md)、[dotnet build](dotnet-build.md)、[dotnet run](dotnet-run.md) など、MSBuild を使用するコマンドで特権昇格を推奨していません。 主な問題は、dotnet コマンドの実行後、ルートと制限付きアカウントの間をユーザーが行ったり来たりするという、アクセス管理の問題です。 制限付きユーザーであれば、ルート ユーザーが作成したファイルにアクセスできません。 この状況を解決する方法はありますが、このような状況になることがそもそも不要です。

ルートと制限付きアカウントの間を行ったり来たりしない限り、ルートでコマンドを実行できます。 たとえば、Docker コンテナーは既定でルートとして実行するため、この特性があります。

## <a name="global-tool-installation"></a>グローバル ツールのインストール

次の指示では、実行に特権昇格を必要とする .NET Core ツールをインストール、実行、アンインストールするときの推奨方法を示しています。

<!-- markdownlint-disable MD025 -->

# <a name="windows"></a>[Windows](#tab/windows)

### <a name="install-the-tool"></a>ツールをインストールする

フォルダー `%ProgramFiles%\dotnet-tools` が既に存在する場合、次を行い、そのディレクトリを記述または変更する許可が "Users" グループに与えられているかどうかを確認します。

- `%ProgramFiles%\dotnet-tools` フォルダーを右クリックし、 **[プロパティ]** を選択します。 **[共通プロパティ]** ダイアログ ボックスが開きます。
- **[セキュリティ]** タブを選択します **[グループ名またはユーザー名]** で、"Users" グループにディレクトリを記述または変更する権限が与えられているかどうかを確認します。
- "Users" グループでディレクトリを記述または変更できる場合、*dotnet-tools* 以外のツールをインストールするとき、別のディレクトリ名を使用します。

ツールをインストールするには、管理者特権のプロンプトで次のコマンドを実行します。 インストール中、*dotnet-tools* フォルダーが作成されます。

```dotnetcli
dotnet tool install PACKAGEID --tool-path "%ProgramFiles%\dotnet-tools".
```

### <a name="run-the-global-tool"></a>グローバル ツールを実行する

**オプション 1** 管理者特権プロンプトで完全パスを使用します。

```cmd
"%ProgramFiles%\dotnet-tools\TOOLCOMMAND"
```

**オプション 2** 新しく作成したフォルダーを `%Path%` に追加します。 この操作を行うのは 1 回だけです。

```cmd
setx Path "%Path%;%ProgramFiles%\dotnet-tools\"
```

次で実行します。

```cmd
TOOLCOMMAND
```

### <a name="uninstall-the-global-tool"></a>グローバル ツールをアンインストールする

管理者特権プロンプトで次のコマンドを入力します。

```dotnetcli
dotnet tool uninstall PACKAGEID --tool-path "%ProgramFiles%\dotnet-tools"
```

# <a name="linux"></a>[Linux](#tab/linux)

[!INCLUDE [elevated-access-unix](../../../includes/elevated-access-unix.md)]

# <a name="macos"></a>[macOS](#tab/macos)

[!INCLUDE [elevated-access-unix](../../../includes/elevated-access-unix.md)]

---

## <a name="local-tools"></a>ローカル ツール

ローカル ツールの範囲は、サブディレクトリごとに/ユーザーごとに設定されます。 管理者特権で実行するとき、ローカル ツールによって、制限付きユーザー環境が特権環境に与えられます。 Linux と macOS では、この結果、ファイルが root ユーザー専用アクセスで設定されます。 ユーザーが制限付きアカウントに戻ると、そのファイルにアクセスしたり、書き込んだりできなくなります。 そのため、特権昇格を必要とするツールをローカル ツールとしてインストールすることは推奨されません。 その代わり、`--tool-path` オプションとグローバル ツールに関する前述のガイドラインをご利用ください。

## <a name="elevation-during-development"></a>開発中の特権昇格

開発中、アプリケーションをテストする目的で管理者特権が必要になることがあります。 これは、たとえば、IoT アプリで一般的なシナリオです。 Microsoft では、特権昇格なしでアプリケーションを開発し、管理者特権でそれを実行することをお勧めしています。 次のようにいくつかのパターンがあります。

- 生成された実行可能ファイルを使用する (最高のスタートアップ パフォーマンスが与えられます):

   ```dotnetcli
   dotnet build
   sudo ./bin/Debug/netcoreapp3.0/APPLICATIONNAME
   ```

- [dotnet run](dotnet-run.md) コマンドを使用するとき、`—no-build` フラグを指定し、新しいバイナリの生成を回避します。

   ```dotnetcli
   dotnet build
   sudo dotnet run --no-build
   ```

## <a name="see-also"></a>関連項目

- [.NET Core グローバル ツールの概要](global-tools.md)
