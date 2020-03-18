---
title: ASP.NET Core MVC アプリの開発
description: ASP.NET Core および Azure での最新の Web アプリケーションの設計 | ASP.NET Core MVC アプリの開発
author: ardalis
ms.author: wiwagn
ms.date: 12/04/2019
ms.openlocfilehash: a18b4dfc60c7d3971136f73f333b7225735710b3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77503951"
---
# <a name="develop-aspnet-core-mvc-apps"></a>ASP.NET Core MVC アプリを開発する

> "最初から正しく理解する必要はありません。 しかし、最後に正しく理解していることは極めて重要です。"  
> _- Andrew Hunt、David Thomas_

ASP.NET Core は、最新のクラウド向けに最適化された Web アプリケーションを構築するための、クロス プラットフォームのオープン ソース フレームワークです。 ASP.NET Core アプリは、軽量なモジュール形式であり、依存関係の挿入の組み込みサポートを備え、テストと保守の容易性を大幅に向上させることができます。 ビュー ベースのアプリだけでなく最新の Web API の構築をサポートする MVC と組み合わせて、ASP.NET Core は、エンタープライズ Web アプリケーション構築のための強力なフレームワークになります。

## <a name="mvc-and-razor-pages"></a>MVC と Razor Pages

ASP.NET Core MVC は、Web ベースの API やアプリを構築する際に便利な機能をたくさん備えています。 MVC という用語は "Model-View-Controller" の略です。これは、ユーザーからの要求に応答する責任をいくつかの部分に分割する UI パターンです。 このパターンに従うだけでなく、ASP.NET Core アプリに各種機能を Razor Pages として実装することもできます。 Razor Pages は ASP.NET Core MVC に組み込まれ、経路指定やモデル バインドなどと同じ機能が使用されます。しかしながら、コントローラーやビューなどに個別のフォルダーやファイルを用意したり、属性基準で経路を指定したりする代わりに、Razor Pages は 1 つのフォルダー ("/Pages") に置かれ、このフォルダーの相対的な位置に基づいて経路を指定し、コントローラー アクションの代わりにハンドラーで要求を処理します。

新しい ASP.NET Core App を作成するとき、構築するアプリの書類に関して計画を立ててください。 Visual Studio では、いくつかのテンプレートの中から選択します。 プロジェクト テンプレートとして最も一般的な 3 つは、Web API、Web アプリケーション、Web アプリケーション (Model-View-Controller) です。 これを決定できるのは最初にプロジェクトを作成するときだけですが、取り消し不可能な決定ではありません。 Web API プロジェクトでは、標準の Model-View-Controller コントローラーが使用されます。既定ではビューだけがありません。 同様に、既定の Web アプリケーション テンプレートでは Razor Pages が使用され、Views フォルダーがありません。 このようなプロジェクトには Views フォルダーを後で追加し、ビューを基盤とする動作に対応できます。 Web API プロジェクトと Model-View-Controller プロジェクトには既定で Pages フォルダーがありませんが、後で追加し、Razor Pages を基盤とする動作に対応できます。 以上の 3 つのテンプレートは、データ (Web API)、ページ ベース、ビュー ベースという 3 つの異なるデフォルト ユーザー インタラクションをサポートするものであると考えることができます。 ただし、必要であれば、3 つのいずれかまたは全部を 1 つのプロジェクトに混在させることができます。

### <a name="why-razor-pages"></a>Razor Pages とは

Razor Pages は、Visual Studio における新しい Web アプリケーションの既定の手法です。 Razor Pages では、非 SPA フォームなど、ページ ベースのアプリケーション機能を一層簡単に構築できます。 コントローラーやビューを使用し、さまざまな依存関係やビュー モデルと連動し、さまざまなビューを返す大掛かりなコントローラーをアプリケーションに与えることが一般的でした。 これにより、複雑さが増加しました。また、単一責任の原則または開放/閉鎖原則に効果的に従わないコントローラーが生じることがよくありました。 Razor Pages では、その Razor マークアップを使用し、Web アプリケーションで特定の論理 "ページ" に対してサーバー側ロジックをカプセル化することでこの問題に対処しています。 サーバー側ロジックのない Razor ページは単純に 1 つの Razor ファイル ("Index.cshtml" など) から構成されます。 ただし、重要な Razor Pages にはほとんどの場合、ページ モデル クラスが関連付けられます。これには慣例として Razor ファイルと同じ名前と ".cs" 拡張子が付けられます。たとえば、"Index.cshtml.cs" のようになります。

Razor ページのページ モデルでは、MVC コントローラーとビューモデルの責任が組み合わされます。 コントローラー アクションのメソッドで要求を処理する代わりに、"OnGet()" のようなページ モデル ハンドラーが実行され、関連付けられているページが既定でレンダリングされます。 Razor Pages では、ASP.NET Core アプリで個々のページを構築するプロセスが簡単になります。それでありながら、ASP.NET Core MVC のアーキテクチャ機能をすべて備えています。 新しいページ ベース機能の既定の選択肢として最適です。

### <a name="when-to-use-mvc"></a>MVC を使用する場面

Web API を構築する場合、Razor Pages を使用してみるより、MVC パターンの方が合理的です。 ご自分のプロジェクトで Web API エンドポイントだけを公開する場合は、Web API のプロジェクト テンプレートから始めるのが理想的です。 それ以外の場合は、任意の ASP.NET Core アプリに、コントローラーと関連付けらた API エンドポイントを簡単に追加できます。 バージョン 5 以前の ASP.NET MVC から ASP.NET Core MVC に既存のアプリケーションを移行する場合、その労力を最小限に抑えるには、ビュー ベースの MVC 手法を使用します。 最初の移行後、新しい機能のために、さらには大規模な移行として Razor Pages を採用することが合理的かどうかを判断できます。

Web アプリの構築方法として Razor Pages を選択した場合でも MVC ビューを選択した場合でも、アプリのパフォーマンスは同じようなものになり、依存関係の注入、フィルター、モデル バインド、妥当性確認などのサポートが含まれます。

## <a name="mapping-requests-to-responses"></a>応答と要求のマッピング

ASP.NET Core アプリの中心となる機能は、受信した要求を送信する応答にマップすることです。 下位のレベルではこれはミドルウェアによって行われ、カスタム ミドルウェアのみで簡単な ASP.NET Core アプリとマイクロサービスを構成することができます。 ASP.NET Core MVC を使うと、"_ルート_"、"_コントローラー_"、"_アクション_" など、さらに上位のレベルで操作することができます。 着信した各要求はアプリケーションのルーティング テーブルと比較され、一致するルートが見つかった場合、関連付けられている (コントローラーに属する) アクション メソッドが呼び出されて要求を処理します。 一致するルートが見つからない場合は、エラー ハンドラーが呼び出されます (この場合、NotFound の結果を返します)。

ASP.NET Core MVC アプリは、規則ルートと属性ルートのどちらか一方または両方を使用できます。 規則ルートはコードで定義されており、次の例のような構文を使ってルーティングの "_規則_" を指定します。

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(name: "default", pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

この例では、"default" という名前のルートがルーティング テーブルに追加されます。 _controller_、_action_、_id_ のプレースホルダーでルート テンプレートを定義します。controller と action プレースホルダーには既定値が指定されており (それぞれ、"Home" と "Index")、id プレースホルダーは ("?" が適用されているので) 省略可能です。 ここで定義されている規則は、要求の最初の部分はコントローラーの名前に対応する必要があり、2 番目の部分はアクションに対応する必要があり、3 番目の部分は必要に応じて id パラメーターを表す、というものです。 規則ルートは通常、Startup クラスの Configure メソッドなど、アプリケーションに対して 1 か所で定義されます。

属性ルートは、グローバルに指定されるのではなく、コントローラーとアクションに直接適用されます。 この方法の場合、特定のメソッドを探しているときに検出しやすいという利点がありますが、ルーティング情報がアプリケーション内の 1 か所に保持されないことを意味します。 属性ルートでは、特定のアクションに対する複数ルートや、コントローラーとアクションの組み合わせルートを、簡単に指定できます。 次に例を示します。

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")] // Combines to define the route template "Home"
    [Route("Index")] // Combines to define route template "Home/Index"
    [Route("/")] // Does not combine, defines the route template ""
    public IActionResult Index() {}
}
```

ルートは [HttpGet] や同様の属性で指定することができ、[Route] 属性を別途追加する必要はありません。 次に示すように、属性ルートではトークンを使って、コントローラーやアクションの名前を繰り返し指定する必要性を軽減することもできます。

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
    [Route("")] // Matches 'Products'
    [Route("Index")] // Matches 'Products/Index'
    public IActionResult Index() {}
}
```

Razor Pages では、属性経路指定は使用されません。 Razor Pages には、その `@page` ディレクティブの一部として追加の経路テンプレート情報を指定できます。

```csharp
@page "{id:int}"
```

前の例の問題のページではルートと整数 `id` パラメーターが一致します。 たとえば、`/Pages` のルートに置かれている *Products.cshtml* ページにはこの経路が与えられます。

```csharp
"/Products/123"
```

特定の要求がルートと一致した後、アクション メソッドが呼び出される前に、ASP.NET Core MVC は要求に対して[モデル バインド](/aspnet/core/mvc/models/model-binding)と[モデル検証](/aspnet/core/mvc/models/validation)を実行します。 モデル バインドでは、着信した HTTP データが、呼び出されるアクション メソッドのパラメーターとして指定されている .NET 型に変換されます。 たとえば、アクション メソッドが int 型の ID パラメーターを必要としている場合、モデル バインドは要求の一部として指定された値からこのパラメーターを提供しようとします。 そのために、モデル バインドは、ポストされたフォーム内、ルート自体、クエリ文字列で値を検索します。 ID 値が見つかった場合は、整数に変換されてからアクション メソッドに渡されます。

モデルをバインドした後、アクション メソッドを呼び出す前に、モデルの検証が行われます。 モデルの検証では、モデルの種類に対するオプション属性が使われ、指定されたモデル オブジェクトが特定のデータ要件に準拠していることを確認できます。 特定の値を必須として指定したり、特定の長さや数値範囲に制限したりすることができます。検証属性が指定されていて、モデルがその要件に準拠していない場合は、ModelState.IsValid プロパティが false に設定され、準拠していない検証規則のセットを要求元のクライアントに送信できます。

モデルの検証を使う場合は常に、状態変更コマンドを実行する前にモデルが有効であることを確認し、無効なデータによってアプリが破損しないようにする必要があります。 [フィルター](/aspnet/core/mvc/controllers/filters)を使って、すべてのアクションにそのためのコードを追加しなくて済むようにできます。 ASP.NET Core MVC フィルターを使うと、要求のグループをインターセプトして、共通のポリシーや横断的な事柄を特定の対象にだけ適用できます。 フィルターは、個々のアクション、コントローラー全体、またはアプリケーション全体に適用できます。

Web API に対しては、ASP.NET Core MVC は[_コンテンツ ネゴシエーション_](/aspnet/core/mvc/models/formatting)をサポートしており、応答の書式設定方法を要求で指定することができます。 要求で指定されたヘッダーに基づき、データを返すアクションは、XML、JSON、または他のサポートされている形式で応答を書式設定します。 この機能を使うと、同じ API を、データ形式要件が異なる複数のクライアントで使用できます。

Web API プロジェクトでは `[ApiController]` 属性の使用を検討してください。この属性は個々のコントローラー、ベース コントローラー クラス、アセンブリ全体に適用できます。 この属性によって、自動モデル検証が追加されます。アクションのモデルが無効な場合、BadRequest と検証エラーの詳細が返されます。 この属性ではまた、あらゆるアクションに、従来の経路を使用せず、属性経路を与えることが要求され、エラーに対する応答で、さらに詳しい ProblemDetails 情報が返されます。

> ### <a name="references--mapping-requests-to-responses"></a>参照 – 応答と要求のマッピング
>
> - **コントローラー アクションへのルーティング**
 > <https://docs.microsoft.com/aspnet/core/mvc/controllers/routing>
> - **モデル バインド**
 > <https://docs.microsoft.com/aspnet/core/mvc/models/model-binding>
> - **モデルの検証**
 > <https://docs.microsoft.com/aspnet/core/mvc/models/validation>
> - **フィルター**
 > <https://docs.microsoft.com/aspnet/core/mvc/controllers/filters>
> - **ApiController 属性**
 > <https://docs.microsoft.com/aspnet/core/web-api/>

## <a name="working-with-dependencies"></a>依存関係の使用

ASP.NET Core は、[依存関係の挿入](/aspnet/core/fundamentals/dependency-injection)と呼ばれる手法の組み込みサポートを備えており、内部的に使用します。 依存関係の挿入は、アプリケーションの異なる部分間を疎結合できる手法です。 結合を緩くすることを、アプリケーションの各部分の分離が容易になり、部分ごとにテストや置換が可能になるため、望ましいことです。 また、アプリケーションの 1 つの部分に対する変更が、アプリケーションの別の場所に予期しない影響を与える可能性も低くなります。 依存関係の挿入は、依存関係逆転 (Dependency Inversion) の原則に基づいており、多くの場合、開放/閉鎖 (Open/closed) の原則の実現に不可欠です。 アプリケーションによる依存関係の処理方法を評価するときは、[静的な結び付き](https://deviq.com/static-cling/)を持つコードのにおいに注意し、"[new は接着剤である](https://ardalis.com/new-is-glue)" という格言を思い出してください。

静的な結び付きは、クラスが静的メソッドを呼び出したとき、または静的プロパティにアクセスしたときに発生し、インフラストラクチャに対する副作用または依存関係があります。 たとえば、あるメソッドが静的メソッドを呼び出し、その静的メソッドがデータベースに書き込む場合、元のメソッドはデータベースに密に結合されます。 何かによってデータベース呼び出しが中断されると、メソッドも中断されます。 このようなメソッドのテストは、静的な呼び出しを模倣するために市販のモック ライブラリが必要になるか、またはテスト データベースを使ってしかテストできないため、非常に困難です。 インフラストラクチャに対する依存関係のない静的呼び出しは (特に、完全にステートレスのもの)、問題なく呼び出すことができて、結合や (コードの結合だけでなく静的な呼び出し自体の) テスト可能性に影響を与えません。

多くの開発者は、静的な結び付きとグローバルな状態のリスクを理解していますが、それでも直接的なインスタンス化によって特定の実装にコードを密接に結合します。 "new は接着剤である" という格言は、この結合を喚起するものであり、`new` キーワードの使用を全般に非難しているわけではありません。 静的メソッドの呼び出しと同様に、外部依存関係を持たない型の新しいインスタンスは通常、実装の詳細にコードを密接に結合したり、テストを困難にしたりすることはありません。 ただし、クラスをインスタンス化するたびに、その特定のインスタンスをその特定の場所にハード コーディングすることに意味があるかどうか、またはそのインスタンスを依存関係として要求する方が優れた設計ではないかということを、少し検討してください。

### <a name="declare-your-dependencies"></a>依存関係の宣言

ASP.NET Core のメソッドとクラスは依存関係を宣言するようになっており、引数としてそれを要求します。 通常、ASP.NET アプリケーションは Startup クラスにおいてセットアップされ、Startup クラス自体が複数のポイントで依存関係の挿入をサポートするように構成されています。 Startup クラスにコンストラクターがある場合は、次のように、コンストラクターで依存関係を要求できます。

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        var builder = new ConfigurationBuilder()
            .SetBasePath(env.ContentRootPath)
            .AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)
            .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true);
    }
}
```

Startup クラスの興味深い点は、明示的な型の要件がないことです。 特別な Startup 基底クラスを継承することなく、特定のインターフェイスも実装していません。 コンストラクターを提供してもしなくてもよく、コンストラクターでは必要なだけいくつでもパラメーターを指定できます。 アプリケーション用に構成した Web ホストは、開始するとき、使うように指示された Startup クラスを呼び出し、依存関係の挿入を使って Startup クラスに必要なすべての依存関係を設定します。 もちろん、ASP.NET Core によって使われるサービス コンテナーで構成されていないパラメーターを要求すると、例外が発生しますが、コンテナーが認識している依存関係を使っている限り、何でも自由に要求できます。

依存関係の挿入は、最初に Startup インスタンスを作成したときから、ASP.NET Core アプリに組み込まれています。 依存関係の挿入は Startup クラスだけの機能ではありません。 Configure メソッドで依存関係を要求することもできます。

```csharp
public void Configure(IApplicationBuilder app,
    IHostingEnvironment env,
    ILoggerFactory loggerFactory)
{

}
```

ただし、ConfigureServices メソッドはこの動作の例外であり、IServiceCollection 型のパラメーターを 1 つだけ受け取る必要があります。 このメソッドは、サービス コンテナーにオブジェクトを追加する役割を持ち、IServiceCollection パラメーターを介して現在構成されているすべてのサービスにアクセスできるため、依存関係の挿入をサポートする必要はまったくありません。 したがって、必要なサービスをパラメーターとして要求するか、ConfigureServices で IServiceCollection を使うことにより、Startup クラスのすべての部分で、ASP.NET Core のサービス コレクションで定義されている依存関係を使用できます。

> [!NOTE]
> 特定のサービスを Startup クラスで確実に利用できるようにする必要がある場合は、CreateDefaultBuilder 呼び出しの中で IWebHostBuilder とその ConfigureServices メソッドを使用して、それらを構成できます。

Startup クラスは、独自のサービスに対するコントローラーからミドルウェアやフィルターまで、ASP.NET Core アプリケーションの他の部分で必要な構成方法のモデルになります。 いずれの場合も、[明示的な依存関係の原則](https://deviq.com/explicit-dependencies-principle/)に従い、依存関係を直接作成するのではなく要求し、アプリケーション全体で依存関係の挿入を利用する必要があります。 実装を直接インスタンス化する場所と方法に注意する必要があります (特に、インフラストラクチャを使用する、または副作用があるサービスとオブジェクトの場合)。 特定の実装の種類に対する参照をハードコーディングするのではなく、抽象化をアプリケーション コアで定義し、引数として渡すようにします。

## <a name="structuring-the-application"></a>アプリケーションの構成

通常、モノリシックなアプリケーションのエントリ ポイントは 1 つです。 ASP.NET Core Web アプリケーションの場合は、ASP.NET Core Web プロジェクトがエントリ ポイントになります。 ただし、これは、ソリューションは 1 つのプロジェクトだけで構成する必要があるという意味ではありません。 懸念事項の分離に従って、アプリケーションを複数の異なるレイヤーに分割するのは役に立ちます。 レイヤーに分割した後は、フォルダーだけでなくプロジェクトも分割して、カプセル化を向上させることができます。 ASP.NET Core アプリケーションでこれらの目標を実現するための最善の方法は、第 5 章で説明されているクリーン アーキテクチャのバリエーションです。 この方法に従うと、アプリケーションのソリューションは、UI、Infrastructure、ApplicationCore に対する個別のライブラリで構成されるようになります。

これらのプロジェクトだけでなく、テスト プロジェクトも別に含まれます (テストについては 9 章で説明します)。

アプリケーションのオブジェクト モデルとインターフェイスは、ApplicationCore プロジェクトに置く必要があります。 このプロジェクトはできるだけ少ない依存関係を持つようにして、ソリューション内の他のプロジェクトはそれを参照します。 永続化する必要があるビジネス エンティティと、インフラストラクチャに直接依存していないサービスは、ApplicationCore プロジェクトで定義します。

永続化を実行する方法や、ユーザーに通知を送信する方法など、実装の詳細は、Infrastructure プロジェクトで保持します。 このプロジェクトは、Entity Framework Core などの実装固有のパッケージを参照しますが、これらの実装に関する詳細をプロジェクトの外部に公開しないようにする必要があります。 インフラストラクチャ サービスとリポジトリは ApplicationCore プロジェクトで定義されているインターフェイスを実装する必要があり、このプロジェクトの永続化の実装が、ApplicationCore で定義されているエンティティの取得と格納を行います。

ASP.NET Core UI プロジェクトは、UI レベルの処理を行いますが、ビジネス ロジックまたはインフラストラクチャの詳細を含まないようにする必要があります。 実際に、理想的なのは Infrastructure プロジェクトに対する依存関係さえ持つべきではなく、これにより 2 つのプロジェクト間に誤って依存関係が発生するのを防ぐことができます。 これは、Autofac などのサード パーティ製 DI コンテナーを使って実現でき、各プロジェクトの Module クラスで DI 規則を定義することができます。

実装の詳細からアプリケーションを分離するもう 1 つの方法は、通常は個別の Docker コンテナーで展開されているマイクロサービスをアプリケーションで呼び出すことです。 この方法を使うと、2 つのプロジェクト間に DI を利用する場合より、懸念事項の分離と分割はさらに大きくなりますが、複雑さは増大します。

### <a name="feature-organization"></a>機能の編成

既定では、ASP.NET Core アプリケーションは、コントローラーとビューさらに多くの場合は ViewModels を含むように、フォルダー構造を編成します。 通常、これらのサーバー側構造をサポートするためのクライアント側のコードは、wwwroot フォルダーに個別に格納されます。 ただし、大規模なアプリケーションでは、特定の機能を使用するためにこれらのフォルダー間を移動する必要があるため、このような編成では問題が発生する可能性があります。 各フォルダー内のファイルとサブフォルダーの数が増えるとますます困難になり、ソリューション エクスプローラーのスクロール量が膨大になります。 この問題の 1 つの解決策は、アプリケーションのコードをファイルの種類別ではなく "_機能_" 別に整理することです。 この編成スタイルは、通常、機能フォルダーまたは[機能スライス](https://docs.microsoft.com/archive/msdn-magazine/2016/september/asp-net-core-feature-slices-for-asp-net-core-mvc)と呼ばれます ([垂直スライス](https://deviq.com/vertical-slices/)も参照してください)。

ASP.NET Core MVC では、この目的のために区分 (Area) がサポートされています。 区分を使うと、各区分フォルダー内にコントローラー フォルダーとビュー フォルダー (および関連するすべてのモデル) のセットを個別に作成できます。 図 7-1 は、区分を使用したフォルダー構造の例です。

![区分編成の例](./media/image7-1.png)

**図 7-1**。 区分編成の例

区分を使う場合は、属性を使って、コントローラーが属する区分の名前でコントローラーを装飾する必要があります。

```csharp
[Area("Catalog")]
public class HomeController
{}
```

また、区分のサポートをルートに追加する必要があります。

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(name: "areaRoute", pattern: "{area:exists}/{controller=Home}/{action=Index}/{id?}");
    endpoints.MapControllerRoute(name: "default", pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

区分の組み込みサポートに加えて、独自のフォルダー構造を作成し、属性とカスタム ルートの代わりの規則を使うこともできます。 このようにすると、機能フォルダーにビューやコントローラーなどのフォルダーが個別に含まれないようにして、フラットな階層を維持し、各機能について 1 つの場所ですべての関連ファイルを簡単に見ることができるようになります。

ASP.NET Core は、組み込みの規則の種類を使って、その動作を制御します。 これらの規則は変更したり置き換えたりできます。 たとえば、名前空間に基づいて特定のコントローラーの機能名を自動的に取得する規則を作成できます (これは通常、コントローラーが存在するフォルダーに関連付けられます)。

```csharp
public class FeatureConvention : IControllerModelConvention
{
    public void Apply(ControllerModel controller)
    {
        controller.Properties.Add("feature",
        GetFeatureName(controller.ControllerType));
    }

    private string GetFeatureName(TypeInfo controllerType)
    {
        string[] tokens = controllerType.FullName.Split('.');
        if (!tokens.Any(t => t == "Features")) return "";
        string featureName = tokens
        .SkipWhile(t => !t.Equals("features",
        StringComparison.CurrentCultureIgnoreCase))
        .Skip(1)
        .Take(1)
        .FirstOrDefault();
        return featureName;
    }
}
```

その後、ConfigureServices でアプリケーションに MVC のサポートを追加するときに、オプションとしてこの規則を指定します。

```csharp
services.AddMvc(o => o.Conventions.Add(new FeatureConvention()));
```

また、ASP.NET Core MVC はビューを配置する場合にも規則を使います。 これをカスタム規則でオーバーライドして、ビューが独自の機能フォルダーに配置されるようにすることができます (上の FeatureConvention によって提供される機能名を使用)。 この方法について詳しくは、MSDN Magazine の記事「[ASP.NET Core MVC 向け機能スライス](https://docs.microsoft.com/archive/msdn-magazine/2016/september/asp-net-core-feature-slices-for-asp-net-core-mvc)」をご覧ください。実際に動くサンプルをダウンロードすることもできます。

### <a name="cross-cutting-concerns"></a>横断的な問題

アプリケーションが大きくなるほど、横断的な事柄を抽出して、重複を排除し、整合性を維持することの重要性が増します。 ASP.NET Core アプリケーションでの横断的な事柄の例としては、認証、モデル検証規則、出力キャッシュ、エラー処理などがありますが、その他にも多くのことがあります。 ASP.NET Core MVC で[フィルター](/aspnet/core/mvc/controllers/filters)を使うと、要求処理パイプラインの特定のステップの前または後にコードを実行できます。 たとえば、フィルターは、モデル バインドの前後、アクションの前後、またはアクションの結果の前後に実行できます。 また、承認フィルターを使って、パイプラインの残りの部分へのアクセスを制御することができます。 図 7-2 では、フィルターを構成した場合に要求の実行がそれをどのように通過するかを示します。

![要求は、承認フィルター、リソース フィルター、モデル バインド、アクション フィルター、アクションの実行とアクション結果の変換、例外フィルター、結果フィルター、結果の実行を介して処理されます。 終了間際に、要求は結果フィルターとリソース フィルターのみで処理されてから、クライアントに送信される応答になります。](./media/image7-2.png)

**図 7-2**。 フィルターと要求パイプラインを通過する要求実行の流れ。

通常、フィルターは属性として実装されるので、コントローラーまたはアクションにフィルターを適用できます (グローバルでも可能)。 この方法で追加したフィルターをアクション レベルで指定すると、コントローラー レベルで指定されたフィルターをオーバーライドするか、その上に構築されて、それ自体がグローバル フィルターをオーバーライドします。 たとえば、\[Route\] 属性を使って、コントローラーとアクションの間にルートを作成できます。 同様に、コントローラー レベルで承認を構成した後、次の例に示すように、個々のアクションでオーバーライドできます。

```csharp
[Authorize]
public class AccountController : Controller

{
    [AllowAnonymous] // overrides the Authorize attribute
    public async Task<IActionResult> Login() {}
    public async Task<IActionResult> ForgotPassword() {}
}
```

最初の Login メソッドでは、AllowAnonymous フィルター (属性) を使って、コントローラー レベルで設定されている Authorize フィルターをオーバーライドします。 ForgotPassword アクション (および AllowAnonymous 属性を持たないクラスの他のすべてのアクション) では、認証された要求が必要です。

フィルターを使うと、API に対する共通エラー処理ポリシーの形式で、重複を除去できます。 たとえば、一般的な API ポリシーでは、存在しないキーを参照している要求に対しては NotFound 応答を返し、モデルの検証が失敗した場合は BadRequest 応答を返します。 次の例では、アクションでのこれら 2 つのポリシーを示します。

```csharp
[HttpPut("{id}")]
public async Task<IActionResult> Put(int id, [FromBody]Author author)
{
    if ((await _authorRepository.ListAsync()).All(a => a.Id != id))
    {
        return NotFound(id);
    }
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }
    author.Id = id;
    await _authorRepository.UpdateAsync(author);
    return Ok();
}
```

このような条件付きコードで、アクション メソッドを乱雑にしないようにしてください。 代わりに、必要に応じて適用できるフィルターにポリシーを格納します。 この例では、コマンドが API に送られるたびに実行する必要があるモデルの検証チェックを、次の属性で置き換えることができます。

```csharp
public class ValidateModelAttribute : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        if (!context.ModelState.IsValid)
        {
            context.Result = new BadRequestObjectResult(context.ModelState);
        }
    }
}
```

[Ardalis.ValidateModel](https://www.nuget.org/packages/Ardalis.ValidateModel) パッケージを含めることで、NuGet 依存関係として `ValidateModelAttribute` をプロジェクトに追加できます。 API の場合、`ApiController` 属性を使用することで、別個の `ValidateModel` フィルターがなくてもこの動作を強制できます。

同様に、フィルターを使ってレコードが存在するかどうかをチェックし、アクションが実行される前に 404 を返して、アクションでこれらのチェックを実行する必要がないようにできます。 共通の規則を抽出し、インフラストラクチャ コードとビジネス ロジックを UI から分離するようにソリューションを構成すると、MVC アクション メソッドは非常にスリムになるはずです。

```csharp
[HttpPut("{id}")]
[ValidateAuthorExists]
public async Task<IActionResult> Put(int id, [FromBody]Author author)
{
    await _authorRepository.UpdateAsync(author);
    return Ok();
}
```

フィルターの実装の詳細については、MSDN Magazine の記事「[実際の ASP.NET Core MVC フィルター](https://docs.microsoft.com/archive/msdn-magazine/2016/august/asp-net-core-real-world-asp-net-core-mvc-filters)」を参照してください。また、実際に動作するサンプルをダウンロードすることもできます。

> ### <a name="references--structuring-applications"></a>参照 – アプリケーションの構成
>
> - **領域**  
>   <https://docs.microsoft.com/aspnet/core/mvc/controllers/areas>
> - **MSDN マガジン - ASP.NET Core MVC 向け機能スライス**  
>   <https://docs.microsoft.com/archive/msdn-magazine/2016/september/asp-net-core-feature-slices-for-asp-net-core-mvc>
> - **フィルター**  
>   <https://docs.microsoft.com/aspnet/core/mvc/controllers/filters>
> - **MSDN Magazine – 実際の ASP.NET Core MVC フィルター**  
>   <https://docs.microsoft.com/archive/msdn-magazine/2016/august/asp-net-core-real-world-asp-net-core-mvc-filters>

## <a name="security"></a>セキュリティ

Web アプリケーションのセキュリティ保護は大きなトピックであり、さまざまな考慮事項があります。 最も基本的なレベルのセキュリティには、特定の要求を行ったユーザーを認識し、その要求が必要なリソースだけにアクセスできるようにすることが含まれます。 認証は、要求で提供された資格情報を、信頼できるデータ ストア内の資格情報と比較して、要求を既知のエンティティから送信されたものとして扱う必要があるかどうかを確認するプロセスです。 承認は、ユーザーの ID に基づいて特定のリソースへのアクセスを制限するプロセスです。 セキュリティに関する 3 番目の考慮事項は、第三者による傍受から要求を保護することであり、少なくとも[アプリケーションが SSL を使っていることを確認する](/aspnet/core/security/enforcing-ssl)必要があります。

### <a name="authentication"></a>認証

ASP.NET Core Identity は、アプリケーションのログイン機能をサポートするために使うことができるメンバーシップ システムです。 ローカル ユーザー アカウントをサポートするだけでなく、Microsoft アカウント、Twitter、Facebook、Google などのプロバイダーからの外部ログイン プロバイダーもサポートします。 ASP.NET Core Identity だけでなく、アプリケーションでは Windows 認証や、[Identity Server](https://github.com/IdentityServer/IdentityServer4) のようなサード パーティの ID プロバイダーを使うこともできます。

[個別のユーザー アカウント] オプションを選択した場合、新しいプロジェクト テンプレートには ASP.NET Core Identity が含まれます。 このテンプレートには、登録、ログイン、外部ログイン、パスワードを忘れた場合、および追加機能のサポートが含まれています。

![[個別のユーザー アカウント] を選択して Identity を事前構成する](./media/image7-3.png)

**図 7-3**。 [個別のユーザー アカウント] を選択して Identity を事前構成する。

Identity のサポートは、Startup の ConfigureServices と Configure の両方で構成されます。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
    services.AddIdentity<ApplicationUser, IdentityRole>()
    .AddEntityFrameworkStores<ApplicationDbContext>()
    .AddDefaultTokenProviders();
    services.AddMvc();
}

public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();
    app.UseIdentity();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllerRoute(name: "default", pattern: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

Configure メソッドで UseMvc の前に UseIdentity を指定することが重要です。 ConfigureServices で Identity を構成すると、AddDefaultTokenProviders の呼び出しがあることがわかります。 これは、Web 通信をセキュリティ保護するために使われる場合があるトークンとは関係なく、ユーザーによる ID 確認のために SMS またはメールでユーザーに送信できるプロンプトを作成するプロバイダーを参照しています。

詳しくは、ASP.NET Core の公式ドキュメントで [2 要素認証の構成](/aspnet/core/security/authentication/2fa)および[外部ログイン プロバイダーの有効化](/aspnet/core/security/authentication/social/)に関する記事をご覧ください。

### <a name="authorization"></a>承認

承認の最も単純な形式には、匿名ユーザーに対するアクセスの制限が含まれます。 これは、特定のコントローラーまたはアクションに \[Authorize\] 属性を適用するだけで実現できます。 ロールを使っている場合は、次のように、特定のロールに属しているユーザーのアクセス制限まで、属性をさらに拡張できます。

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{

}
```

この例では、HRManager ロールと Finance ロールのどちらか一方または両方に属しているユーザーは、SalaryController にアクセスできます。 ユーザーが (複数のロールの 1 つだけでなく) 複数のロールに属していることを必要とするには、属性を複数回適用し、そのたびに必要なロールを指定できます。

特定のロール セットを文字列として多くの異なるコントローラーやアクションで指定すると、望ましくない繰り返しにつながります。 承認規則をカプセル化する承認ポリシーを構成し、\[Authorize\] 属性を適用するときに、個別のロールではなくポリシーを指定することができます。

```csharp
[Authorize(Policy = "CanViewPrivateReport")]
public IActionResult ExecutiveSalaryReport()
{
    return View();
}
```

この方法でポリシーを使うと、適用される特定のロールまたは規則から、制限されるアクションの種類を切り離すことができます。 後で、特定のリソースへのアクセスを必要とする新しいロールを作成する場合は、ポリシーを更新するだけでよく、すべての \[Authorize\] 属性ですべてのロール リストを更新する必要はありません。

#### <a name="claims"></a>クレーム

クレームは、認証済みユーザーのプロパティを表す名前と値のペアです。 たとえば、ユーザーの従業員番号をクレームとして格納できます。 その後、承認ポリシーの一部としてクレームを使用できます。 次の例では、"EmployeeNumber" という名前のクレームが存在する必要のある "EmployeeOnly" というポリシーを作成しています。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

その後、上記のように、このポリシーを \[Authorize\] 属性で使って、コントローラーやアクションを保護できます。

#### <a name="securing-web-apis"></a>Web API のセキュリティ保護

ほとんどの Web API は、トークン ベースの認証システムを実装する必要があります。 トークン認証はステートレスであり、拡張できるように設計されています。 トークン ベースの認証システムでは、クライアントは最初に認証プロバイダーで認証を行う必要があります。 それが成功した場合、クライアントはトークンを発行されます。トークンは、単に暗号化された意味のある文字列です。 その後、クライアントが API に要求を発行する必要があるときは、このトークンを要求のヘッダーとして追加します。 サーバーは、要求を完了する前に、要求ヘッダーに含まれるトークンを検証します。 図 7-4 はこのプロセスを示したものです。

![TokenAuth](./media/image7-4.png)

**図 7-4.** Web API に対するトークン ベースの認証。

独自の認証サービスを作成したり、Azure AD や OAuth と統合したり、[IdentityServer](https://github.com/IdentityServer) のようなオープンソースのツールを利用してサービスを実装したりできます。

#### <a name="custom-security"></a>カスタム セキュリティ

暗号化、ユーザー メンバーシップ、トークン生成システムの実装を "独自に展開する" 場合、特別な注意が必要です。 代案がたくさん売られているし、オープン ソースも利用できます。ほぼ間違いなく、カスタム実装よりセキュリティが優れています。

> ### <a name="references--security"></a>参照 – セキュリティ
>
> - **セキュリティ ドキュメントの概要**  
>   https://docs.microsoft.com/aspnet/core/security/
> - **ASP.NET Core アプリで SSL を適用する**  
>   <https://docs.microsoft.com/aspnet/core/security/enforcing-ssl>
> - **Identity の概要**  
>   <https://docs.microsoft.com/aspnet/core/security/authentication/identity>
> - **承認の概要**  
>   <https://docs.microsoft.com/aspnet/core/security/authorization/introduction>
> - **Azure App Service での API Apps に対する認証および承認**  
>   <https://docs.microsoft.com/azure/app-service-api/app-service-api-authentication>
> - **Identity Server**  
>   <https://github.com/IdentityServer>

## <a name="client-communication"></a>クライアントの通信

Web API によりページを提供してデータの要求に応答するだけでなく、ASP.NET Core アプリは接続されているクライアントと直接通信できます。 この送信通信では、さまざまなトランスポート テクノロジを使うことができ、最も一般的なものは WebSocket です。 ASP.NET Core SignalR は、サーバーとクライアントの間のリアルタイム通信機能をアプリケーションに容易に追加できるようにするライブラリです。 SignalR は、WebSocket などのさまざまなトランスポート テクノロジをサポートしており、多くの実装の詳細を開発者から抽象化します。

WebSocket を直接使うか、他の技法を使うかに関係なく、リアルタイムのクライアント通信は、さまざまなアプリケーション シナリオで役に立ちます。 次に、それらの例の一部を示します。

- ライブ チャット ルーム アプリケーション

- アプリケーションの監視

- ジョブの進行状況の更新

- 通知

- 対話型のフォーム アプリケーション

クライアント通信をアプリケーションに組み込むときは、通常、2 つのコンポーネントがあります。

- サーバー側の接続マネージャー (SignalR Hub、WebSocketManager WebSocketHandler)

- クライアント側のライブラリ

クライアントはブラウザーに限定されず、モバイル アプリ、コンソール アプリ、他のネイティブ アプリも SignalR/WebSocket を使って通信できます。 次の簡単なプログラムは、WebSocketManager サンプル アプリケーションの一部として、チャット アプリケーションに送信されたすべての内容をコンソールにエコーします。

```csharp
public class Program
{
    private static Connection _connection;
    public static void Main(string[] args)
    {
        StartConnectionAsync();
        _connection.On("receiveMessage", (arguments) =>
        {
            Console.WriteLine($"{arguments[0]} said: {arguments[1]}");
        });
        Console.ReadLine();
        StopConnectionAsync();
    }

    public static async Task StartConnectionAsync()
    {
        _connection = new Connection();
        await _connection.StartConnectionAsync("ws://localhost:65110/chat");
    }

    public static async Task StopConnectionAsync()
    {
        await _connection.StopConnectionAsync();
    }
}
```

アプリケーションがクライアント アプリケーションと直接通信する方法を検討し、リアルタイム通信によってアプリのユーザー エクスペリエンスが向上するかどうかを検討します。

> ### <a name="references--client-communication"></a>参照 – クライアントの通信
>
> - **ASP.NET Core SignalR**  
>   <https://github.com/dotnet/aspnetcore/tree/master/src/SignalR>
> - **WebSocket マネージャー**  
>   https://github.com/radu-matei/websocket-manager

## <a name="domain-driven-design--should-you-apply-it"></a>ドメイン駆動設計 – 適用する必要があるか?

ドメイン駆動設計 (DDD) は、"_ビジネス ドメイン_" に注目するソフトウェア構築のためのアジャイルな方法です。 実際のシステムのしくみについて開発者に関わることができるビジネス ドメインの専門家とのコミュニケーションと対話に重点が置かれます。 たとえば、株取引を処理するシステムを構築する場合のドメインの専門家は、経験豊富な株式ブローカーなどです。 DDD は、大規模で複雑なビジネスの問題に対処するように設計されており、多くの場合、ドメインの理解とモデリングに対する投資に見合わないため、小さくて単純なアプリケーションには適しません。

DDD 手法でソフトウェアを作成する場合、チーム (非技術的な利害関係者や貢献者を含む) は問題領域のための "_ユビキタス言語_" を開発する必要があります。 つまり、モデル化される実際の概念、それに相当するソフトウェア、概念を保持するために存在する構造 (データベース テーブルなど) に、同じ用語を使う必要があります。 したがって、ユビキタス言語で説明される概念は、"_ドメイン モデル_" の基礎を形成する必要があります。

ドメイン モデルは、相互に対話してシステムの動作を表すオブジェクトで構成されます。 これらのオブジェクトは以下のカテゴリに分けられます。

- [エンティティ](https://deviq.com/entity/)は、ID のスレッドでオブジェクトを表します。 エンティティは、通常、後で取得できるキーを使って永続的に格納されます。

- [集約](https://deviq.com/aggregate-pattern/)は、単位として永続化する必要のあるオブジェクトのグループを表します。

- [値オブジェクト](https://deviq.com/value-object/)は、プロパティ値の合計に基づいて比較できる概念を表します。 たとえば、開始日と終了日で構成される DateRange などです。

- [ドメイン イベント](https://martinfowler.com/eaaDev/DomainEvent.html)は、システムの他の部分にとって重要な、システム内で発生している出来事を表します。

DDD ドメイン モデルでは、モデル内に複雑な動作をカプセル化する必要があります。 特にエンティティは、プロパティの単なるコレクションであってはなりません。 ドメイン モデルに動作が欠如していて、システムの状態を表しているだけの場合は、[貧血症のモデル](https://deviq.com/anemic-model/)と呼ばれ、DDD では望ましくないことです。

これらのモデルの種類に加えて、通常、DDD ではさまざまなパターンが使用されます。

- [リポジトリ](https://deviq.com/repository-pattern/)は、永続化の詳細を抽象化します。

- [ファクトリ](https://en.wikipedia.org/wiki/Factory_method_pattern)は、複雑なオブジェクトの作成をカプセル化します。

- ドメイン イベントは、トリガーの動作から依存する動作を分離します。

- [サービス](http://gorodinski.com/blog/2012/04/14/services-in-domain-driven-design-ddd/)は、複雑な動作やインフラストラクチャの実装の詳細をカプセル化します。

- [コマンド](https://en.wikipedia.org/wiki/Command_pattern)は、コマンドの発行とコマンド自体の実行を切り離します。

- [仕様](https://deviq.com/specification-pattern/)は、クエリの詳細をカプセル化します。

また、DDD では、疎結合、カプセル化、単体テストを使って簡単に検証できるコードに対応するため、前に説明したクリーン アーキテクチャを使うことも推奨されます。

### <a name="when-should-you-apply-ddd"></a>DDD を適用する必要がある場合

DDD は、(技術的だけでなく) ビジネス的にも非常に複雑な大規模アプリケーションに適しています。 アプリケーションでは、ドメイン専門家の知識が必要になります。 ドメイン モデル自体にも重要な動作が存在する必要があり、データ ストアからさまざまなレコードの現在の状態を単に取得するだけでなく、ビジネス ルールや相互作用を表します。

### <a name="when-shouldnt-you-apply-ddd"></a>DDD を適用してはならない場合

DDD では、モデリング、アーキテクチャ、コミュニケーションへの投資が伴い、小規模なアプリケーションや基本的に CRUD (作成/読み取り/更新/削除) だけのアプリケーションでは正当化されない可能性があります。 アプリケーション開発のアプローチに DDD を選んだ場合でも、ドメインに動作を持たない貧血症のモデルがあることがわかったときは、アプローチの再考が必要な場合があります。 アプリケーションで DDD を使う必要がないか、またはデータベースやユーザー インターフェイスではなくドメイン モデルにビジネス ロジックをカプセル化するためのアプリケーションのリファクタリングにサポートが必要な可能性があります。

ハイブリッド アプローチでは、アプリケーションのトランザクション部分やさらに複雑な部分にだけ DDD を使い、アプリケーションの簡単な CRUD 部分や読み取り専用の部分には使わないようにします。 たとえば、レポートを表示したり、ダッシュボードにデータを視覚化したりするためにデータをクエリする場合、集約の制約を使用する必要はありません。 このような要件に対しては、独立したシンプルな読み取りモデルでまったく問題ありません。

> ### <a name="references--domain-driven-design"></a>参照 – ドメイン駆動設計
>
> - **わかりやすい英語での DDD (StackOverflow の回答)**  
>   <https://stackoverflow.com/questions/1222392/can-someone-explain-domain-driven-design-ddd-in-plain-english-please/1222488#1222488>

## <a name="deployment"></a>配置

ホストされる場所に関係なく、ASP.NET Core アプリケーションを展開するプロセスには複数のステップがあります。 最初のステップであるアプリケーションの発行は、`dotnet publish` CLI コマンドを使用して実行できます。 アプリケーションがコンパイルされて、アプリケーションの実行に必要なすべてのファイルが指定したフォルダに配置されます。 Visual Studio から展開する場合、このステップは自動的に実行されます。 publish フォルダーには、アプリケーションとその依存関係の .exe ファイルと .dll ファイルが格納されます。 自己充足型のアプリケーションには、.NET ランタイムのバージョンも含まれます。 ASP.NET Core アプリケーションには、構成ファイル、静的クライアント資産、MVC ビューも含まれます。

ASP.NET Core アプリケーションはコンソール アプリケーションであり、サーバーの起動時、およびアプリケーション (またはサーバー) がクラッシュした場合の再起動時に、起動される必要があります。 プロセス マネージャーを使って、このプロセスを自動化できます。 ASP.NET Core の最も一般的なプロセス マネージャーは、Linux の場合は Nginx と Apache、Windows の場合は IIS と Windows Service です。

プロセス マネージャーに加え、ASP.NET Core アプリケーションでは、リバース プロキシ サーバーも使用できます。 リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。 リバース プロキシ サーバーによって、アプリケーションにセキュリティ層が提供されます。 また、Kestrel は同じポートでの複数アプリケーションのホストもサポートしていないので、ホスト ヘッダーなどの手法を使って同じポートと IP アドレスで複数のアプリケーションをホストすることはできません。

![インターネットに対する Kestrel](./media/image7-5.png)

**図 7-5**。 リバース プロキシ サーバーの背後の Kestrel でホストされている ASP.NET

リバース プロキシが役に立つもう 1 つのシナリオは、SSL/HTTPS を使って複数のアプリケーションをセキュリティ保護する場合です。 この場合、リバース プロキシでは SSL だけを構成する必要があります。 リバース プロキシ サーバーと Kestrel の間の通信は、HTTP で行われます (図 7-6 を参照)。

![HTTPS で保護されたリバース プロキシ サーバーの背後でホストされている ASP.NET](./media/image7-6.png)

**図 7-6**。 HTTPS で保護されたリバース プロキシ サーバーの背後でホストされている ASP.NET

急速に普及しているアプローチは ASP.NET Core アプリケーションを Docker コンテナーでホストする方法で、ローカルにホストしたり、クラウド ベースのホスト用に Azure に展開したりできます。 Docker コンテナーは、Kestrel で実行されるアプリケーションのコードを格納することができ、上記のように、リバース プロキシ サーバーの背後に展開されます。

Azure でアプリケーションをホストしている場合は、専用の仮想アプライアンスとして Microsoft Azure Application Gateway を使って、複数のサービスを提供できます。 個々のアプリケーションに対するリバース プロキシとして動作するだけでなく、Application Gateway は次の機能を提供することもできます。

- HTTP の負荷分散

- SSL のオフロード (インターネットに対する SSL のみ)

- エンド ツー エンドの SSL

- 複数サイトのルーティング (最大 20 個のサイトを 1 つの Application Gateway に統合)

- Web アプリケーション ファイアウォール

- WebSocket のサポート

- 高度な診断

_Azure のデプロイ オプションについては、[10 章](development-process-for-azure.md)で説明します。_

> ### <a name="references--deployment"></a>参照 – 展開
>
> - **ホスティングと展開の概要**  
>   <https://docs.microsoft.com/aspnet/core/publishing/>
> - **Kestrel とリバース プロキシを使用するタイミング**  
>   <https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy>
> - **Docker で ASP.NET Core アプリをホストする**  
>   <https://docs.microsoft.com/aspnet/core/publishing/docker>
> - **Azure Application Gateway の概要**  
>   <https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction>

>[!div class="step-by-step"]
>[前へ](common-client-side-web-technologies.md)
>[次へ](work-with-data-in-asp-net-core-apps.md)
