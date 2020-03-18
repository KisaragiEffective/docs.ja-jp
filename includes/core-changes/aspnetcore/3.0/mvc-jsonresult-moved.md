---
ms.openlocfilehash: f6fd75c5b49156f44d31c650ea452eb549f13b0e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901614"
---
### <a name="mvc-jsonresult-moved-to-microsoftaspnetcoremvccore"></a>MVC:JsonResult は Microsoft.AspNetCore.Mvc.Core に移動されました

`JsonResult` は `Microsoft.AspNetCore.Mvc.Core` アセンブリに移動されました。 この型は、[Microsoft.AspNetCore.Mvc.Formatters.Json](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Json) に定義されていました。 大多数のユーザーのこの問題に対処するために、アセンブリ レベルの [[TypeForwardedTo]](xref:System.Runtime.CompilerServices.TypeForwardedToAttribute) 属性が `Microsoft.AspNetCore.Mvc.Formatters.Json` に追加されました。 サードパーティのライブラリを使用するアプリでは問題が発生する可能性があります。

#### <a name="version-introduced"></a>導入されたバージョン

3.0 Preview 6

#### <a name="old-behavior"></a>以前の動作

2\.2 ベースのライブラリを使用するアプリは正常にビルドされます。

#### <a name="new-behavior"></a>新しい動作

2\.2 ベースのライブラリを使用するアプリはコンパイルに失敗します。 次のテキストのバリエーションを含むエラーが表示されます。

```
The type 'JsonResult' exists in both 'Microsoft.AspNetCore.Mvc.Core, Version=3.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60' and 'Microsoft.AspNetCore.Mvc.Formatters.Json, Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'
```

このような問題の例については、[dotnet/aspnetcore#7220](https://github.com/dotnet/aspnetcore/issues/7220) を参照してください。

#### <a name="reason-for-change"></a>変更理由

[aspnet/Announcements#325](https://github.com/aspnet/Announcements/issues/325) で説明されているように、ASP.NET Core の構成に対するプラットフォーム レベルの変更。

#### <a name="recommended-action"></a>推奨される操作

2\.2 バージョンの `Microsoft.AspNetCore.Mvc.Formatters.Json` に対してコンパイルされたライブラリでは、すべてのコンシューマーの問題に対処するために再コンパイルが必要になる場合があります。 影響を受ける場合は、ライブラリの作成者にお問い合わせください。 ASP.NET Core 3.0 を対象とするライブラリの再コンパイルを要請してください。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

<xref:Microsoft.AspNetCore.Mvc.JsonResult?displayProperty=nameWithType>

<!-- 

### Affected APIs

`T:Microsoft.AspNetCore.Mvc.JsonResult`

-->
