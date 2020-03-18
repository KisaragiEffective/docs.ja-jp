---
ms.openlocfilehash: 1b4b0aba3ea24682ae972bf283ac387692c83781
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901738"
---
### <a name="http-defaulthttpcontext-extensibility-removed"></a>HTTP:DefaultHttpContext の機能拡張の削除

ASP.NET Core 3.0 のパフォーマンスの改善の一環として、`DefaultHttpContext` の機能拡張が削除されました。 クラスは `sealed` になりました。 詳細については、[dotnet/aspnetcore#6504](https://github.com/dotnet/aspnetcore/pull/6504) を参照してください。

単体テストで `Mock<DefaultHttpContext>` を使用している場合は、代わりに `Mock<HttpContext>` を使用してください。

ディスカッションについては、[dotnet/aspnetcore#6534](https://github.com/dotnet/aspnetcore/issues/6534) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

クラスは `DefaultHttpContext` から派生できます。

#### <a name="new-behavior"></a>新しい動作

クラスは `DefaultHttpContext` から派生できません。

#### <a name="reason-for-change"></a>変更理由

機能拡張は `HttpContext` のプーリングを可能にするために当初提供されていましたが、不要な複雑さをもたらし、他の最適化の妨げとなっていました。

#### <a name="recommended-action"></a>推奨アクション

単体テストで `Mock<DefaultHttpContext>` を使用する場合、今後は代わりに `Mock<HttpContext>` を使用してください。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

<xref:Microsoft.AspNetCore.Http.DefaultHttpContext?displayProperty=nameWithType>

<!--

#### Affected APIs

`T:Microsoft.AspNetCore.Http.DefaultHttpContext`

-->
