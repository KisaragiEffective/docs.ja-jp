---
ms.openlocfilehash: 58dbb73902c0226fa81acf1a70de2160f406f6c6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901699"
---
### <a name="authorization-iauthorizationpolicyprovider-implementations-require-new-method"></a>承認:IAuthorizationPolicyProvider の実装には新しいメソッドが必要です

ASP.NET Core 3.0 では、`IAuthorizationPolicyProvider` に新しい `GetFallbackPolicyAsync` メソッドが追加されました。 このフォールバック ポリシーは、ポリシーが指定されていない場合に、承認ミドルウェアによって使用されます。

詳細については、[dotnet/aspnetcore#9759](https://github.com/dotnet/aspnetcore/pull/9759) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

`IAuthorizationPolicyProvider` の実装には、`GetFallbackPolicyAsync` メソッドは必要ありませんでした。

#### <a name="new-behavior"></a>新しい動作

`IAuthorizationPolicyProvider` の実装には、`GetFallbackPolicyAsync` メソッドが必要です。

#### <a name="reason-for-change"></a>変更理由

ポリシーが指定されていない場合に新しい `AuthorizationMiddleware` を使用にするには、新しいメソッドが必要でした。

#### <a name="recommended-action"></a>推奨アクション

`IAuthorizationPolicyProvider` の実装に `GetFallbackPolicyAsync` メソッドを追加します。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationPolicyProvider?displayProperty=fullName>

<!-- 

#### Affected APIs

`T:Microsoft.AspNetCore.Authorization.IAuthorizationPolicyProvider`

-->
