---
title: .NET Core を使用した REST クライアントの作成
description: このチュートリアルでは、.NET Core と C# 言語のさまざまな機能を説明します。
ms.date: 01/09/2020
ms.assetid: 51033ce2-7a53-4cdd-966d-9da15c8204d2
ms.openlocfilehash: 5796df2d2fd8c4d9aaca783d720448c90858c067
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79156858"
---
# <a name="rest-client"></a>REST クライアント

このチュートリアルでは、.NET Core と C# 言語のさまざまな機能を説明します。 内容は以下のとおりです。

* .NET Core CLI の基本事項。
* C# 言語機能の概要
* NuGet での依存関係の管理
* HTTP 通信
* JSON 情報の処理
* 属性を使用した構成の管理

GitHub 上の REST サービスに対して HTTP 要求を発行するアプリケーションをビルドします。 JSON 形式で情報を読み取り、その JSON パケットを C# オブジェクトに変換します。 最後に、C# オブジェクトを操作する方法について説明します。

このチュートリアルには、多くの機能が含まれています。 それらを 1 つずつビルドしてみましょう。

このトピックの[最終的なサンプル](https://github.com/dotnet/samples/tree/master/csharp/getting-started/console-webapiclient)も参照したい方は、ダウンロードできます。 ダウンロード方法については、「[サンプルおよびチュートリアル](../../samples-and-tutorials/index.md#viewing-and-downloading-samples)」を参照してください。

## <a name="prerequisites"></a>必須コンポーネント

お使いのコンピューターを、.NET Core が実行されるように設定する必要があります。 インストールの手順については、[.NET Core のダウンロード](https://dotnet.microsoft.com/download) ページを参照してください。 このアプリケーションは、Windows、Linux、macOS または Docker コンテナーで実行できます。
お好みのコード エディターをインストールしてください。 次の説明では、オープン ソースのクロス プラットフォーム エディターである [Visual Studio Code](https://code.visualstudio.com/) を使用しています。 しかし、他の使い慣れたツールを使用しても構いません。

## <a name="create-the-application"></a>アプリケーションを作成する

最初に新しいアプリケーションを作成します。 コマンド プロンプトを開き、アプリケーション用の新しいディレクトリを作成します。 それを、現在のディレクトリとしてください。 コンソール ウィンドウに次のコマンドを入力します。

```dotnetcli
dotnet new console --name WebApiClient
```

これで、基本的な "Hello World" アプリケーションのスターター ファイルが作成されます。 プロジェクト名は "WebApiClient" です。 これは新しいプロジェクトであるため、依存関係はありません。 1 回目の実行では、.NET Core フレームワークがダウンロードされ、開発証明書がインストールされ、NuGet パッケージ マネージャーが実行されて、不足している依存関係が復元されます。

変更を開始する前に、コマンド プロンプトに「`dotnet run`」(「[注](#dotnet-restore-note)」を参照してください) と入力してアプリケーションを実行します。 環境に依存関係がない場合、`dotnet run` では自動的に `dotnet restore` が実行されます。 アプリケーションのリビルドが必要な場合にも `dotnet build` が実行されます。
初期設定の後は、プロジェクトにとって意味がある場合にのみ `dotnet restore` または `dotnet build` を実行する必要があります。

## <a name="adding-new-dependencies"></a>新しい依存関係を追加する

.NET Core の重要な設計目標の 1 つは、.NET インストール サイズを最小限に抑えることです。 その一部の機能のための追加ライブラリがアプリケーションで必要な場合は、C# プロジェクト (\*.csproj) ファイルにそれらの依存関係を追加します。 ここで示す例の場合は、`System.Runtime.Serialization.Json` パッケージを追加して、アプリケーションが JSON 応答を処理できるようにする必要があります。

このアプリケーションには、`System.Runtime.Serialization.Json` パッケージが必要です。 次の [.NET CLI](../../core/tools/dotnet-add-package.md) コマンドを実行して、それをご自身のプロジェクトに追加します。

```dotnetcli
dotnet add package System.Text.Json
```

## <a name="making-web-requests"></a>Web 要求を作成する

Web サイトからデータの取得を開始する準備ができました。 このアプリケーションでは、[GitHub API](https://developer.github.com/v3/) から情報を読み取ります。 [.NET Foundation](https://www.dotnetfoundation.org/) にあるプロジェクトに関する情報を読み取ります。 最初に、プロジェクトに関する情報を取得する GitHub API に対する要求を作成します。 使用するエンドポイントは <https://api.github.com/orgs/dotnet/repos> です。 HTTP GET 要求を使用して、これらのプロジェクトに関する情報をすべて取得します。
HTTP GET 要求はブラウザーでも使用されるので、ブラウザーに URL を貼り付けて、取得および処理する情報を確認できます。

<xref:System.Net.Http.HttpClient> クラスを使用して Web 要求を作成します。 最新のすべての .NET API と同様に、<xref:System.Net.Http.HttpClient> は実行時間の長い API の非同期メソッドだけをサポートします。
最初に非同期メソッドを作成します。 アプリケーションの機能をビルドする際に実装を記述します。 最初に、プロジェクト ディレクトリ内の `program.cs` ファイルを開き、`Program` クラスに次のメソッドを追加します。

```csharp
private static async Task ProcessRepositories()
{
}
```

`Main` メソッドの先頭に `using` ディレクティブを追加して、C# コンパイラに <xref:System.Threading.Tasks.Task> 型を認識させる必要があります。

```csharp
using System.Threading.Tasks;
```

この時点でプロジェクトをビルドすると、このメソッドに対して警告が生成されます。これは、`await` 演算子がメソッドに含まれておらず、メソッドが同期的に実行されるためです。 現時点ではこの警告を無視してください。`await` 演算子は、メソッドを記述する際に追加します。

次に、`namespace` ステートメントに定義されている名前空間の名前を、既定値の `ConsoleApp` から `WebAPIClient` に変更します。 後から、この名前空間で `repo` クラスを定義します。

次に、`Main` メソッドを更新してこのメソッドを呼び出します。 `ProcessRepositories` メソッドはタスクを返します。そのタスクが完了する前にプログラムを終了しないでください。 そのため、`Main` のシグネチャを変更する必要があります。 `async` 修飾子を追加し、戻り値の型を `Task` に変更します。 次に、メソッドの本体で、呼び出しを `ProcessRepositories` に追加します。 そのメソッド呼び出しに `await` キーワードを追加します。

```csharp
static async Task Main(string[] args)
{
    await ProcessRepositories();
}
```

これで、何も実行せず、非同期的に実行されるプログラムの準備ができました。 これを改善しましょう。

まず、Web からデータを取得できるオブジェクトが必要です。この条件に合うものとして、<xref:System.Net.Http.HttpClient> を使用できます。 このオブジェクトは要求と応答を処理します。 *Program.cs* ファイル内の `Program` クラスで該当する型の単一のインスタンスをインスタンス化します。

```csharp
namespace WebAPIClient
{
    class Program
    {
        private static readonly HttpClient client = new HttpClient();

        static async Task Main(string[] args)
        {
            //...
        }
    }
}
```

`ProcessRepositories` メソッドに戻って、最初のバージョンのプログラムを記述します。

```csharp
private static async Task ProcessRepositories()
{
    client.DefaultRequestHeaders.Accept.Clear();
    client.DefaultRequestHeaders.Accept.Add(
        new MediaTypeWithQualityHeaderValue("application/vnd.github.v3+json"));
    client.DefaultRequestHeaders.Add("User-Agent", ".NET Foundation Repository Reporter");

    var stringTask = client.GetStringAsync("https://api.github.com/orgs/dotnet/repos");

    var msg = await stringTask;
    Console.Write(msg);
}
```

また、ファイルの先頭に 2 つの新しい `using` ディレクティブを追加して、プログラムをコンパイルする必要があります。

```csharp
using System.Net.Http;
using System.Net.Http.Headers;
```

この最初のバージョンでは、.NET Foundation にあるすべてのリポジトリのリストを読み取る Web 要求を作成します (.NET Foundation の gitHub ID は "dotnet" です)。 最初の数行では、この要求の <xref:System.Net.Http.HttpClient> を設定します。 最初は、GitHub の JSON 応答を受け入れるように構成されます。
この形式は単なる JSON です。 次の行では、このオブジェクトからのすべての要求にユーザー エージェント ヘッダーを追加します。 これらの 2 つのヘッダーは、GitHub サーバー コードによってチェックされ、GitHub から情報を取得するために必要です。

<xref:System.Net.Http.HttpClient> を構成したら、Web 要求を作成して応答を取得します。 この最初のバージョンでは、<xref:System.Net.Http.HttpClient.GetStringAsync(System.String)?displayProperty=nameWithType> 簡易メソッドを使います。 この簡易メソッドは、Web 要求を作成するタスクを開始し、要求が返されると応答ストリームを読み取って、ストリームからコンテンツを抽出します。 応答の本文は <xref:System.String> として返されます。 この文字列は、タスクが完了すると使用できます。

このメソッドの最後の 2 行は、そのタスクを待機し、コンソールに応答を出力します。
アプリケーションをビルドして実行してください。 `ProcessRepositories` に `await` 演算子が含まれているため、ビルドの警告が表示されなくなりました。 JSON 形式の長いテキストが表示されます。

## <a name="processing-the-json-result"></a>JSON の結果を処理する

この時点では、Web サーバーからの応答を取得し、その応答に含まれているテキストを表示するコードの記述が完了しています。 次に、JSON 応答を C# オブジェクトに変換します。

<xref:System.Text.Json.JsonSerializer?displayProperty=nameWithType> クラスは、オブジェクトを JSON にシリアル化し、JSON をオブジェクトに逆シリアル化します。 最初にクラスを定義して、GitHub API から返される `repo` JSON オブジェクトを表します。

```csharp
using System;

namespace WebAPIClient
{
    public class Repository
    {
        public string name { get; set; }
    }
}
```

"repo.cs" という新しいファイルに上記のコードを記述します。 このバージョンのクラスは、JSON データを処理する最もシンプルなパスを表します。 クラス名とメンバー名は、C# の規約に従うのではなく、JSON パケットで使用される名前に一致します。 後でいくつかの構成属性を指定して、これを修正します。 このクラスは、JSON のシリアル化と逆シリアル化に関する、もう一つの重要な特性を示しています:JSON パケット内のすべてのフィールドがこのクラスの一部というわけではありません。
JSON シリアライザーは、使用されているクラス型に含まれていない情報を無視します。
この機能により、JSON パケット内のフィールドのサブセットのみを操作する型を容易に作成できます。

型の作成が完了したら、逆シリアル化を実行します。

次に、シリアライザーを使用して、JSON を C# オブジェクトに変換します。 `ProcessRepositories` メソッド内の <xref:System.Net.Http.HttpClient.GetStringAsync(System.String)> の呼び出しを次の 3 行に置き換えます。

```csharp
var streamTask = client.GetStreamAsync("https://api.github.com/orgs/dotnet/repos");
var repositories = await JsonSerializer.DeserializeAsync<List<Repository>>(await streamTask);
```

新しい名前空間を使用しているため、ファイルの先頭にそれを追加する必要もあります。

```csharp
using System.Text.Json;
```

<xref:System.Net.Http.HttpClient.GetStringAsync(System.String)> ではなく、<xref:System.Net.Http.HttpClient.GetStreamAsync(System.String)> が使用されていることをご確認ください。 シリアライザーは、文字列の代わりにストリームをソースとして使用します。 前のコード スニペットの 2 行目で使用されている C# 言語のいくつかの機能について説明します。 <xref:System.Text.Json.JsonSerializer.DeserializeAsync%60%601(System.IO.Stream,System.Text.Json.JsonSerializerOptions,System.Threading.CancellationToken)?displayProperty=nameWithType> への最初の引数は `await` 式です (他の 2 つのパラメーターは省略可能で、このコード スニペットでは省略されています)。await 式は、コード内のほとんどの場所に表示される可能性があります (これまでは代入ステートメントの一部としてしか表示されていませんでした)。 `Deserialize` メソッドは "*ジェネリック*" です。つまり、JSON テキストから作成するオブジェクトの種類に対して型引数を指定する必要があります。 この例では、汎用オブジェクト <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> の 1 つである `List<Repository>` に逆シリアル化しています。 `List<>` クラスには、オブジェクトのコレクションが格納されます。 型引数は、`List<>` に格納されているオブジェクトの型を宣言します。 JSON テキストは、リポジトリ オブジェクトのコレクションを表しているため、型引数は `Repository` です。

このセクションでの作業はほぼ完了です。 JSON を C# オブジェクトに変換したので、次は各リポジトリの名前を表示します。 次の行があるとします。

```csharp
var msg = await stringTask;   //**Deleted this
Console.Write(msg);
```

この行を次の行に置き換えます。

```csharp
foreach (var repo in repositories)
    Console.WriteLine(repo.name);
```

アプリケーションをコンパイルして実行します。 .NET Foundation に含まれるリポジトリの名前が出力されます。

## <a name="controlling-serialization"></a>シリアル化を制御する

機能を追加する前に、`[JsonPropertyName]` 属性を使用して `name` プロパティに対処しましょう。 repo.cs の `name` フィールドの宣言を次のように変更してください。

```csharp
[JsonPropertyName("name")]
public string Name { get; set; }
```

`[JsonPropertyName]` 属性を使用するには、`using` ディレクティブに <xref:System.Text.Json.Serialization> 名前空間を追加する必要があります。

```csharp
using System.Text.Json.Serialization;
```

この変更により、program.cs で各リポジトリの名前を記述するコードを変更する必要があります。

```csharp
Console.WriteLine(repo.Name);
```

`dotnet run` を実行して、マッピングが正しいことを確認します。 前と同じ出力が表示されるはずです。

新機能を追加する前に、もう 1 つの変更を行います。 `ProcessRepositories` メソッドは非同期作業を行って、リポジトリのコレクションを返すことができます。 そのメソッドから `List<Repository>` を返して、情報を記述するコードを `Main` メソッドに移動します。

結果が `Repository` オブジェクトのリストであるタスクを返すように `ProcessRepositories` のシグネチャを変更します。

```csharp
private static async Task<List<Repository>> ProcessRepositories()
```

続いて、JSON 応答の処理後にリポジトリを返します。

```csharp
var streamTask = client.GetStreamAsync("https://api.github.com/orgs/dotnet/repos");
var repositories = await JsonSerializer.DeserializeAsync<List<Repository>>(await streamTask);
return repositories;
```

このメソッドを `async` とマークしたので、コンパイラは戻り値として `Task<T>` オブジェクトを生成します。
次に、`Main` メソッドを変更して、それらの結果をキャプチャし、各リポジトリ名をコンソールに書き込むようにします。 `Main` メソッドは次のようになります。

```csharp
public static async Task Main(string[] args)
{
    var repositories = await ProcessRepositories();

    foreach (var repo in repositories)
        Console.WriteLine(repo.Name);
}
```

## <a name="reading-more-information"></a>詳細情報を確認する

最後に、GitHub API から送信される JSON パケット内のいくつかのプロパティを処理します。 すべてのプロパティを追加する必要はありませんが、いくつかのプロパティを追加することで C# 言語のさらなる機能を示すことができます。

最初に、いくつかの単純型を `Repository` クラスの定義に追加します。 そのクラスに次のプロパティを追加します。

```csharp
[JsonPropertyName("description")]
public string Description { get; set; }

[JsonPropertyName("html_url")]
public Uri GitHubHomeUrl { get; set; }

[JsonPropertyName("homepage")]
public Uri Homepage { get; set; }

[JsonPropertyName("watchers")]
public int Watchers { get; set; }
```

これらのプロパティには、(JSON パケットに格納されている) 文字列型からターゲット型への組み込みの変換があります。 <xref:System.Uri> 型はおそらく初めて使用する型でしょう。 これは URI (ここでは URL) を表します。 `Uri` 型と `int` 型では、ターゲット型に変換しないデータが JSON パケットに含まれている場合、シリアル化アクションで例外がスローされます。

プロパティの追加が完了したら、それらの要素を表示するように `Main` メソッドを更新してください。

```csharp
foreach (var repo in repositories)
{
    Console.WriteLine(repo.Name);
    Console.WriteLine(repo.Description);
    Console.WriteLine(repo.GitHubHomeUrl);
    Console.WriteLine(repo.Homepage);
    Console.WriteLine(repo.Watchers);
    Console.WriteLine();
}
```

最後の手順として、最後のプッシュ操作に関する情報を追加します。 この情報は、JSON 応答では次のように書式設定されます。

```json
2016-02-08T21:27:00Z
```

この形式は .NET の標準の <xref:System.DateTime> 形式に従っていません。 そのため、カスタムの変換メソッドを記述する必要があります。 また、`Repository` クラスのユーザーに公開されている未加工の文字列もおそらく不要でしょう。 この文字列も属性を使用して制御できます。 最初に、日時の文字列表現を `Repository` クラスに保持する `public` プロパティと、返される日付を表す書式設定された文字列を返す `LastPush` `readonly` プロパティを定義します。

```csharp
[JsonPropertyName("pushed_at")]
public string JsonDate { get; set; }

public DateTime LastPush =>
    DateTime.ParseExact(JsonDate, "yyyy-MM-ddTHH:mm:ssZ", CultureInfo.InvariantCulture);
```

定義したばかりの新しいコンストラクトについて詳しく見てみましょう。 `LastPush` プロパティは、"*式形式のメンバー*" を使用して `get` アクセサーに対して定義されます。 `set` アクセサーがない。 C# では `set` アクセサーを省略することで "*読み取り専用*" プロパティを定義します (C# では "*書き込み専用*" プロパティを作成できますが、その値は制限されています)。<xref:System.DateTime.ParseExact(System.String,System.String,System.IFormatProvider)> メソッドは、指定された日付形式を使用して文字列を解析し、<xref:System.DateTime> オブジェクトを作成します。次に、`CultureInfo` オブジェクトを使用して `DateTime` にメタデータを追加します。 解析操作が失敗した場合、プロパティのアクセサーは例外をスローします。

<xref:System.Globalization.CultureInfo.InvariantCulture> を使用するには、`repo.cs` の `using` ディレクティブに <xref:System.Globalization> 名前空間を追加する必要があります。

```csharp
using System.Globalization;
```

最後に、コンソールで出力ステートメントをもう 1 つ追加すれば、このアプリケーションを再度ビルドして実行する準備は完了です。

```csharp
Console.WriteLine(repo.LastPush);
```

以上で、作成してきたバージョンは[最終的なサンプル](https://github.com/dotnet/samples/tree/master/csharp/getting-started/console-webapiclient)と同じになるはずです。

## <a name="conclusion"></a>まとめ

このチュートリアルでは、Web 要求を作成する方法、結果を解析する方法、およびそれらの結果のプロパティを表示する方法を紹介しました。 また、依存関係として新しいパッケージをプロジェクトに追加しました。 さらに、オブジェクト指向の手法をサポートする C# 言語の一部の機能について確認しました。

<a name="dotnet-restore-note"></a>

[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]
