---
ms.openlocfilehash: 8cb0aca991f5adfe4561ef56090cb9f7b2e56283
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75902057"
---
### <a name="localization-resourcemanagerwithculturestringlocalizer-and-withculture-marked-obsolete"></a>ローカリゼーション:ResourceManagerWithCultureStringLocalizer と WithCulture を古い形式としてマーク

[ResourceManagerWithCultureStringLocalizer](https://github.com/aspnet/Localization/blob/43b974482c7b703c92085c6f68b3b23d8fe32720/src/Microsoft.Extensions.Localization/ResourceManagerWithCultureStringLocalizer.cs#L18) クラスと [WithCulture](https://github.com/aspnet/Localization/blob/master/src/Microsoft.Extensions.Localization/ResourceManagerStringLocalizer.cs#L154-L170) インターフェイス メンバーは、特に、ユーザーが独自の `IStringLocalizer` 実装を作成するときに、ローカライズでユーザーの混乱の原因になることがよくあります。 これらの項目は、`IStringLocalizer` インスタンスが "言語ごとで、リソースごと" であるという印象をユーザーに与えています。 実際には、インスタンスは "リソースごと" のみである必要があります。 検索対象の言語は、実行時に `CultureInfo.CurrentUICulture` によって決定されます。 混乱の原因を排除するために、ASP.NET Core 3.0 Preview 3 では、API が古い形式としてマークされました。 これらの API は、将来のリリースで削除される予定です。

コンテキストについては、[dotnet/aspnetcore#3324](https://github.com/dotnet/aspnetcore/issues/3324) を参照してください。 ディスカッションについては、[dotnet/aspnetcore#7756](https://github.com/dotnet/aspnetcore/issues/7756) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

メソッドは `Obsolete` としてマークされていませんでした。

#### <a name="new-behavior"></a>新しい動作

メソッドは `Obsolete` としてマークされています。

#### <a name="reason-for-change"></a>変更理由

API が、推奨されていないユース ケースを表していました。 ローカライズの設計について混乱が生じていました。

#### <a name="recommended-action"></a>推奨アクション

代わりに `ResourceManagerStringLocalizer` を使用することをお勧めします。 カルチャは `CurrentCulture` によって設定できます。 これが適切でない場合は、[ResourceManagerWithCultureStringLocalizer](https://github.com/aspnet/Localization/blob/43b974482c7b703c92085c6f68b3b23d8fe32720/src/Microsoft.Extensions.Localization/ResourceManagerWithCultureStringLocalizer.cs#L18) のコピーを作成して使用してください。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

- <xref:Microsoft.Extensions.Localization.ResourceManagerWithCultureStringLocalizer>
- <xref:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer.WithCulture%2A?displayProperty=nameWithType>

<!--

#### Affected APIs

- `T:Microsoft.Extensions.Localization.ResourceManagerWithCultureStringLocalizer`
- `Overload:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer.WithCulture`

-->
