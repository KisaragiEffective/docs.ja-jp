---
ms.openlocfilehash: 6679e38aefa7d61ce430dc5375ff3b35c641ea27
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72394355"
---
### <a name="identity-signinasync-throws-exception-for-unauthenticated-identity"></a>ID:SignInAsync が認証されていない ID に対して例外をスロー

既定では、`SignInAsync` は、`IsAuthenticated` が `false` であるプリンシパル/ID に対して例外をスローします。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

`SignInAsync` は、`IsAuthenticated` が `false` である ID も含め、すべてのプリンシパル/ID を受け入れます。

#### <a name="new-behavior"></a>新しい動作

既定では、`SignInAsync` は、`IsAuthenticated` が `false` であるプリンシパル/ID に対して例外をスローします。 この動作を抑制する新しいフラグがありますが、既定の動作が変更されました。

#### <a name="reason-for-change"></a>変更理由

既定では、これらのプリンシパルは `[Authorize]` / `RequireAuthenticatedUser()` によって拒否されていたため、以前の動作には問題がありました。

#### <a name="recommended-action"></a>推奨アクション

ASP.NET Core 3.0 Preview 6 では、`AuthenticationOptions` の `RequireAuthenticatedSignIn` フラグが既定で `true` に設定されています。 以前の動作を復元するには、このフラグを `false` に設定します。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
