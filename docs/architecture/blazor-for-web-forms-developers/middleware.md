---
title: モジュール、ハンドラー、ミドルウェア
description: モジュール、ハンドラー、およびミドルウェアを使用した HTTP 要求の処理について説明します。
author: danroth27
ms.author: daroth
ms.date: 10/11/2019
ms.openlocfilehash: 3ecc109c54f88b5b06a1474f7c6e262d426a78a9
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75337473"
---
# <a name="modules-handlers-and-middleware"></a>モジュール、ハンドラー、ミドルウェア

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

ASP.NET Core アプリは一連の*ミドルウェア*に基づいて構築されています。 ミドルウェアは、要求と応答を処理するためにパイプラインに配置されるハンドラーです。 Web フォームアプリでは、HTTP ハンドラーとモジュールは同様の問題を解決します。 ASP.NET Core では、モジュール、ハンドラー、 *Global.asax.cs*、およびアプリのライフサイクルはミドルウェアに置き換えられます。 この章では、Blazor アプリのコンテキストでのミドルウェアについて説明します。

## <a name="overview"></a>の概要

ASP.NET Core 要求パイプラインは、順番に呼び出される一連の要求デリゲートで構成されています。 次の図は、その概念を示しています。 実行のスレッドは黒い矢印をたどります。

![パイプライン](media/middleware/request-delegate-pipeline.png)

上の図には、ライフサイクルイベントの概念がありません。 この概念は、ASP.NET Web フォーム要求がどのように処理されるかについての基礎となります。 このシステムにより、どのようなプロセスが発生しているかがわかりやすくなり、ミドルウェアをいつでも挿入できるようになります。 ミドルウェアは、要求パイプラインに追加された順序で実行されます。 また、構成ファイルではなくコードに追加されています (通常は*Startup.cs*)。

## <a name="katana"></a>Katana

Katana に慣れている読者は、ASP.NET Core に慣れることができます。 実際、Katana は、ASP.NET Core が派生するフレームワークです。 ASP.NET 4.x 用の同様のミドルウェアとパイプラインパターンが導入されました。 Katana 向けに設計されたミドルウェアは、ASP.NET Core パイプラインで動作するように調整できます。

## <a name="common-middleware"></a>共通ミドルウェア

ASP.NET 4.x には多数のモジュールが含まれています。 同様に、ASP.NET Core にも多くのミドルウェアコンポーネントが用意されています。 IIS モジュールは、ASP.NET Core がある場合に使用できます。 それ以外の場合は、ネイティブ ASP.NET Core ミドルウェアを使用できます。

次の表に、ASP.NET Core の代替ミドルウェアとコンポーネントの一覧を示します。

|Module                 |ASP.NET 4.x モジュール           |ASP.NET Core オプション|
|-----------------------|-----------------------------|-------------------|
|HTTP エラー            |`CustomErrorModule`          |[状態コード ページ ミドルウェア](/aspnet/core/fundamentals/error-handling#usestatuscodepages)|
|既定のドキュメント       |`DefaultDocumentModule`      |[既定のファイル ミドルウェア](/aspnet/core/fundamentals/static-files#serve-a-default-document)|
|ディレクトリの参照     |`DirectoryListingModule`     |[ディレクトリ参照ミドルウェア](/aspnet/core/fundamentals/static-files#enable-directory-browsing)|
|動的圧縮    |`DynamicCompressionModule`   |[応答圧縮ミドルウェア](/aspnet/core/performance/response-compression)|
|失敗した要求のトレース|`FailedRequestsTracingModule`|[ASP.NET Core のログ](/aspnet/core/fundamentals/logging/index#tracesource-provider)|
|ファイルのキャッシュ           |`FileCacheModule`            |[応答キャッシュ ミドルウェア](/aspnet/core/performance/caching/middleware)|
|HTTP キャッシュ           |`HttpCacheModule`            |[応答キャッシュ ミドルウェア](/aspnet/core/performance/caching/middleware)|
|HTTP ログ           |`HttpLoggingModule`          |[ASP.NET Core のログ](/aspnet/core/fundamentals/logging/index)|
|HTTP リダイレクト       |`HttpRedirectionModule`      |[ミドルウェアの URL リライト](/aspnet/core/fundamentals/url-rewriting)|
|ISAPI フィルター          |`IsapiFilterModule`          |[ミドルウェア](/aspnet/core/fundamentals/middleware/index)|
|ISAPI                  |`IsapiModule`                |[ミドルウェア](/aspnet/core/fundamentals/middleware/index)|
|要求のフィルター処理      |`RequestFilteringModule`     |[URL リライトミドルウェア IRule](/aspnet/core/fundamentals/url-rewriting#irule-based-rule)|
|URL の書き換え&#8224;   |`RewriteModule`              |[ミドルウェアの URL リライト](/aspnet/core/fundamentals/url-rewriting)|
|静的な圧縮     |`StaticCompressionModule`    |[応答圧縮ミドルウェア](/aspnet/core/performance/response-compression)|
|静的コンテンツ         |`StaticFileModule`           |[静的ファイル ミドルウェア](/aspnet/core/fundamentals/static-files)|
|URL 承認      |`UrlAuthorizationModule`     |[ASP.NET Core ID](/aspnet/core/security/authentication/identity)|

このリストは完全なものではありませんが、2つのフレームワーク間にどのようなマッピングが存在するのかを把握する必要があります。 詳細な一覧については、「 [ASP.NET Core を使用した IIS モジュール](/aspnet/core/host-and-deploy/iis/modules)」を参照してください。

## <a name="custom-middleware"></a>カスタムミドルウェア

組み込みのミドルウェアでは、アプリに必要なすべてのシナリオを処理することはできません。 このような場合は、独自のミドルウェアを作成するのが理にかなっています。 ミドルウェアを定義する方法は複数ありますが、最も単純なのは単純なデリゲートです。 クエリ文字列からカルチャ要求を受け取る次のミドルウェアを考えてみます。

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            var cultureQuery = context.Request.Query["culture"];

            if (!string.IsNullOrWhiteSpace(cultureQuery))
            {
                var culture = new CultureInfo(cultureQuery);

                CultureInfo.CurrentCulture = culture;
                CultureInfo.CurrentUICulture = culture;
            }

            // Call the next delegate/middleware in the pipeline
            await next();
        });

        app.Run(async (context) =>
            await context.Response.WriteAsync(
                $"Hello {CultureInfo.CurrentCulture.DisplayName}"));
    }
}
```

ミドルウェアは、`IMiddleware` インターフェイスを実装するか、次のミドルウェア規則に従って、クラスとして定義することもできます。 詳細については、「[カスタム ASP.NET Core ミドルウェアを作成](/aspnet/core/fundamentals/middleware/write)する」を参照してください。

>[!div class="step-by-step"]
>[前へ](data.md)
>[次へ](config.md)
