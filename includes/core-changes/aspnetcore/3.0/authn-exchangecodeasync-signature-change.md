---
ms.openlocfilehash: a4e20e0468d861138ad801c9dbfa15340b3f388c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72394284"
---
### <a name="authentication-oauthhandler-exchangecodeasync-signature-changed"></a>認証:OAuthHandler ExchangeCodeAsync 署名が変更されました

ASP.NET Core 3.0 では、`OAuthHandler.ExchangeCodeAsync` の署名が次のように変更されました。

```csharp
protected virtual System.Threading.Tasks.Task<Microsoft.AspNetCore.Authentication.OAuth.OAuthTokenResponse> ExchangeCodeAsync(string code, string redirectUri) { throw null; }
```

移動先:

```csharp
protected virtual System.Threading.Tasks.Task<Microsoft.AspNetCore.Authentication.OAuth.OAuthTokenResponse> ExchangeCodeAsync(Microsoft.AspNetCore.Authentication.OAuth.OAuthCodeExchangeContext context) { throw null; }
```

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

`code` と `redirectUri` の文字列が個別の引数として渡されました。

#### <a name="new-behavior"></a>新しい動作

`Code` と `RedirectUri` は、`OAuthCodeExchangeContext` コンストラクターを使用して設定できる `OAuthCodeExchangeContext` のプロパティです。 新しい `OAuthCodeExchangeContext` 型は、`OAuthHandler.ExchangeCodeAsync` に渡される唯一の引数です。

#### <a name="reason-for-change"></a>変更理由

この変更により、追加のパラメーターを中断することなく提供できます。 新しい `ExchangeCodeAsync` オーバーロードを作成する必要はありません。

#### <a name="recommended-action"></a>推奨アクション

適切な `code` と `redirectUri` の値を使用して、`OAuthCodeExchangeContext` を作成します。 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties> インスタンスを指定する必要があります。 この 1 つの `OAuthCodeExchangeContext` インスタンスは、複数の引数ではなく、`OAuthHandler.ExchangeCodeAsync` に渡すことができます。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthHandler%601.ExchangeCodeAsync(System.String,System.String)?displayProperty=nameWithType>

<!--

#### Affected APIs

`M:Microsoft.AspNetCore.Authentication.OAuth.OAuthHandler`1.ExchangeCodeAsync(System.String,System.String)`

-->
