---
ms.openlocfilehash: 494e792d63a611cdaedf3e40aa607cfbb0420ae4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901765"
---
### <a name="authentication-newtonsoftjson-types-replaced"></a>認証:Newtonsoft.Json 型の置き換え

ASP.NET Core 3.0 では、Authentication API で使用される `Newtonsoft.Json` 型が `System.Text.Json` 型に置き換えられました。 次の場合を除き、Authentication パッケージの基本的な使用方法は影響を受けません。

* [aspnet-contrib](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) のプロバイダーなど、OAuth プロバイダーから派生したクラス。
* 高度な要求操作の実装。

詳細については、[dotnet/aspnetcore#7105](https://github.com/dotnet/aspnetcore/pull/7105) を参照してください。 ディスカッションについては、[dotnet/aspnetcore#7289](https://github.com/dotnet/aspnetcore/issues/7289) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="recommended-action"></a>推奨アクション

派生 OAuth 実装の場合、最も一般的な変更は、[こちら](https://github.com/dotnet/aspnetcore/pull/7105/files?utf8=%E2%9C%93&diff=unified&w=1#diff-e1c9f9740a6fe8021020a6f249c589b0L40)で示すように、`CreateTicketAsync` オーバーライドで `JObject.Parse` を `JsonDocument.Parse` に置き換えることです。 `JsonDocument` は、`IDisposable` を実装します。

既知の変更の概要を以下に示します。

- <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run(Newtonsoft.Json.Linq.JObject,System.Security.Claims.ClaimsIdentity,System.String)?displayProperty=nameWithType> は `ClaimAction.Run(JsonElement userData, ClaimsIdentity identity, string issuer)` になります。 `ClaimAction` のすべての派生実装も同様に影響を受けます。
- <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapCustomJson(Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection,System.String,System.Func{Newtonsoft.Json.Linq.JObject,System.String})?displayProperty=nameWithType> は `MapCustomJson(this ClaimActionCollection collection, string claimType, Func<JsonElement, string> resolver)` になります。
- <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapCustomJson(Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection,System.String,System.String,System.Func{Newtonsoft.Json.Linq.JObject,System.String})?displayProperty=nameWithType> は `MapCustomJson(this ClaimActionCollection collection, string claimType, string valueType, Func<JsonElement, string> resolver)` になります。
- <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthCreatingTicketContext> では、古いコンストラクターの 1 つが削除され、もう 1 つのコンストラクターでは `JObject` が `JsonElement` に置き換えられました。 これに合わせて、`User` プロパティと `RunClaimActions` メソッドが更新されました。
- <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthTokenResponse.Success(Newtonsoft.Json.Linq.JObject)> は、`JObject` ではなく、`JsonDocument` 型のパラメーターを受け取るようになりました。 これに合わせて `Response` プロパティが更新されました。 `OAuthTokenResponse` が破棄可能になり、`OAuthHandler` によって破棄されます。 `ExchangeCodeAsync` をオーバーライドする派生 OAuth 実装では、`JsonDocument` または `OAuthTokenResponse` を破棄する必要はありません。
- <xref:Microsoft.AspNetCore.Authentication.OpenIdConnect.UserInformationReceivedContext.User?displayProperty=nameWithType> が `JObject` から `JsonDocument` に変更されました。
- <xref:Microsoft.AspNetCore.Authentication.Twitter.TwitterCreatingTicketContext.User?displayProperty=nameWithType> が `JObject` から `JsonElement` に変更されました。
- <xref:Microsoft.AspNetCore.Authentication.Twitter.TwitterHandler.CreateTicketAsync(System.Security.Claims.ClaimsIdentity,Microsoft.AspNetCore.Authentication.AuthenticationProperties,Microsoft.AspNetCore.Authentication.Twitter.AccessToken,Newtonsoft.Json.Linq.JObject)?displayProperty=nameWithType> が `JObject` の受け入れから `JsonElement` に変更されました。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

- <xref:Microsoft.AspNetCore.Authentication.Facebook?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Authentication.Google?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Authentication.MicrosoftAccount?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Authentication.OAuth?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Authentication.OpenIdConnect?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Authentication.Twitter?displayProperty=nameWithType>

<!--

#### Affected APIs

- `N:Microsoft.AspNetCore.Authentication.Facebook`
- `N:Microsoft.AspNetCore.Authentication.Google`
- `N:Microsoft.AspNetCore.Authentication.MicrosoftAccount`
- `N:Microsoft.AspNetCore.Authentication.OAuth`
- `N:Microsoft.AspNetCore.Authentication.OpenIdConnect`
- `N:Microsoft.AspNetCore.Authentication.Twitter`

-->
