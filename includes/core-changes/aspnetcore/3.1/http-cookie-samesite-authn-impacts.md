---
ms.openlocfilehash: 3cc07eef109b9096bc5a5fbcd1ea098a23b2155f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78968344"
---
### <a name="http-browser-samesite-changes-impact-authentication"></a>HTTP:ブラウザー SameSite の変更による認証への影響

Chrome や Firefox などの一部のブラウザーでは、Cookie の `SameSite` の実装に破壊的変更が加えられました。 この変更は、OpenID Connect や WS-Federation などのリモート認証シナリオに影響します。これをオプトアウトするには、`SameSite=None` を送信します。 ただし、iOS 12 および一部の古いバージョンの他のブラウザーでは `SameSite=None` は中断します。 アプリはこれらのバージョンをスニッフィングし、`SameSite` を省略する必要があります。

この問題に関するディスカッションについては、[dotnet/aspnetcore#14996](https://github.com/dotnet/aspnetcore/issues/14996) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.1 Preview 1

#### <a name="old-behavior"></a>以前の動作

`SameSite` は、HTTP Cookie の 2016 ドラフト標準の拡張機能です。 これは、クロスサイト リクエスト フォージェリ (CSRF) を軽減することを目的としています。 もともと、これは、新しいパラメーターを追加することでサーバーでオプトインされる機能として設計されました。 ASP.NET Core 2.0 で `SameSite` の初期サポートが追加されました。

#### <a name="new-behavior"></a>新しい動作

Google から、は下位互換性のない新しいドラフト標準が提案されました この標準では、既定のモードが `Lax` に変更され、オプトアウト対象の新しいエントリ `None` が追加されています。`Lax` はほとんどのアプリの Cookie に十分です。ただし、OpenID Connect や WS-Federation ログインなどのクロスサイト シナリオは中断されます。 ほとんどの OAuth ログインは、要求フローの違いによって影響を受けません。 新しい `None` パラメーターを使うと、以前のドラフト標準 (iOS 12 など) を実装したクライアントとの互換性の問題が発生します。 Chrome 80 にこの変更が含まれます。 Chrome 製品の発売タイムラインについては、「[SameSite Updates](https://www.chromium.org/updates/same-site)」(SameSite の更新) を参照してください。

ASP.NET Core 3.1 は、新しい `SameSite` 動作を実装するように更新されました。 この更新により、`SameSite=None` が出力され、`SameSite` 属性を省略する新しい値 `SameSiteMode.Unspecified` が追加されるように `SameSiteMode.None` の動作が再定義されました。 すべての Cookie API の既定は `Unspecified` になりましたが、Cookie を使用する一部のコンポーネントでは、OpenID Connect の相関関係や nonce Cookie など、シナリオに固有の値を設定します。

この分野の最近の変更については、「[HTTP:SameSite の cookie オプションの既定値が一部、None に変更されました](/dotnet/core/compatibility/2.2-3.0#http-some-cookie-samesite-defaults-changed-to-none)」を参照してください。 ASP.NET Core 3.0 では、ほとんどの既定値が <xref:Microsoft.AspNetCore.Http.SameSiteMode.Lax?displayProperty=nameWithType> から <xref:Microsoft.AspNetCore.Http.SameSiteMode.None?displayProperty=nameWithType> に変更されました (ただし、以前の標準を使用しています)。

#### <a name="reason-for-change"></a>変更理由

既に概要を説明したように、ブラウザーと仕様が変わっています。

#### <a name="recommended-action"></a>推奨アクション

サードパーティ ログインなどを介してリモート サイトとやり取りするアプリは、以下を行う必要があります。

* 複数のブラウザー上でこのようなシナリオをテストする。
* 「[以前のブラウザーをサポートする](#support-older-browsers)」で説明されている Cookie ポリシー ブラウザー スニッフィングの軽減策を適用します。

テストとブラウザー スニッフィングの手順については、次のセクションを参照してください。

##### <a name="determine-if-youre-affected"></a>影響を受けているかどうかを判断する

新しい動作をオプトインできるクライアント バージョンを使用して、Web アプリをテストします。 Chrome、Firefox、Microsoft Edge Chromium のいずれにも、テストに使用できる新しいオプトイン機能フラグがあります。 パッチを適用した後、アプリが古いクライアント バージョンと互換性があることを確認します (特に Safari)。 詳細については、「[以前のブラウザーをサポートする](#support-older-browsers)」を参照してください。

##### <a name="chrome"></a>Chrome

Chrome 78 以降では、誤解を招くテスト結果が生成されます。 これらのバージョンには一時的な軽減策が適用されており、2 分前よりも短い Cookie が許可されます。 適切なテスト フラグを有効にすると、Chrome 76 および 77 からはより正確な結果が生成されます。 新しい動作をテストするには、`chrome://flags/#same-site-by-default-cookies` を有効に切り替えます。 Chrome 75 以前は、新しい `None` 設定を使うと失敗すると報告されています。 詳細については、「[以前のブラウザーをサポートする](#support-older-browsers)」を参照してください。

Google は、以前のバージョンの Chrome を提供していません。 ただし、以前のバージョンの Chromium をダウンロードすることはできますが、テスト目的には十分です。 [Chromium のダウンロード](https://www.chromium.org/getting-involved/download-chromium)に関する記事の手順に従ってください。

* [Chromium 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chromium 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

##### <a name="safari"></a>Safari

Safari 12 では以前のドラフトが厳密に実装されており、Cookie に新しい `None` 値が存在すると失敗します。 これは、「[以前のブラウザーをサポートする](#support-older-browsers)」に示されているブラウザー スニッフィング コードを使って防ぐ必要があります。 Microsoft Authentication Library (MSAL)、Active Directory 認証ライブラリ (ADAL)、または使用している任意のライブラリを使って、Safari 12 および 13 と WebKit ベースの OS スタイルのログインをテストしてください。 この問題は、基盤の OS バージョンによって変わります。 OSX Mojave 10.14 および iOS 12 には、新しい動作との互換性の問題があることがわかっています。 OSX Catalina 10.15 または iOS 13 にアップグレードすると、問題が解決します。 現在、Safari には新しい仕様の動作をテストするためのオプトイン フラグがありません。

##### <a name="firefox"></a>Firefox

Firefox の新しい標準のサポートは、バージョン 68 以降で、機能フラグ `network.cookie.sameSite.laxByDefault` を指定して `about:config` ページで選択することでオプトインできます。 以前のバージョンの Firefox では、互換性の問題は報告されていません。

##### <a name="microsoft-edge"></a>Microsoft Edge

Microsoft Edge では古い `SameSite` 標準がサポートされていますが、バージョン 44 の時点では、新しい標準との互換性の問題はありませんでした。

##### <a name="microsoft-edge-chromium"></a>Microsoft Edge Chromium

機能フラグは `edge://flags/#same-site-by-default-cookies` です。 Microsoft Edge Chromium 78 を使ってテストした際には、互換性の問題は検出されませんでした。

##### <a name="electron"></a>Electron

Electron の複数のバージョンには、Chromium の古いバージョンが含まれています。 たとえば、Microsoft Teams で使用されている Electron のバージョンは Chromium 66 であり、以前の動作を示しています。 お使いの製品に使用されている Electron のバージョンを使って、ご自分で互換性テストを実行してください。 詳細については、「[以前のブラウザーをサポートする](#support-older-browsers)」を参照してください。

##### <a name="support-older-browsers"></a>以前のブラウザーをサポートする

2016 `SameSite` 標準では、不明な値を `SameSite=Strict` 値として扱うことが義務付けられていました。 そのため、元の標準をサポートする以前のブラウザーがある場合、値が `None` の `SameSite` プロパティが表示されたときに、中断される可能性があります。 このような以前のブラウザーをサポートする場合、Web アプリではブラウザー スニッフィングを実装する必要があります。 `User-Agent` 要求ヘッダー値は非常に不安定であり、週単位で変わるため、ASP.NET Core ではブラウザー スニッフィングを実装していません。 代わりに、Cookie ポリシーの拡張ポイントを使って、`User-Agent` 固有のロジックを追加できます。

*Startup.cs* で、次のコードを追加します。

```csharp
private void CheckSameSite(HttpContext httpContext, CookieOptions options)
{
    if (options.SameSite == SameSiteMode.None)
    {
        var userAgent = httpContext.Request.Headers["User-Agent"].ToString();
        // TODO: Use your User Agent library of choice here.
        if (/* UserAgent doesn't support new behavior */)
        {
            options.SameSite = SameSiteMode.Unspecified;
        }
    }
}

public void ConfigureServices(IServiceCollection services)
{
    services.Configure<CookiePolicyOptions>(options =>
    {
        options.MinimumSameSitePolicy = SameSiteMode.Unspecified;
        options.OnAppendCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
        options.OnDeleteCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
    });
}

public void Configure(IApplicationBuilder app)
{
    // Before UseAuthentication or anything else that writes cookies.
    app.UseCookiePolicy();

    app.UseAuthentication();
    // code omitted for brevity
}
```

##### <a name="opt-out-switches"></a>オプトアウト スイッチ

`Microsoft.AspNetCore.SuppressSameSiteNone` 互換性スイッチを使用すると、新しい ASP.NET Core Cookie の動作を一時的にオプトアウトできます。 プロジェクトの *runtimeconfig.template.json* ファイルに次の JSON を追加します。

```json
{
  "configProperties": {
    "Microsoft.AspNetCore.SuppressSameSiteNone": "true"
  }
}
```

##### <a name="other-versions"></a>その他のバージョン

関連する `SameSite` パッチは以下に対して適用されます。

* ASP.NET Core 2.1、2.2、および 3.0
* `Microsoft.Owin` 4.1
* `System.Web` (.NET Framework 4.7.2 以降の場合)

#### <a name="category"></a>カテゴリ

ASP.NET

#### <a name="affected-apis"></a>影響を受ける API

- <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy%2A?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite%2A?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite%2A?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.Http.SameSiteMode?displayProperty=fullName>
- <xref:Microsoft.Net.Http.Headers.SameSiteMode?displayProperty=fullName>
- <xref:Microsoft.Net.Http.Headers.SetCookieHeaderValue.SameSite%2A?displayProperty=fullName>

<!--

#### Affected APIs

- `Overload:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy`
- `Overload:Microsoft.AspNetCore.Http.CookieBuilder.SameSite`
- `Overload:Microsoft.AspNetCore.Http.CookieOptions.SameSite`
- `T:Microsoft.AspNetCore.Http.SameSiteMode`
- `T:Microsoft.Net.Http.Headers.SameSiteMode`
- `Overload:Microsoft.Net.Http.Headers.SetCookieHeaderValue.SameSite`

-->
