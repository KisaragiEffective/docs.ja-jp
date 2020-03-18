---
ms.openlocfilehash: 58b1190e3e6a3168d35700eed655f6756e076a29
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901942"
---
### <a name="mvc-async-suffix-trimmed-from-controller-action-names"></a>MVC:コントローラー アクション名から Async サフィックスを削除

[dotnet/aspnetcore#4849](https://github.com/dotnet/aspnetcore/issues/4849) への対処の一環として、ASP.NET Core MVC では、アクション名から `Async` サフィックスが既定で削除されます。 ASP.NET Core 3.0 以降、この変更はルーティングとリンク生成の両方に影響します。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

次の ASP.NET Core MVC コントローラーについて考えてみましょう。

```csharp
public class ProductController : Controller
{
    public async IActionResult ListAsync()
    {
        var model = await DbContext.Products.ToListAsync();
        return View(model);
    }
}
```

このアクションは、`Product/ListAsync` を介してルーティング可能です。 リンクの生成には、`Async` サフィックスを指定する必要があります。 次に例を示します。

```cshtml
<a asp-controller="Product" asp-action="ListAsync">List</a>
```

#### <a name="new-behavior"></a>新しい動作

ASP.NET Core 3.0 では、このアクションは `Product/List` を介してルーティング可能です。 リンク生成コードでは、`Async` サフィックスを省略する必要があります。 次に例を示します。

```cshtml
<a asp-controller="Product" asp-action="List">List</a>
```

この変更は、`[ActionName]` 属性を使用して指定された名前には影響しません。 新しい動作は、`Startup.ConfigureServices` で `MvcOptions.SuppressAsyncSuffixInActionNames` を `false` に設定することによって無効にすることができます。

```csharp
services.AddMvc(options =>
{
   options.SuppressAsyncSuffixInActionNames = false;
});
```

#### <a name="reason-for-change"></a>変更理由

規則に従い、非同期 .NET メソッドには `Async` サフィックスを付加します。 ただし、メソッドで MVC アクションを定義する場合、`Async` サフィックスを使用するのは望ましくありません。

#### <a name="recommended-action"></a>推奨アクション

アプリが、名前の `Async` サフィックスを保持する MVC アクションに依存している場合、次のいずれかの軽減策を選択します。

- `[ActionName]` 属性を使用して、元の名前を保持します。
- `Startup.ConfigureServices` で `MvcOptions.SuppressAsyncSuffixInActionNames` を `false` に設定して、名前の変更を完全に無効にします。

```csharp
services.AddMvc(options =>
{
   options.SuppressAsyncSuffixInActionNames = false;
});
```

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
