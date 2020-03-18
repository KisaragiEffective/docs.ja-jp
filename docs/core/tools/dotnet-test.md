---
title: dotnet test コマンド
description: dotnet test コマンドは、指定されたプロジェクトで単体テストを実行する場合に使用されます。
ms.date: 02/27/2020
ms.openlocfilehash: 6e906ab396a788905c99f50e73390b765b240efc
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78157012"
---
# <a name="dotnet-test"></a>dotnet test

**この記事の対象:** ✔️ .NET Core 2.1 SDK 以降のバージョン

## <a name="name"></a>名前

`dotnet test` - 単体テストを実行するために使用される .NET テスト ドライバー。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet test [<PROJECT>] [-a|--test-adapter-path] [--blame]
    [-c|--configuration] [--collect] [-d|--diag] [-f|--framework]
    [--filter] [-l|--logger] [--no-build] [--no-restore]
    [-o|--output] [-r|--results-directory] [-s|--settings]
    [-t|--list-tests] [-v|--verbosity] [-- <RunSettings arguments>]

dotnet test [-h|--help]
```

## <a name="description"></a>説明

`dotnet test` コマンドは、指定されたプロジェクトで単体テストを実行する場合に使用されます。 `dotnet test` コマンドは、プロジェクト用に指定されたテスト ランナーのコンソール アプリケーションを起動します。 テスト ランナーでは、単体テスト フレームワーク (MSTest、NUnit、xUnit など) 用に定義されたテストを実行し、各テストの成功または失敗をレポートします。 すべてのテストが成功した場合、テスト ランナーは 0 の終了コードを返します。それ以外の、いずれかのテストに失敗した場合は、1 を返します。 テスト ランナーと単体テスト ライブラリは、NuGet パッケージとしてパッケージ化され、プロジェクトの通常の依存関係として復元されます。

テスト プロジェクトでは、通常の `<PackageReference>` 要素を使用してテスト ランナーを指定します。次のサンプル プロジェクト ファイルのようになります。

[!code-xml[XUnit Basic Template](../../../samples/snippets/csharp/xunit-test/xunit-test.csproj)]

## <a name="arguments"></a>引数

- **`PROJECT`**

  テスト プロジェクトへのパス。 指定しない場合は、既定で現在のディレクトリに設定されます。

## <a name="options"></a>オプション

- **`a|--test-adapter-path <PATH_TO_ADAPTER>`**

  テスト実行で指定されたパスからカスタムのテスト アダプターを使用します。

- **`-blame`**

  変更履歴モードでテストを実行します。 このオプションは、テスト ホストがクラッシュする原因となる問題のあるテストを分離するために役立ちます。 これは、現在のディレクトリ内に出力ファイルを *Sequence.xml* として作成します。このファイルは、クラッシュ前にテストの実行順序をキャプチャします。

- **`c|--configuration {Debug|Release}`**

  ビルド構成を定義します。 既定値は `Debug` ですが、プロジェクトの構成がこの既定の SDK 設定をオーバーライドする可能性があります。

- **`-collect <DATA_COLLECTOR_FRIENDLY_NAME>`**

  テストの実行のためのデータ コレクターを有効にします。 詳細については、[「Monitor and analyze test run」](https://aka.ms/vstest-collect) (テストの実行のモニターと分析) を参照してください。

- **`d|--diag <PATH_TO_DIAGNOSTICS_FILE>`**

  テスト プラットフォームの診断モードを有効にし、指定したファイルに診断メッセージを出力します。

- **`f|--framework <FRAMEWORK>`**

  特定の[フレームワーク](../../standard/frameworks.md)のテスト バイナリを検索します。

- **`--filter <EXPRESSION>`**

  指定された式を使用して、現在のプロジェクト内のテストを除外します。 詳細については、「[フィルター オプションの詳細](#filter-option-details)」セクションをご覧ください。 選択的単体テストのフィルター処理の使用方法に関する詳細と例については、「[選択的単体テストの実行](../testing/selective-unit-tests.md)」をご覧ください。

- **`h|--help`**

  コマンドの短いヘルプを印刷します。

- **`l|--logger <LoggerUri/FriendlyName>`**

  テスト結果のロガーを指定します。

- **`--no-build`**

  実行前にテスト プロジェクトをビルドしません。 また、- `--no-restore` フラグが暗黙的に設定されます。

- **`--no-restore`**

  コマンドを実行するときに、暗黙的な復元を実行しません。

- **`-o|--output <OUTPUT_DIRECTORY>`**

  実行するバイナリを検索するディレクトリです。

- **`-r|--results-directory <PATH>`**

  テスト結果が配置されるディレクトリです。 指定されたディレクトリが存在しない場合は、作成されます。

- **`-s|--settings <SETTINGS_FILE>`**

  テストの実行に使用する `.runsettings` ファイルです。 [`.runsettings` ファイルを使用して単体テストを構成します。](/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file)

- **`-t|--list-tests`**

  現在のプロジェクトで検出されたすべてのテストを一覧表示します。

- **`-v|--verbosity <LEVEL>`**

  コマンドの詳細レベルを設定します。 指定できる値は、`q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]`、および `diag[nostic]` です。

- `RunSettings` 引数

  引数は、テストの `RunSettings` 構成として渡されます。 引数は、"-- " に続く `[name]=[value]` のペアとして指定されます (-- の後ろのスペースに注意してください)。 複数の `[name]=[value]` のペアを区切るにはスペースを使用します。

  例 : `dotnet test -- MSTest.DeploymentEnabled=false MSTest.MapInconclusiveToFailed=True`

  詳細については、[vstest.console.exe の RunSettings 引数渡し](https://github.com/Microsoft/vstest-docs/blob/master/docs/RunSettingsArguments.md)に関するページをご覧ください。

## <a name="examples"></a>使用例

- 現在のディレクトリのプロジェクトでテストを実行します。

  ```dotnetcli
  dotnet test
  ```

- `test1` プロジェクトでテストを実行します。

  ```dotnetcli
  dotnet test ~/projects/test1/test1.csproj
  ```

- 現在のディレクトリでプロジェクトのテストを実行し、trx 形式でテスト結果ファイルを生成します。

  ```dotnetcli
  dotnet test --logger trx
  ```

## <a name="filter-option-details"></a>フィルター オプションの詳細

`--filter <EXPRESSION>`

`<Expression>` の形式は `<property><operator><value>[|&<Expression>]` です。

`<property>` は `Test Case` の属性です。 よく利用される単体テスト フレームワークでサポートされるプロパティは以下の通りです。

| テスト フレームワーク | サポートされるプロパティ                                                                                      |
| -------------- | --------------------------------------------------------------------------------------------------------- |
| MSTest         | <ul><li>FullyQualifiedName</li><li>名前</li><li>ClassName</li><li>優先度</li><li>TestCategory</li></ul> |
| xUnit          | <ul><li>FullyQualifiedName</li><li>DisplayName</li><li>Traits</li></ul>                                   |

`<operator>` は、プロパティと値の関係を示します。

| 演算子 | 関数        |
| :------: | --------------- |
| `=`      | 完全一致     |
| `!=`     | 完全一致ではない |
| `~`      | 内容        |
| `!~`     | 含まない    |

`<value>` は文字列です。 すべての参照で大文字と小文字が区別されます。

`<operator>` を含まない式は、自動的に `FullyQualifiedName` プロパティの `contains` とみなされます (たとえば、`dotnet test --filter xyz` は `dotnet test --filter FullyQualifiedName~xyz` と同じです)。

式は条件演算子を使用して結合できます。

| 演算子            | 関数 |
| ------------------- | -------- |
| <code>&#124;</code> | OR       |
| `&`                 | AND      |

条件演算子を使用する場合は、式をかっこで囲みます (例: `(Name~TestMethod1) | (Name~TestMethod2)`)。

選択的単体テストのフィルター処理の使用方法に関する詳細と例については、「[選択的単体テストの実行](../testing/selective-unit-tests.md)」をご覧ください。

## <a name="see-also"></a>関連項目

- [フレームワークとターゲット](../../standard/frameworks.md)
- [.NET Core のランタイム識別子 (RID) のカタログ](../rid-catalog.md)
