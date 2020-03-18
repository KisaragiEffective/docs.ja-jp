---
title: 'チュートリアル: macOS での Visual Studio Code を使用した .NET Core ソリューションの作成'
description: このドキュメントでは、Visual Studio Code を使用して .NET Core ソリューションを作成する手順とワークフローを説明します。
ms.date: 12/19/2019
ms.openlocfilehash: f5da16d413ddc25587ff35550fe9f308dc87f4bb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78156596"
---
# <a name="tutorial-create-a-net-core-solution-in-macos-using-visual-studio-code"></a>チュートリアル: macOS での Visual Studio Code を使用した .NET Core ソリューションの作成

このドキュメントでは、macOS 用の .NET Core ソリューションを作成する手順とワークフローを説明します。 プロジェクトと単体テストを作成し、デバッグ ツールを使用して、[NuGet](https://www.nuget.org/) からサードパーティ製ライブラリを組み込む方法について説明します。

> [!NOTE]
> この記事では、macOS で [Visual Studio Code](https://code.visualstudio.com) を使用します。

## <a name="prerequisites"></a>必須コンポーネント

[.NET Core SDK](https://dotnet.microsoft.com/download) のインストール。 .NET Core SDK には、.NET Core のフレームワークとランタイムの最新リリースが含まれています。

[Visual Studio Code](https://code.visualstudio.com) のインストール。 この記事の中では、.NET Core の開発エクスペリエンスが向上する Visual Studio Code 拡張機能もインストールします。

Visual Studio Code C# 拡張機能をインストールするには、Visual Studio Code を開き、<kbd>Fn</kbd>+<kbd>F1</kbd> を押して Visual Studio Code パレットを開きます。 「**ext install**」と入力して、拡張機能の一覧を表示します。 C# 拡張機能を選択します。 Visual Studio Code を再起動して、拡張機能をアクティブにします。 詳細については、[Visual Studio Code C# 拡張機能のドキュメント](https://github.com/OmniSharp/omnisharp-vscode/blob/master/debugger.md)を参照してください。

## <a name="get-started"></a>作業開始

このチュートリアルでは 3 つのプロジェクト (ライブラリ プロジェクト、そのライブラリ プロジェクトのテスト、およびライブラリを使用するコンソール アプリケーション) を作成します。 GitHub の dotnet/samples レポジトリで、この記事の[ソースを表示またはダウンロード](https://github.com/dotnet/samples/tree/master/core/getting-started/golden)することができます。 ダウンロード方法については、「[サンプルおよびチュートリアル](../../samples-and-tutorials/index.md#viewing-and-downloading-samples)」を参照してください。

Visual Studio Code を開始します。 <kbd>Ctrl</kbd><kbd>\`</kbd> (バッククォートまたはアクサン グラーブ) キーを押すか、メニューから **[表示]**  >  **[ターミナル]** の順に選択し、Visual Studio Code で埋め込みターミナルを開きます。 Visual Studio Code の外部で作業を行う場合は、エクスプローラーの **[コマンド プロンプトで開く]** コマンド (macOS または Linux の場合は **[ターミナルで開く]** ) を使用して外部シェルを開くこともできます。

1 つ以上の .NET Core プロジェクトのコンテナーとして機能する、ソリューション ファイルの作成を開始します。 ターミナル内で、[`dotnet new`](../tools/dotnet-new.md) コマンドを実行し、*golden* という名前の新しいフォルダー内に、新しいソリューション *golden.sln* を作成します。

```dotnetcli
dotnet new sln -o golden
```

新しい *golden* フォルダーに移動し、以下のコマンドを実行して、ライブラリ プロジェクトを作成します。これにより、*library* フォルダーに *library.csproj* と *Class1.cs* の 2 つのファイルが生成されます。

```dotnetcli
dotnet new classlib -o library
```

以下のように、[`dotnet sln`](../tools/dotnet-sln.md) コマンドを実行し、新しく作成された *library.csproj* プロジェクトをソリューションに追加します。

```dotnetcli
dotnet sln add library/library.csproj
```

*library.csproj* ファイルには、次の情報が含まれています。

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
  </PropertyGroup>

</Project>
```

このライブラリ メソッドでは、JSON 形式でオブジェクトのシリアル化と逆シリアル化を行います。 JSON のシリアル化および逆シリアル化をサポートするには、`Newtonsoft.Json` NuGet パッケージへの参照を追加します。 `dotnet add` コマンドにより、新しい項目がプロジェクトに追加されます。 NuGet パッケージへの参照を追加するには、[`dotnet add package`](../tools/dotnet-add-package.md) コマンドを使用して、パッケージの名前を指定します。

```dotnetcli
dotnet add library package Newtonsoft.Json
```

これにより、`Newtonsoft.Json` とその依存関係がライブラリ プロジェクトに追加されます。 あるいは、*library.csproj* ファイルを手動で編集し、次のノードを追加します。

```xml
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" Version="12.0.2" />
</ItemGroup>
```

[`dotnet restore`](../tools/dotnet-restore.md) を実行します ([注記参照](#dotnet-restore-note))。これにより、依存関係が復元され、*project.assets.json* ファイルなどの 3 つのファイルを含む *obj* フォルダーが *library* 内に作成されます。

```dotnetcli
dotnet restore
```

*library* フォルダーで、ファイル *Class1.cs* の名前を *Thing.cs* に変更します。 このコードを次のコードを使って置き換えます。

```csharp
using static Newtonsoft.Json.JsonConvert;

namespace Library
{
    public class Thing
    {
        public int Get(int left, int right) =>
            DeserializeObject<int>($"{left + right}");
    }
}
```

`Thing` クラスには、`Get` という 1 つのパブリック メソッドが含まれます。このメソッドは 2 つの数値の合計を返しますが、そのためには合計値を文字列に変換した後、それを整数に逆シリアル化します。 このコードでは、[`using static` ディレクティブ](../../csharp/language-reference/keywords/using-static.md)、[式本体のメンバー](../../csharp/whats-new/csharp-7.md#more-expression-bodied-members)、および[文字列補間](../../csharp/language-reference/tokens/interpolated.md)など、C# の最新の機能をいくつか利用しています。

[`dotnet build`](../tools/dotnet-build.md) コマンドを使用して、ライブラリをビルドします。 これにより、*golden/library/bin/Debug/netstandard1.4* に *library.dll* ファイルが生成されます。

```dotnetcli
dotnet build
```

## <a name="create-the-test-project"></a>テスト プロジェクトの作成

ライブラリのテスト プロジェクトをビルドします。 *golden* フォルダーから、次のようにして新しいテスト プロジェクトを作成します。

```dotnetcli
dotnet new xunit -o test-library
```

次のようにしてテスト プロジェクトをソリューションに追加します。

```dotnetcli
dotnet sln add test-library/test-library.csproj
```

コンパイラがライブラリ プロジェクトを見つけて使用できるように、前のセクションで作成したライブラリにプロジェクト参照を追加します。 次の [`dotnet add reference`](../tools/dotnet-add-reference.md) コマンドを使用します。

```dotnetcli
dotnet add test-library/test-library.csproj reference library/library.csproj
```

あるいは、*test-library.csproj* ファイルを手動で編集し、次のノードを追加します。

```xml
<ItemGroup>
  <ProjectReference Include="..\library\library.csproj" />
</ItemGroup>
```

これで依存関係が正しく構成されました。次は、ライブラリのテストを作成します。 *UnitTest1.cs* を開き、内容を次のコードに置き換えます。

```csharp
using Library;
using Xunit;

namespace TestApp
{
    public class LibraryTests
    {
        [Fact]
        public void TestThing()
        {
            Assert.NotEqual(42, new Thing().Get(19, 23));
        }
    }
}
```

最初に失敗する単体テスト (`Assert.NotEqual`) を作成する場合、値 42 は 19+23 (つまり、42) と等しくないことをアサートすることに注意してください。 単体テストのビルドにおける重要な手順は、最初に一度失敗するテストを作成して、そのロジックを確認することです。

*golden* フォルダーから、次のコマンドを実行します。

```dotnetcli
dotnet restore
dotnet test test-library/test-library.csproj
```

これらのコマンドは、再帰的にすべてのプロジェクトを検出し、依存関係を復元してビルドし、xUnit テスト ランナーをアクティブにしてテストを実行します。 予期したとおり、1 つのテストに失敗します。

*UnitTest1.cs* ファイルを編集し、アサーションを `Assert.NotEqual` から `Assert.Equal` に変更します。 *golden* フォルダーから次のコマンドを実行し、テストを再実行します。今回は成功します。

```dotnetcli
dotnet test test-library/test-library.csproj
```

## <a name="create-the-console-app"></a>コンソール アプリの作成

次の手順で作成するコンソール アプリは、前の手順で作成したライブラリ プロジェクトに対する依存関係を認識し、実行時にそのライブラリ メソッドを呼び出します。 この開発パターンを使用して、複数のプロジェクトで再利用可能なライブラリの作成方法を確認します。

次のように、*golden* フォルダーから新しいコンソール アプリケーションを作成します。

```dotnetcli
dotnet new console -o app
```

次のように、コンソール アプリ プロジェクトをソリューションに追加します。

```dotnetcli
dotnet sln add app/app.csproj
```

次のように `dotnet add reference` コマンドを実行して、ライブラリに対する依存関係を作成します。

```dotnetcli
dotnet add app/app.csproj reference library/library.csproj
```

`dotnet restore` を実行し、ソリューションの 3 つのプロジェクトの依存関係を復元します ([注記参照](#dotnet-restore-note))。 *Program.cs* を開き、`Main` メソッドの内容を次の行に置き換えます。

```csharp
WriteLine($"The answer is {new Thing().Get(19, 23)}");
```

次のように、2 つの `using` ディレクティブを *Program.cs* ファイルの先頭に追加します。

```csharp
using static System.Console;
using Library;
```

次の `dotnet run` コマンドを実行して、実行可能ファイルを実行します。`dotnet run` の `-p` オプションは、メイン アプリケーションのプロジェクトを指定します。 アプリは "The answer is 42" という文字列を生成します。

```dotnetcli
dotnet run -p app/app.csproj
```

## <a name="debug-the-application"></a>アプリケーションのデバッグ

`Main` メソッドの `WriteLine` ステートメントにブレークポイントを設定します。 そのためには、カーソルが `WriteLine` 行にある状態で <kbd>Fn</kbd>+<kbd>F9</kbd> キーを押すか、ブレークポイントを設定する行の左余白でマウスをクリックします。 コード行の横の余白に赤い丸が表示されます。 ブレークポイントに達した場合、ブレークポイント行が実行される*前*にコードの実行が停止します。

Visual Studio Code ツール バーでデバッグ アイコンを選択するか、メニュー バーから **[表示]**  >  **[デバッグ]** の順に選択するか、あるいはキーボード ショートカットの <kbd>&#8679;</kbd><kbd>&#8984;</kbd><kbd>D</kbd> を使用して、デバッガー タブを開きます。

![Visual Studio Code デバッガー](./media/using-on-macos/visual-studio-code-debugger.png)

[再生] ボタンを押して、デバッガーでアプリケーションを開始します。 このプロジェクトでテスト プロジェクトとアプリケーションの両方を作成しました。 開始するプロジェクトをデバッガーから尋ねられます。 「アプリ」プロジェクトを選択します。 アプリは実行を開始し、ブレークポイントに達した時点で停止します。 `Get` メソッドにステップ インし、正しい引数を渡したことを確認します。 答えが 42 であることを確認してください。

<a name="dotnet-restore-note"></a>
[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]
