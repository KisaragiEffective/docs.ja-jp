---
ms.openlocfilehash: 5741e8cdd51e00d5459c4c1032a56682429aab17
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901999"
---
### <a name="mvc-pubternal-types-changed-to-internal"></a>MVC:"pubternal" 型を internal に変更

ASP.NET Core 3.0 では、MVC のすべての "pubternal" 型が、サポートされている名前空間で `public` になるか、必要に応じて `internal` になるように更新されました。

#### <a name="change-description"></a>変更の説明

ASP.NET Core では、"pubternal" 型は `public` として宣言されますが、`.Internal` サフィックスが付加された名前空間に存在します。 これらの型は `public` ですが、サポート ポリシーがなく、破壊的変更の対象となります。 残念ながら、これらの型が誤って使用されることが多かったため、これらのプロジェクトに破壊的変更が加えられ、フレームワークを維持する機能が制限されることになりました。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

MVC の一部の型は `public` でしたが、`.Internal` 名前空間に存在していました。 これらの型にはサポート ポリシーがなく、破壊的変更の対象となりました。

#### <a name="new-behavior"></a>新しい動作

このような型はすべて、サポートされている名前空間で `public` になるか、`internal` としてマークされるように更新されます。

#### <a name="reason-for-change"></a>変更理由

"pubternal" 型が誤って使用されることが多かったため、これらのプロジェクトに破壊的変更が加えられ、フレームワークを維持する機能が制限されることになりました。

#### <a name="recommended-action"></a>推奨アクション

真に `public` になり、サポートされている新しい名前空間に移動された型を使用する場合は、新しい名前空間に一致するように参照を更新してください。

`internal` としてマークされるようになった型を使用する場合は、代替を見つける必要があります。 以前の "pubternal" 型は、一般的な使用ではサポートされていませんでした。 これらの名前空間に、アプリにとって重要な特定の型が存在する場合は、[dotnet/aspnetcore](https://github.com/dotnet/aspnetcore/issues) で問題を報告してください。 要求された型を `public` にするために検討される可能性があります。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

この変更には、次の名前空間の型が含まれます。

- `Microsoft.AspNetCore.Mvc.Cors.Internal`
- `Microsoft.AspNetCore.Mvc.DataAnnotations.Internal`
- `Microsoft.AspNetCore.Mvc.Formatters.Internal`
- `Microsoft.AspNetCore.Mvc.Formatters.Json.Internal`
- `Microsoft.AspNetCore.Mvc.Formatters.Xml.Internal`
- `Microsoft.AspNetCore.Mvc.Internal`
- `Microsoft.AspNetCore.Mvc.ModelBinding.Internal`
- `Microsoft.AspNetCore.Mvc.Razor.Internal`
- `Microsoft.AspNetCore.Mvc.RazorPages.Internal`
- `Microsoft.AspNetCore.Mvc.TagHelpers.Internal`
- `Microsoft.AspNetCore.Mvc.ViewFeatures.Internal`

<!--

#### Affected APIs

- `N:Microsoft.AspNetCore.Mvc.Cors.Internal`
- `N:Microsoft.AspNetCore.Mvc.DataAnnotations.Internal`
- `N:Microsoft.AspNetCore.Mvc.Formatters.Internal`
- `N:Microsoft.AspNetCore.Mvc.Formatters.Json.Internal`
- `N:Microsoft.AspNetCore.Mvc.Formatters.Xml.Internal`
- `N:Microsoft.AspNetCore.Mvc.Internal`
- `N:Microsoft.AspNetCore.Mvc.ModelBinding.Internal`
- `N:Microsoft.AspNetCore.Mvc.Razor.Internal`
- `N:Microsoft.AspNetCore.Mvc.RazorPages.Internal`
- `N:Microsoft.AspNetCore.Mvc.TagHelpers.Internal`
- `N:Microsoft.AspNetCore.Mvc.ViewFeatures.Internal`

-->
