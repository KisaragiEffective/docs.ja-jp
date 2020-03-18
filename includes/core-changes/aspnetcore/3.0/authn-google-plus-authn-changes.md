---
ms.openlocfilehash: c634c43e72d345721f2d8f2e9f45760e927a86ab
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72394072"
---
### <a name="authentication-google-deprecated-and-replaced"></a>認証:Google+ が非推奨になり、置き換えられました

Google では、早ければ 2019 年 1 月 28 日をもってアプリ向け Google+ Sign-in の[サービスが終了](https://developers.google.com/+/api-shutdown)します。

#### <a name="change-description"></a>変更の説明

ASP.NET 4.x と ASP.NET Core では、Google+ Sign-in API を使用して、Web アプリで Google アカウント ユーザーを認証しています。 影響を受ける NuGet パッケージは、ASP.NET Core の場合は [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google/)、ASP.NET Web Forms および MVC での `Microsoft.Owin` の場合は [Microsoft.Owin.Security.Google](https://www.nuget.org/packages/Microsoft.Owin.Security.Google/) です。

代わりとなる Google の API では、別のデータ ソースと形式が使用されています。 構造的な変更については、以下に示す軽減策と解決策を参照してください。 データ自体が要件を満たしているかどうかをアプリで確認する必要があります。 たとえば、名前、電子メール アドレス、プロファイル リンク、プロファイル写真では、以前とは微妙に異なる値が提供される場合があります。

#### <a name="version-introduced"></a>導入されたバージョン

すべてのバージョン。 この変更は、ASP.NET Core の外部での変更です。

#### <a name="recommended-action"></a>推奨アクション

##### <a name="owin-with-aspnet-web-forms-and-mvc"></a>ASP.NET Web Forms および MVC での Owin

`Microsoft.Owin` 3.1.0 以降については、一時的な軽減策の概要が[こちら](https://github.com/aspnet/AspNetKatana/issues/251#issuecomment-449587635)にあります。 アプリでは、データ形式の変更を確認するために、軽減策を使用してテストを完了する必要があります。 `Microsoft.Owin` 4.0.1 と修正プログラムをリリースする計画があります。 以前のバージョンを使用しているアプリは、バージョン 4.0.1 に更新する必要があります。

##### <a name="aspnet-core-1x"></a>ASP.NET Core 1.x

[ASP.NET Web Forms および MVC での Owin](#owin-with-aspnet-web-forms-and-mvc) の軽減策は、ASP.NET Core 1.x に適応できます。 1\.x は[有効期間の終了](https://dotnet.microsoft.com/platform/support-policy)状態に達したため、NuGet パッケージの修正プログラムは計画されていません。

##### <a name="aspnet-core-2x"></a>ASP.NET Core 2.x

`Microsoft.AspNetCore.Authentication.Google` バージョン 2.x の場合は、`Startup.ConfigureServices` での `AddGoogle` の既存の呼び出しを次のコードに置き換えます。

```csharp
.AddGoogle(o =>
{
    o.ClientId = Configuration["Authentication:Google:ClientId"];
    o.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
    o.UserInformationEndpoint = "https://www.googleapis.com/oauth2/v2/userinfo";
    o.ClaimActions.Clear();
    o.ClaimActions.MapJsonKey(ClaimTypes.NameIdentifier, "id");
    o.ClaimActions.MapJsonKey(ClaimTypes.Name, "name");
    o.ClaimActions.MapJsonKey(ClaimTypes.GivenName, "given_name");
    o.ClaimActions.MapJsonKey(ClaimTypes.Surname, "family_name");
    o.ClaimActions.MapJsonKey("urn:google:profile", "link");
    o.ClaimActions.MapJsonKey(ClaimTypes.Email, "email");
});
```

2 月の 2.1 と 2.2 の修正プログラムには、先行の再構成が新しい既定値として組み込まれています。 ASP.NET Core 2.0 は[有効期間の終了](https://dotnet.microsoft.com/platform/support-policy)に達したため、修正プログラムは計画されていません。

##### <a name="aspnet-core-30"></a>ASP.NET Core 3.0

ASP.NET Core 2.x 用に提供されている軽減策は、ASP.NET Core 3.0 にも使用できます。 今後の 3.0 のプレビューにおいて、`Microsoft.AspNetCore.Authentication.Google` パッケージが削除される可能性があります。 ユーザーは、代わりに `Microsoft.AspNetCore.Authentication.OpenIdConnect` に誘導されます。 `Startup.ConfigureServices` 内の `AddGoogle` を `AddOpenIdConnect` に置き換える方法を次のコードに示します。 この置き換えは ASP.NET Core 2.0 以降で使用でき、必要に応じて ASP.NET Core 1.x にも適応できます。

```csharp
.AddOpenIdConnect("Google", o =>
{
    o.ClientId = Configuration["Authentication:Google:ClientId"];
    o.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
    o.Authority = "https://accounts.google.com";
    o.ResponseType = OpenIdConnectResponseType.Code;
    o.CallbackPath = "/signin-google"; // Or register the default "/sigin-oidc"
    o.Scope.Add("email");
});
JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();
```

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

<xref:Microsoft.AspNetCore.Authentication.Google?displayProperty=fullName>

<!-- 

#### Affected APIs

`N:Microsoft.AspNetCore.Authentication.Google`

-->
