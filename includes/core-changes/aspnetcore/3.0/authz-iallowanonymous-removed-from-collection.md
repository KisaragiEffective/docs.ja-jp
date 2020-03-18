---
ms.openlocfilehash: 0c88d40e34d2d6458bb463a09d716dea42b711fe
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901799"
---
### <a name="authorization-iallowanonymous-removed-from-authorizationfiltercontextfilters"></a>承認:IAllowAnonymous が AuthorizationFilterContext.Filters から削除されました

ASP.NET Core 3.0 時点で、MVC は、コントローラーとアクション メソッドで検出された [[AllowAnonymous]](xref:Microsoft.AspNetCore.Authorization.AllowAnonymousAttribute) 属性に対して [AllowAnonymousFilters](xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter) を追加しません。 この変更は、<xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> の派生物についてはローカルに対処されますが、<xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAuthorizationFilter> と <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> の実装では破壊的変更です。 [[TypeFilter]](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute) 属性でラップするこのような実装は、構成と依存関係の挿入の両方が必要な場合に、厳密に型指定された属性ベースの承認を実現するための[一般的](https://stackoverflow.com/a/41348219/608220)でサポートされる方法です。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

<xref:Microsoft.AspNetCore.Authorization.IAllowAnonymous> が [AuthorizationFilterContext.Filters](xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext.Filters%2A) コレクションに表示されました。 このインターフェイスの存在をテストすることは、個々のコントローラー メソッドでフィルターをオーバーライドまたは無効にするための有効なアプローチでした。

#### <a name="new-behavior"></a>新しい動作

`IAllowAnonymous` は `AuthorizationFilterContext.Filters` コレクションに表示されなくなりました。 以前の動作に依存する `IAsyncAuthorizationFilter` の実装では、通常、HTTP 401 権限なしまたは HTTP 403 禁止応答が断続的に発生します。

#### <a name="reason-for-change"></a>変更理由

新しいエンドポイント ルーティング戦略が ASP.NET Core 3.0 で導入されました。

#### <a name="recommended-action"></a>推奨アクション

エンドポイント メタデータで `IAllowAnonymous`を検索します。 次に例を示します。

```csharp
var endpoint = context.HttpContext.GetEndpoint();
if (endpoint?.Metadata?.GetMetadata<IAllowAnonymous>() != null)
{
}
```

この手法の例については、[こちらの HasAllowAnonymous メソッド](https://github.com/dotnet/aspnetcore/blob/bd65275148abc9b07a3b59797a88d485341152bf/src/Mvc/Mvc.Core/src/Authorization/AuthorizeFilter.cs#L236)を参照してください。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

None

<!--

#### Affected APIs

Not detectable via API analysis

-->
