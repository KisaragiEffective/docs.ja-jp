---
ms.openlocfilehash: 15ba678431b97e7c961c119d83546569bdf9bad2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74282529"
---
### <a name="http-some-cookie-samesite-defaults-changed-to-none"></a>HTTP:SameSite の cookie オプションの既定値が一部、None に変更されました

`SameSite` は、一部のクロスサイト リクエスト フォージェリ (CSRF) 攻撃の軽減に貢献する cookie のオプションです。 このオプションが初めて導入されたとき、さまざまな ASP.NET Core API で使用されていた既定値に整合性がありませんでした。 整合性がないことで、結果は混乱を招きました。 ASP.NET Core 3.0 より、この既定値の整合性が改善されました。 この機能はコンポーネントごとに選択する必要があります。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

同様の ASP.NET Core API で異なる既定 <xref:Microsoft.AspNetCore.Http.SameSiteMode> 値が使用されました。 不整合の例は `HttpResponse.Cookies.Append(String, String)` と `HttpResponse.Cookies.Append(String, String, CookieOptions)` で確認できます。それぞれ、既定値は `SameSiteMode.None` と `SameSiteMode.Lax` でした。

#### <a name="new-behavior"></a>新しい動作

影響を受ける API の既定値はすべて `SameSiteMode.None` になります。

#### <a name="reason-for-change"></a>変更理由

既定値が変更され、`SameSite` がオプトイン機能になりました。

#### <a name="recommended-action"></a>推奨アクション

cookie を出す各コンポーネントでは、そのシナリオに `SameSite` が適切かどうかを決定する必要があります。 影響を受ける API の使用状況を確認し、必要に応じて `SameSite` を再構成します。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

- <xref:Microsoft.AspNetCore.Http.IResponseCookies.Append(System.String,System.String,Microsoft.AspNetCore.Http.CookieOptions)?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy%2A?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:Microsoft.AspNetCore.Http.IResponseCookies.Append(System.String,System.String,Microsoft.AspNetCore.Http.CookieOptions)`
- `Overload:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy`

-->
