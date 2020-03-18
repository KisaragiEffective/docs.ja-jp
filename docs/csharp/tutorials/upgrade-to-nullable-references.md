---
title: null 許容参照型にアップグレードする
description: この高度なチュートリアルでは、null 許容参照型を使用して既存のコードを移行する方法を示します。
ms.date: 02/19/2019
ms.technology: csharp-null-safety
ms.custom: mvc
ms.openlocfilehash: 9767493059623e770cc100b83b9284e8d0bdf0f8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79156455"
---
# <a name="tutorial-migrate-existing-code-with-nullable-reference-types"></a>チュートリアル: null 許容参照型で既存のコードを移行する

C# 8 には **null 許容参照型**が導入されています。これは、null 許容値型が値型を補完するのと同じように、参照型を補完するものです。 型に `?` を追加することで、変数が **null 許容参照型**であることを宣言します。 たとえば、`string?` は、null が許容される `string` を表します。 これらの新しい型を使用して、一部の変数では*常に値を持つ必要があり*、他の変数では*値が欠落することも可能である*という設計意図をさらに明確に示すことができます。 参照型の既存の変数は、null 非許容参照型として解釈されます。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> - コードを操作するときに null 参照のチェックを有効にする。
> - null 値に関連するさまざまな警告を診断して修正する。
> - null 許容が有効なコンテキストと null 許容が無効なコンテキストの間のインターフェイスを管理する。
> - null 許容注釈のコンテキストを制御する。

## <a name="prerequisites"></a>必須コンポーネント

お使いのコンピューターを、.NET Core が実行されるように設定する必要があります。C# 8.0 コンパイラも実行されるようにします。 C# 8 コンパイラは [Visual Studio 2019 バージョン 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) または [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download) 以降で使用できます。

このチュートリアルでは、.NET と、C# と Visual Studio または .NET Core CLI のいずれかに精通していることを前提としています。

## <a name="explore-the-sample-application"></a>サンプル アプリケーションを調べる

これから移行するサンプル アプリケーションは、RSS フィード リーダー Web アプリです。 1 つの RSS フィードから読み取り、最新の記事の要約を表示します。 いずれかの記事を選択すると、そのサイトに移動できます。 そのアプリケーションは比較的新しいものですが、null 許容参照型が有効になる前に書かれました。 アプリケーションの設計上の決定では適切な原則が表されていましたが、この重要な言語機能は利用されていません。

サンプル アプリケーションには、アプリの主要な機能を検証する単体テスト ライブラリが含まれています。 生成された警告に基づいて実装のいずれかを変更する場合、そのプロジェクトによって容易に安全なアップグレードを行うことができます。 GitHub の [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/nullable-reference-migration/start) リポジトリから、スタート コードをダウンロードすることができます。

プロジェクト移行の目標は、新しい言語機能を利用して変数の null 値の許容についての意図を明確に表すこと、そしてと null 許容注釈コンテキストおよび null 許容警告コンテキストを `enabled` に設定したときにコンパイラで警告が生成されないような方法でそれを行うことです。

## <a name="upgrade-the-projects-to-c-8"></a>プロジェクトを C# 8 にアップグレードする

最初のステップとして、移行タスクの範囲を決定します。 まず最初に、プロジェクトを C# 8.0 (またはそれ以降) にアップグレードします。 Web プロジェクトと単体テスト プロジェクトの両方について、csproj ファイルの PropertyGroup に `LangVersion` 要素を追加します。

```xml
<LangVersion>8.0</LangVersion>
```

言語バージョンをアップグレードすると C# 8.0 が選択されますが、null 許容注釈コンテキストまたは null 許容警告コンテキストは有効になりません。 プロジェクトをリビルドして、警告なしにビルドされることを確認します。

次に、null 許容注釈コンテキストを有効にして、警告がいくつ生成されるかを確認します。 ソリューションの両方の csproj ファイルで、`LangVersion` 要素のすぐ下に次の要素を追加します。

```xml
<Nullable>enable</Nullable>
```

テスト ビルドを行い、警告の一覧に注目します。 この小さいアプリケーションでは 5 つのコンパイラ警告が生成されるので、null 許容注釈コンテキストを有効にしたまま、プロジェクト全体の警告の修正を始めます。

そのような戦略は、小さいプロジェクトに対してのみ機能します。 大きいプロジェクトでは、コードベース全体の null 許容注釈コンテキストを有効にすると生成される警告の数が多く、体系的に警告を修正することは困難です。 大規模なエンタープライズ プロジェクトでは、一度に 1 つのプロジェクトを移行するのが普通です。 各プロジェクトでは、一度に 1 つのクラスまたはファイルを移行します。

## <a name="warnings-help-discover-original-design-intent"></a>元の設計意図の発見に役立つ警告

複数の警告が生成されているクラスが 2 つあります。 `NewsStoryViewModel` クラスから始めます。 作業を行っているコードのセクションに警告の範囲を制限できるように、両方の csproj ファイルから `Nullable` 要素を削除します。 *NewsStoryViewModel.cs* ファイルを開き、次のディレクティブを追加して `NewsStoryViewModel` に対して null 許容注釈コンテキストを有効にし、そのクラス定義の後で元に戻します。

```csharp
#nullable enable
public class NewsStoryViewModel
{
    public DateTimeOffset Published { get; set; }
    public string Title { get; set; }
    public string Uri { get; set; }
}
#nullable restore
```

これら 2 つのディレクティブは、移行作業の対象を絞るのに役立ちます。 null 許容の警告は、今まさに作業を行っているコードの領域に対して生成されます。 プロジェクト全体の警告を有効にする準備ができるまで、それをオンのままにします。 後でプロジェクト全体の null 許容注釈を有効にしたときに、誤ってコンテキストを無効にしないように、`disable` 値ではなく `restore` を使用する必要があります。 プロジェクト全体の null 許容注釈コンテキストを有効にした後は、そのプロジェクトからすべての `#nullable` プラグマを削除できます。

`NewsStoryViewModel` クラスはデータ転送オブジェクト (DTO) であり、プロパティのうち 2 つは読み取り/書き込み文字列です。

[!code-csharp[InitialViewModel](~/samples/snippets/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/ViewModels/NewsStoryViewModel.cs#StarterViewModel)]

これら 2 つのプロパティが、`CS8618` "null 非許容型のプロパティが初期化されていません" の原因になっています。 `NewsStoryViewModel` が構築されるときに、どちらの `string` プロパティも既定値が `null` になっているので、それははっきりわかります。 重要なのは、`NewsStoryViewModel` オブジェクトの構築方法を明らかにすることです。 このクラスを調べても、`null` 値が設計の一部なのかどうか、またはこれらのオブジェクトは作成されるたび null 以外の値に設定されるのかどうかはわかりません。 ニュース記事は、`NewsService` クラスの `GetNews` メソッドで作成されます。

[!code-csharp[StarterCreateNewsItem](~/samples/snippets/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/Services/NewsService.cs#CreateNewsItem)]

上記のコード ブロックでは多くのことが行われています。 このアプリケーションでは、[AutoMapper](https://automapper.org/) NuGet パッケージを使用して、`ISyndicationItem` からニュース項目を作成しています。 ニュース記事の項目が構築され、その 1 つのステートメントでプロパティが設定されることはわかっています。 つまり、`NewsStoryViewModel` の設計では、これらのプロパティが `null` 値になってはならないことが示されています。 これらのプロパティは、**null 非許容参照型**にする必要があります。 それが、元の設計意図を最も適切に表しています。 実際、`NewsStoryViewModel` は、null 値以外で正しくインスタンス化されて "*います*"。 これにより、次の初期化コードが有効な修正になります。

```csharp
public class NewsStoryViewModel
{
    public DateTimeOffset Published { get; set; }
    public string Title { get; set; } = default!;
    public string Uri { get; set; } = default!;
}
```

`string` 型の場合は `null` である `default` を `Title` および `Uri` に割り当てても、プログラムの実行時の動作は変わりません。 `NewsStoryViewModel` はやはり null 値で構築されますが、コンパイラの警告は報告されなくなります。 **null 許容演算子** (`default` 式の後の `!` 文字) は、その前の式が null ではないことをコンパイラに指示します。 この手法は、他の変更によってコード ベースに大きな変化が強制されるときは便利なことがありますが、このアプリケーションでは、比較的簡単でもっとよい解決策があります。すべてのプロパティがコンストラクター内で設定される場合は、`NewsStoryViewModel` を変更不可の型にします。 `NewsStoryViewModel` を次のように変更します。

[!code-csharp[FinishedViewModel](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/ViewModels/NewsStoryViewModel.cs#FinishedViewModel)]

それを行った後は、プロパティを設定するのではなくコンストラクターを使用するように、AutoMapper を構成するコードを更新する必要があります。 `NewsService.cs` を開き、ファイルの最後にある次のコードを探します。

[!code-csharp[StarterAutoMapper](~/samples/snippets/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/Services/NewsService.cs#ConfigureAutoMapper)]

そのコードでは、`ISyndicationItem` オブジェクトのプロパティが `NewsStoryViewModel` プロパティにマップされています。 AutoMapper では代わりにコンストラクターを使用してマッピングを提供する必要があります。 上記のコードを次のような AutoMapper 構成に置き換えます。

[!code-csharp[FinishedViewModel](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Services/NewsService.cs#ConfigureAutoMapper)]

このクラスは小さく、注意深く調べたので、このクラスの宣言の上にある `#nullable enable` ディレクティブを有効にする必要があることに注意してください。 コンストラクターを変更すると何かが壊れる可能性があるため、続行する前に、すべてのテストを実行し、アプリケーションをテストすることをお勧めします。

最初の一連の変更では、変数を `null` に設定してはならないことが元の設計で示されていたことを検出する方法を示しました。 その手法は、**Correct by Construction (構築からの正しさ)** と呼ばれます。 構築するときに、オブジェクトとそのプロパティを `null` にできないことを宣言します。 コンパイラのフロー分析により、構築後にそれらのプロパティが `null` に設定されていないことが保証されます。 このコンストラクターは外部のコードによって呼び出され、そのコードでは **null 許容が認識されていない**ことに注意してください。 新しい構文では、実行時のチェックは提供されません。 外部のコードによって、コンパイラのフロー分析が回避される可能性があります。

クラスの構造によって、意図に対する別の手掛かりが提供されている場合があります。 *Pages* フォルダーの *Error.cshtml.cs* ファイルを開きます。 `ErrorViewModel` には、次のコードが含まれています。

[!code-csharp[StarterErrorModel](~/samples/snippets/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/Pages/Error.cshtml.cs#StartErrorModel)]

クラス宣言の前に `#nullable enable` ディレクティブを追加し、後に `#nullable restore` ディレクティブを追加します。 `RequestId` が初期化されていないことを示す警告を 1 つ受け取ります。 クラスを調べることで、場合によっては `RequestId` プロパティを null にする必要があると判断します。 `ShowRequestId` プロパティの存在は、欠落値があってもかまわないことを示します。 `null` が有効なので、`string` 型に `?` を追加して、`RequestId` プロパティは "*null 許容参照型*" であることを示します。 最終的なクラスは次の例のようになります。

[!code-csharp[FinishedErrorModel](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Pages/Error.cshtml.cs#ErrorModel)]

プロパティの使用方法を調べると、関連するページで、マークアップでレンダリングする前にプロパティが null かどうか確認されていることがわかります。 それは null 許容参照型の安全な使用方法なので、このクラスについては終了します。

## <a name="fixing-nulls-causes-change"></a>null の修正による変更

ある一連の警告を修正すると、関連するコードで新しい警告が発生することがよくあります。 `index.cshtml.cs` クラスを修正することによって、警告の動作を見てみましょう。 `index.cshtml.cs` ファイルを開いてコードを調べます。 このファイルには、索引ページに対する分離コードが含まれます。

[!code-csharp[StarterIndexModel](~/samples/snippets/csharp/tutorials/nullable-reference-migration/start/SimpleFeedReader/Pages/Index.cshtml.cs#IndexModelStart)]

`#nullable enable` ディレクティブを追加すると、2 つの警告が表示されます。 `ErrorText` プロパティも `NewsItems` プロパティも初期化されていません。 このクラスを調べた結果、どちらのプロパティも null 許容参照型にする必要があることがわかりました。どちらにもプライベートのセッターがあります。 厳密に 1 つだけが、`OnGet` メソッドで割り当てられます。 変更する前に、両方のプロパティのコンシューマーを確認します。 ページ自体では、エラーのマークアップが生成される前に、`ErrorText` で null が確認されています。 `NewsItems` コレクションは `null` に対して確認され、項目が含まれることが確認されます。 簡単に修正するには、両方のプロパティを null 許容参照型にします。 より適切な修正方法は、コレクションを null 非許容参照型にして、ニュースを取得するときに既存のコレクションに項目を追加することです。 最初の修正として、`ErrorText` の `string` 型に `?` を追加します。

[!code-csharp[UpdateErrorText](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Pages/Index.cshtml.cs#UpdateErrorText)]

`ErrorText` プロパティに対するすべてのアクセスは null チェックによって既に保護されているので、その変更は他のコードには反映されません。 次に、`NewsItems` リストを初期化し、プロパティのセッターを削除して、読み取り専用のプロパティにします。

[!code-csharp[InitializeNewsItems](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Pages/Index.cshtml.cs#InitializeNewsItems)]

それによって警告は修正されますが、エラーが発生するようになります。 `NewsItems` リストは **Correct by Construction** になっていますが、`OnGet` でリストを設定するコードを、新しい API と一致するように変更する必要があります。 割り当ての代わりに、`AddRange` を呼び出して既存のリストにニュース項目を追加します。

[!code-csharp[AddRange](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Pages/Index.cshtml.cs#AddRange)]

割り当ての代わりに `AddRange` を使用するということは、`GetNews` メソッドで `List` ではなく `IEnumerable` を返すことができることを意味します。 それによって 1 つの割り当てが保存されます。 次のコード サンプルで示すように、メソッドのシグネチャを変更し、`ToList` の呼び出しを削除します。

[!code-csharp[GetNews](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Services/NewsService.cs#GetNewsFinished)]

シグネチャを変更すると、テストの 1 つにも影響があります。 `SimpleFeedReader.Tests` プロジェクトの `Services` フォルダーの `NewsServiceTests.cs` ファイルを開きます。 `Returns_News_Stories_Given_Valid_Uri` テストに移動し、`result` 変数の型を `IEnumerable<NewsItem>` に変更します。 型を変更すると `Count` プロパティを使用できなくなるので、`Assert` の `Count` プロパティを `Any()` の呼び出しに置き換えます。

[!code-csharp[FixTests](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader.Tests/Services/NewsServiceTests.cs#FixTestSignature)]

ファイルの先頭に `using System.Linq` ステートメントを追加する必要もあります。

この一連の変更では、ジェネリック インスタンス化を含むコードを更新するときの特別な考慮事項に注目しています。 リストとリスト内の null 非許容型の要素の両方です。 どちらか一方または両方が、null 許容型である可能性があります。 以下のすべての宣言が許容されます。

- `List<NewsStoryViewModel>`: null 非許容ビュー モデルのを null 非許容リスト。
- `List<NewsStoryViewModel?>`: null 許容ビュー モデルのを null 非許容リスト。
- `List<NewsStoryViewModel>?`: null 非許容ビュー モデルのを null 許容リスト。
- `List<NewsStoryViewModel?>?`: null 許容ビュー モデルのを null 許容リスト。

## <a name="interfaces-with-external-code"></a>外部コードとのインターフェイス

`NewsService` クラスを変更したので、そのクラスの `#nullable enable` 注釈を有効にします。 これにより、新しい警告が生成されることはありません。 ただし、クラスを注意深く調べることは、コンパイラのフロー分析に含まれるいくつかの制限を明らかにするのに役立ちます。 コンストラクターを調べます。

[!code-csharp[ServiceConstructor](~/samples/snippets/csharp/tutorials/nullable-reference-migration/finished/SimpleFeedReader/Services/NewsService.cs#ServiceConstructor)]

`IMapper` パラメーターは、null 非許容参照として型指定されています。 それは ASP.NET Core インフラストラクチャ コードで呼び出されるため、`IMapper` が null にはならないということは、コンパイラには実際にはわかりません。 ASP.NET Core の既定の依存関係挿入 (DI) コンテナーでは、コードが正しくなるよう、必要なサービスを解決できない場合は、例外がスローされます。 null 許容注釈コンテキストを有効にしてコードをコンパイルした場合でも、コンパイラではパブリック API のすべての呼び出しを検証することはできません。 さらに、null 許容参照型がまだ使用されていないプロジェクトで、ライブラリが使用される可能性があります。 null 非許容型として宣言されている場合でも、パブリック API への入力を検証してください。

## <a name="get-the-code"></a>コードを取得する

最初のテスト コンパイルで明らかになった警告を修正したので、今度は両方のプロジェクトで null 許容注釈コンテキストを有効にできます。 プロジェクトをリビルドします。コンパイラで警告は報告されません。 [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/nullable-reference-migration/finished) GitHub リポジトリで、完成したプロジェクトのコードを入手できます。

null 許容参照型をサポートする新しい機能を使用すると、コードでの `null` 値の処理方法に関する潜在的なエラーを発見して修正するのに役立ちます。 null 許容注釈コンテキストを有効にすると、null にできない変数と、null 値を含むことができる変数に関する、設計の意図を表すことができます。 これらの機能を使用すると、設計意図を簡単に宣言できるようになります。 同様に、null 許容警告コンテキストでは、その意図に違反しているときに警告を発行するようコンパイラに指示されます。 それらの警告を参考にすると、より回復性が高く、実行中に `NullReferenceException` がスローされる可能性が低くなるように、コードを更新できます。 コードの局所的な領域に注目して移行を行うことができ、それ以外のコードベースは変更されないように、これらのコンテキストの範囲を制御できます。 実際には、この移行タスクを、クラスに対する定期的なメンテナンスの一部にすることができます。 このチュートリアルでは、null 許容参照型を使用するようにアプリケーションを移行するプロセスを示しました。 このプロセスのさらに大きな実際の例については、[Jon Skeet](https://github.com/jskeet) が [NodaTime](https://github.com/nodatime/nodatime/pull/1240/commits) に null 許容参照型を組み込むために行った PR を調べてください。 また、[Entity Framework Core の null 許容参照型の使用](/ef/core/miscellaneous/nullable-reference-types)に関するページで、Entity Framework Core で null 許容参照型を使用する手法を確認することもできます。
