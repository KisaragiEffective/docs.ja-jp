---
title: ASP.NET Core MVC アプリのテスト
description: ASP.NET Core および Azure での最新の Web アプリケーションの設計 | ASP.NET Core MVC アプリのテスト
author: ardalis
ms.author: wiwagn
ms.date: 12/04/2019
ms.openlocfilehash: 2b347442c4a9b7b6cf912ec461248f901dc45417
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79147492"
---
# <a name="test-aspnet-core-mvc-apps"></a>ASP.NET Core MVC アプリのテスト

> *"あなたが製品の単体テストを好まないと、あなたの顧客もテストを望まないでしょう。"*
 > \_- 匿名 -

変更に対応するとき、複雑なソフトウェアには予想外のエラーが発生することがあります。 そのため、ほとんどの些細な (少なくとも重要性が最も低い) アプリケーションを除くすべてのアプリケーションで変更後のテストが必須となります。 手動テストはソフトウェアのテスト方法として最も遅く、信頼性がなく、高額です。 残念ながら、アプリケーションにテスト機能が付いていない場合、手動テストが唯一の手段になることがあります。 [第 4 章](architectural-principles.md)に記載されているアーキテクチャの原則に従って作成されたアプリケーションは、単体テストが可能になります。 ASP.NET Core アプリケーションでは、統合テストと機能テストの自動化がサポートされています。

## <a name="kinds-of-automated-tests"></a>自動テストの種類

ソフトウェア アプリケーションでは、さまざまな種類のテストが自動化されています。 最も単純でレベルの低いテストが単体テストです。 少し高いレベルに、統合テストや機能テストがあります。 他の種類のテスト (UI テスト、ロード テスト、ストレス テスト、スモーク テストなど) は、このドキュメントでは扱いません。

### <a name="unit-tests"></a>単体テスト

単体テストでは、アプリケーションのロジックの 1 つの部分をテストします。 単体テストに含まれない内容を列挙するして単体テストをさらに説明できます。 単体テストでは、依存関係やインフラストラクチャとのコードの連動をテストしません。それは統合テストの対象です。 単体テストでは、コード記述の基礎となるフレームワークをテストしません。フレームワークは動作すると想定してください。動作しないことが判明した場合、バグを提出するか、回避策となるコードを記述してください。 単体テストは、メモリとプロセスの中で完全に実行されます。 ファイル システム、ネットワーク、データベースとは通信しません。 単体テストはコードのみをテストします。

単体テストはコードの 1 単位のみをテストし、外部の依存関係が関与しないため、極めて短時間で実行されます。 そのため、何百という単体テストからなるテスト スイートを数秒で実行できます。 単体テストは頻繁に実行してください。共有ソース管理リポジトリにプッシュするたびに実行するのが理想です。ビルド サーバーで自動化ビルドを実行したときには、もちろん毎回実行してください。

### <a name="integration-tests"></a>統合テスト

データベースやファイル システムなど、インフラストラクチャとやり取りするコードをカプセル化することは良い考えですが、それでもカプセル化されないコードが残るので、それをテストすることになります。 また、アプリケーションの依存関係が完全に解決されるとき、コードの層が予想どおりにやり取りすることを検証してください。 これは統合テストの責務です。 統合テストには、単体テストより時間がかかり、設定が難しくなる傾向があります。外部の依存関係やインフラストラクチャに依存することが多いためです。 そのため、統合テストでは、単体テストでテストできるようなことはテストしないでください。 所与のシナリオを単体テストでテストできる場合、単体テストでテストしてください。 単体テストでテストできない場合、統合テストの利用を検討してください。

統合テストは多くの場合、セットアップと破棄のプロシージャが単体テストより複雑です。 たとえば、実際のデータベースに対して統合テストを行うとき、データベースをテスト実行前の既知の状態に戻す方法が必要になります。 新しいテストが追加され、運用データベース スキーマが拡大するにつれ、テスト スクリプトのサイズが増加し、より複雑になります。 大規模なシステムの多くでは、共有ソース管理の変更を調べる前に、開発者ワークステーションで完全な統合テスト スイートを実行することは実用的ではありません。 そのような場合、ビルド サーバーで統合テストを実行できることがあります。

### <a name="functional-tests"></a>機能テスト

システムの一部のコンポーネントが正しく連動することを確認する目的で、統合テストは開発者の視点から記述されます。 機能テストはユーザーの視点から記述され、その要件に基づき、システムの正確性を検証します。 機能テストとは何か、単体テストとの比較で考えるとき、次の抜粋が類推として役に立ちます。

> "多くの場合、システムの開発は家の建築に例えられます。 この類推はまったく正しいというわけではありませんが、単体テストと機能テストの違いを理解する目的で拡大解釈できます。 単体テストは、建築調査官が家の建築現場を訪問する行為に似ています。 調査官は家のさまざまな内部システム、土台、骨組み、電気、配管を重点的に調べ、 家の各部分が正しく安全に機能すること、つまり、建築法規に準拠していることを確認 (テスト) します。 このシナリオにおける機能テストは、家主がこの同じ建築現場を訪問する行為に似ています。 家主は、内部システムが適切に動作し、建築調査官がその仕事を遂行しているものと想定し、 その家で生活することはどのような感じになるのかを重点的に確認します。 家はどのように見えるか、各部屋の大きさはちょうど良いか、家は家族の希望に合っているか、朝日を取り入れる場所に窓が取り付けられているかが重要となります。 家主は家に機能テストを実行します。 家主の視点はユーザーの視点です。 建築調査官は家に単体テストを実行します。 調査官の視点は開発者の視点です。"

出典: [Unit Testing versus Functional Tests](https://www.softwaretestingtricks.com/2007/01/unit-testing-versus-functional-tests.html) (単体テストと機能テストの比較)

"開発者は 2 通りの失敗をします。間違った方法で開発することと間違ったものを開発することです。" というのは私の好きな表現です。 単体テストでは、正しい方法で開発していることを確認します。機能テストでは、正しいものを開発していることを確認します。

機能テストはシステム レベルで動作するため、ある程度の UI 自動化が必要になることがあります。 統合テストと同様に、通常、ある種のテスト インフラストラクチャとも連動します。 そのため単体テストや統合テストより遅くなり、不安定になります。 機能テストは、システムがユーザーの期待どおりに動作すると確信できるために必要な数だけ行ってください。

### <a name="testing-pyramid"></a>テストのピラミッド

Martin Fowler がテストをピラミッド図にしました。図 9-1 がその例です。

![テストのピラミッド](./media/image9-1.png)

**図 9-1** テストのピラミッド

ピラミッドの各層はテストの種類を表し、その相対的な大きさはアプリケーションのために記述すべきテストの数を表します。 ご覧のように、単体テストの土台を大きくし、それより小さい統合テスト層が続き、さらに小さい機能テスト層が続くという構成が推奨されています。 各層には、理想的には、それより下の層では適切に実行できないテストのみを含めます。 特定のシナリオで必要とするテストの種類を決定するとき、このピラミッドを念頭に置いてください。

### <a name="what-to-test"></a>テストの内容

自動化テストの記述経験がない開発者にとっては、何をテストするのかが共通の問題です。 理想的な出発点は条件ロジックをテストすることです。 条件付きステートメント (if-else、switch など) に基づいて動作が変わるメソッドを使用している場所では、特定の条件に対する正しい動作を確認するためのテストを、少なくとも 2、3 個思い付くはずです。 コードの条件に間違いがある場合、(エラーのない) コード経由で "Happy Path" のテストを少なくとも 1 つ記述し、(エラーがあるか、結果が不規則な) "Sad Path" のテストを少なくとも 1 つ記述し、エラーに直面したときにアプリケーションが予想どおりに動作することを確認することをお勧めします。 最後に、コード カバレッジなどの指標ではなく、エラーが起こりうるものに対するテストに集中的に取り組みます。 一般的に、カバレッジは少ないよりも多い方が良いとされます。 ただし、複雑でビジネスに不可欠なメソッド用にさらにいくつかのテストを記述することは、通常、テストのコード カバレッジ メトリックを改善するためだけに自動プロパティのテストを記述するよりも優れた時間の使い方です。

## <a name="organizing-test-projects"></a>テスト プロジェクトを整理する

テスト プロジェクトは、自分にとって最適に機能するように整理できます。 種類 (単体テストや統合テスト) やテスト内容 (プロジェクトや名前空間) に基づいてテストを分類することをお勧めします。 その分類が 1 つのテスト プロジェクト内のフォルダーで構成されるのか、複数のテスト プロジェクト内のフォルダーで構成されるのかは設計上の決定事項の 1 つです。 プロジェクトが 1 つであれば最も単純に整理されますが、大型のプロジェクトでさまざまなテストが含まれる場合、あるいはさまざまなテスト セットをより簡単に実行するには、複数のテスト プロジェクトが必要になることもあります。 多くのチームは、自分たちがテストしているプロジェクトに基づいてプロジェクトを整理します。アプリケーションに相当な数のプロジェクトが含まれるとき、テスト プロジェクトが大量になります。各プロジェクトに含まれるテストの種類に基づいてさらに細分化される場合は特に大量になります。 妥協案としては、テストの種類あたり、アプリケーションあたりプロジェクトを 1 つとし、テスト プロジェクトの中にフォルダーを置き、テスト対象のプロジェクト (とクラス) を示します。

一般的な方法は、‘src' フォルダーの下でアプリケーション プロジェクトを整理し、並列する ‘tests' フォルダーの下でアプリケーションのテスト プロジェクトを整理することです。 この整理方法が有効な場合、Visual Studio でこれに合ったソリューションを作成できます。

![ソリューション内のテストの整理](./media/image9-2.png)

**図 9-2** ソリューション内のテストの整理

好きな方のテスト フレームワークを利用できます。 xUnit フレームワークは良好に動作し、ASP.NET Core と EF Core テストはすべてこれで記述されています。 図 9-3 のテンプレートを使用するか、CLI から `dotnet new xunit` を使用して、Visual Studio で xUnit テスト プロジェクトを追加できます。

![Visual Studio で xUnit テスト プロジェクトを追加する](./media/image9-3.png)

**図 9-3** Visual Studio で xUnit テスト プロジェクトを追加する

### <a name="test-naming"></a>テストの命名規則

テストには一貫性のある名前を付けてください。各テストの内容を示す名前にします。 テストするクラスやメソッドに基づいてテスト クラスに名前を付けるという方法でうまく行ったことがあります。 結果的に小さなテスト クラスがたくさん作られますが、それぞれのテストが担当する内容は極めて明白になります。 テストするクラスやメソッドを識別するためにテスト クラスの名前を設定したら、テスト メソッド名を利用し、テストする動作を指定できます。 それにより、求められる動作と、その動作を生むための入力や前提が含まれます。 テスト名の例:

- `CatalogControllerGetImage.CallsImageServiceWithId`

- `CatalogControllerGetImage.LogsWarningGivenImageMissingException`

- `CatalogControllerGetImage.ReturnsFileResultWithBytesGivenSuccess`

- `CatalogControllerGetImage.ReturnsNotFoundResultGivenImageMissingException`

この方法のバリエーションとしては、それぞれのテスト クラスの名前の末尾を "Should" にして時制を少し変えます。

- `CatalogControllerGetImage`**Should**`.`**Call**`ImageServiceWithId`

- `CatalogControllerGetImage`**Should**`.`**Log**`WarningGivenImageMissingException`

少しばかり冗長ですが、2 つ目の命名規則の方がわかりやすいと感じるチームもあるでしょう。 いずれにせよ、テストの動作がわかる命名規則を利用してください。テストが失敗したとき、何が失敗したのか名前から判断できます。 ControllerTests.Test1 のような曖昧な名前をテストに付けないでください。テスト結果に表示されたとき、何の有用性もありません。

小さなテスト クラスをたくさん生成する上記のような命名規則に従う場合、フォルダーや名前空間を利用し、テストをさらに整理することをお勧めします。 図 9-4 では、テスト プロジェクト内のフォルダー別にテストを整理している手法を確認できます。

![テストされるクラスに基づき、テスト クラスをフォルダーで分けて整理する](./media/image9-4.png)

**図 9-4** テストされるクラスに基づき、テスト クラスをフォルダーで分けて整理します。

特定のアプリケーション クラスにテスト対象メソッドが多数ある (したがってテスト クラスも多数ある) 場合は、そのアプリケーション クラスに対応するフォルダーにそれらを配置することをお勧めします。 その整理方法では、ファイルをどこか他の場所のフォルダーに入れて整理する場合と変わりません。 1 つのフォルダーの中に関連ファイルが 3 つもしくは 4 つ以上存在するとき、それぞれの下位フォルダーに移動させると便利なことがあります。

## <a name="unit-testing-aspnet-core-apps"></a>ASP.NET Core アプリを単体テストする

うまく設計された ASP.NET Core アプリケーションでは、ビジネス エンティティやさまざまなサービスにほとんどの複雑性やビジネス ロジックがカプセル化されます。 ASP.NET Core MVC アプリ自体とそのコントローラー、ビューモデル、ビューには単体テストはほとんど必要ありません。 アクションの機能性の多くは、アクション メソッド自体の外にあります。 ルーティングが正しく機能するかどうかのテストやグローバル エラーの処理は、単体テストで効果的に行うことができません。 同様に、モデル検証、認証、許可のフィルターを含むすべてのフィルターも、コントローラーのアクション メソッドをターゲットとするテストによって単体テストを行うことはできません。 動作の源がなければ、ほとんどのアクション メソッドは取るに足りないほど小さくなります。動作の源を利用するコントローラーに関係なくテストできるサービスに作業の大部分が委任されます。

場合によっては、単体テストする目的で、コードを改良する必要があります。 一般的には、インフラストラクチャに対して直接コード化せず、抽象化を特定し、依存関係挿入を利用し、テストするコードの抽象化にアクセスします。 たとえば、次の例をご覧ください。これは画像を表示する単純なアクション メソッドです。

```csharp
[HttpGet("[controller]/pic/{id}")]
public IActionResult GetImage(int id)
{
    var contentRoot = _env.ContentRootPath + "//Pics";
    var path = Path.Combine(contentRoot, id + ".png");
    Byte[] b = System.IO.File.ReadAllBytes(path);
    return File(b, "image/png");
}
```

このメソッドの単体テストは、ファイル システムからの読み取りに使われている `System.IO.File` に直接依存することで難しくなります。 この動作をテストし、予想どおり動くことを確認できますが、実際のファイルでそれをするのは統合テストです。 このメソッドのルートを単体テストできない点に注目してください。この後すぐ、機能テストでこれを行う方法について説明します。

ファイル システムの動作の単体テストが直接実行できないために、ルートをテストできない場合、どのようなテストが残されているのでしょうか? 改良して単体テストを可能にすると、テスト ケースや、エラー処理など、足りない動作が見つかることがあります。 ファイルが見つからないとき、メトリックは何を行うのでしょうか? 何をすべきでしょうか? この例では、改良後のメソッドは次のようになります。

```csharp
[HttpGet("[controller]/pic/{id}")]
public IActionResult GetImage(int id)
{
    byte[] imageBytes;
    try
    {
        imageBytes = _imageService.GetImageBytesById(id);
    }
    catch (CatalogImageMissingException ex)
    {
        _logger.LogWarning($"No image found for id: {id}");
        return NotFound();
    }
    return File(imageBytes, "image/png");
}
```

`_logger` と `_imageService` は、両方とも依存関係として注入されます。 これで、アクション メソッドに渡されるのと同じ ID が `_imageService` に渡されることと、生成されるバイトが FileResult の一部として返されることをテストできるようになりました。 また、エラー ログが予想どおり行われることと、画像が見つからない場合に `NotFound` 結果が返されることもテストできます (これが重要なアプリケーション動作である (つまり、問題を診断するために開発者が追加した一時的なコードではない) ことを前提とします)。 実際のファイル ロジックは別個の実装サービスに移動しており、ファイルが足りない場合にアプリケーション固有の例外を返すように拡大されています。 統合テストを利用して、この実装を非依存でテストできます。

ほとんどの場合、コントローラーにグローバルの例外ハンドラーを使用します。それにより、コントローラーのロジック量が最小限に抑えられ、単体テストの必要がなくなります。 コントローラー アクションのほとんどは機能テストと下記の `TestServer` クラスでテストしてください。

## <a name="integration-testing-aspnet-core-apps"></a>ASP.NET Core Apps を統合テストする

ASP.NET Core アプリのほとんどの統合テストで、インフラストラクチャ プロジェクトに定義されているサービスとその他の種類の実装をテストします。 たとえば、Infrastructure プロジェクト内に存在するデータ アクセス クラスから [EF Core が期待されるデータを正常に更新および取得したことをテストする](https://docs.microsoft.com/ef/core/miscellaneous/testing/)ことができます。 ASP.NET Core MVC プロジェクトが正しく動作していることをテストする最良の方法は、テスト ホストで実行されているアプリに対して機能テストを実行することです。

## <a name="functional-testing-aspnet-core-apps"></a>ASP.NET Core アプリを機能テストする

ASP.NET Core アプリケーションの場合、`TestServer` クラスを利用すると、機能テストをとても簡単に記述できます。 `TestServer` を `WebHostBuilder` (または `HostBuilder`) を使用するように直接構成する (アプリケーションに対して通常実行するのと同じ)、または `WebApplicationFactory` 型 (バージョン 2.1 以降で使用可能) で構成します。 テスト ホストと運用ホストをできる限り一致させるようにする必要があるため、テストではアプリが運用環境で実行する内容と同様の動作が実行されます。 `WebApplicationFactory` クラスは、ビューなどの静的リソースを検索するために ASP.NET Core によって使用される、TestServer の ContentRoot を構成するときに便利です。

IClassFixture\<WebApplicationFactory\<TEntry>> を実装するテスト クラスを作成することによって、シンプルな機能テストを作成できます。ここで、TEntry はご利用の Web アプリケーションの Startup クラスです。 これを配置すると、ファクトリの CreateClient メソッドを使用して、テスト フィクスチャでクライアントを作成できます。

```cs
public class BasicWebTests : IClassFixture<WebApplicationFactory<Startup>>
{
    protected readonly HttpClient _client;

    public BaseWebTest(WebApplicationFactory<Startup> factory)
    {
        _client = factory.CreateClient();
    }

    // write tests that use _client
}
```

アプリケーションをメモリ データ ストアで使用するように構成し、テスト データを使ってアプリケーションをシードするなど、各テストを実行する前に、ご利用のサイトの追加の構成を実行する必要があることがよくあります。 これを行うには、独自の WebApplicationFactory\<TEntry> のサブクラスを作成して、その ConfigureWebHost メソッドをオーバーライドする必要があります。 以下の例は、eShopOnWeb FunctionalTests プロジェクトからのもので、メインの Web アプリケーション上でのテストの一部として使用されます。

```cs
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Mvc.Testing;
using Microsoft.EntityFrameworkCore;
using Microsoft.eShopWeb.Infrastructure.Data;
using Microsoft.eShopWeb.Infrastructure.Identity;
using Microsoft.eShopWeb.Web;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using System;

namespace Microsoft.eShopWeb.FunctionalTests.Web
{
    public class WebTestFixture : WebApplicationFactory<Startup>
    {
        protected override void ConfigureWebHost(IWebHostBuilder builder)
        {
            builder.UseEnvironment("Testing");

            builder.ConfigureServices(services =>
            {
                 services.AddEntityFrameworkInMemoryDatabase();

                // Create a new service provider.
                var provider = services
                    .AddEntityFrameworkInMemoryDatabase()
                    .BuildServiceProvider();

                // Add a database context (ApplicationDbContext) using an in-memory
                // database for testing.
                services.AddDbContext<CatalogContext>(options =>
                {
                    options.UseInMemoryDatabase("InMemoryDbForTesting");
                    options.UseInternalServiceProvider(provider);
                });

                services.AddDbContext<AppIdentityDbContext>(options =>
                {
                    options.UseInMemoryDatabase("Identity");
                    options.UseInternalServiceProvider(provider);
                });

                // Build the service provider.
                var sp = services.BuildServiceProvider();

                // Create a scope to obtain a reference to the database
                // context (ApplicationDbContext).
                using (var scope = sp.CreateScope())
                {
                    var scopedServices = scope.ServiceProvider;
                    var db = scopedServices.GetRequiredService<CatalogContext>();
                    var loggerFactory = scopedServices.GetRequiredService<ILoggerFactory>();

                    var logger = scopedServices
                        .GetRequiredService<ILogger<WebTestFixture>>();

                    // Ensure the database is created.
                    db.Database.EnsureCreated();

                    try
                    {
                        // Seed the database with test data.
                        CatalogContextSeed.SeedAsync(db, loggerFactory).Wait();

                        // seed sample user data
                        var userManager = scopedServices.GetRequiredService<UserManager<ApplicationUser>>();
                        var roleManager = scopedServices.GetRequiredService<RoleManager<IdentityRole>>();
                        AppIdentityDbContextSeed.SeedAsync(userManager, roleManager).Wait();
                    }
                    catch (Exception ex)
                    {
                        logger.LogError(ex, $"An error occurred seeding the " +
                            "database with test messages. Error: {ex.Message}");
                    }
                }
            });
        }
    }
}
```

テストでは、クライアントを作成するために使用し、このクライアント インスタンスを使用してアプリケーションに要求を出すことによって、このカスタム WebApplicationFactory を活用できます。 アプリケーションには、テストのアサーションの一部として使用できる、シードされたデータがあります。 次のテストは、eShopOnWeb アプリケーションのホーム ページが正しく読み込まれることを検証し、シード データの一部としてアプリケーションに追加された製品の一覧が含まれます。

```cs
using Microsoft.eShopWeb.FunctionalTests.Web;
using System.Net.Http;
using System.Threading.Tasks;
using Xunit;

namespace Microsoft.eShopWeb.FunctionalTests.WebRazorPages
{
    [Collection("Sequential")]
    public class HomePageOnGet : IClassFixture<WebTestFixture>
    {
        public HomePageOnGet(WebTestFixture factory)
        {
            Client = factory.CreateClient();
        }

        public HttpClient Client { get; }

        [Fact]
        public async Task ReturnsHomePageWithProductListing()
        {
            // Arrange & Act
            var response = await Client.GetAsync("/");
            response.EnsureSuccessStatusCode();
            var stringResponse = await response.Content.ReadAsStringAsync();

            // Assert
            Assert.Contains(".NET Bot Black Sweatshirt", stringResponse);
        }
    }
}
```

この機能テストは、配置されているあらゆるミドルウェア、フィルター、バインダーなど、完全な ASP.NET Core MVC / Razor Pages アプリケーション スタックを行使します。 特定のルート ("/") が想定される正常な状態コードと HTML 出力を返すことを検証します。 これは実際の Web サーバーを設定せずに行い、実際の Web サーバーを利用して不安定になることを回避します (ファイアウォール設定の問題など)。 TestServer に対して実行される機能テストは通常、統合テストや単体テストより遅くなりますが、テスト Web サーバーのネットワークで実行されるテストよりはるかに速くなります。 アプリケーションのフロント エンドのスタックが想定どおりに確実に動作するようにするため、機能テストを使用する必要があります。 これらのテストは、コントローラーやページに重複があり、フィルターを追加して重複に対処するときに特に便利です。 このリファクタリングではアプリケーションの動作を変更せずに、機能テストのスイートによってこの状況を検証することをお勧めします。

> ### <a name="references--test-aspnet-core-mvc-apps"></a>リファレンス – ASP.NET Core MVC アプリのテスト
>
> - **ASP.NET Core でのテスト** \
>   <https://docs.microsoft.com/aspnet/core/testing/>
> - **単体テストの名前付け規則** \
>   <https://ardalis.com/unit-test-naming-convention>
> - **EF Core のテスト** \
>   <https://docs.microsoft.com/ef/core/miscellaneous/testing/>
> - **ASP.NET Core での統合テスト** \
>   <https://docs.microsoft.com/aspnet/core/test/integration-tests>

>[!div class="step-by-step"]
>[前へ](work-with-data-in-asp-net-core-apps.md)
>[次へ](development-process-for-azure.md)
