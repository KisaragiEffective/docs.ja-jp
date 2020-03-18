---
ms.openlocfilehash: c5e4b5619394f99a419fe48aee190ad741ea8c0d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73041660"
---
### <a name="identity-ui-uses-static-web-assets-feature"></a>ID:UI で静的な Web 資産機能を使用

ASP.NET Core 3.0 では静的な Web 資産機能が導入され、ID UI でこれが導入されました。

#### <a name="change-description"></a>変更の説明

ID UI で静的な Web 資産機能が導入された結果として、次のようになります。

- フレームワークの選択は、プロジェクト ファイルで `IdentityUIFrameworkVersion` プロパティを使用して行います。
- Bootstrap 4 が、ID UI の既定の UI フレームワークです。 Bootstrap 3 はサポートが終了したので、サポートされているバージョンへの移行を検討する必要があります。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

ID UI の既定の UI フレームワークは **Bootstrap 3** でした。 UI フレームワークは、`Startup.ConfigureServices` で `AddDefaultUI` メソッド呼び出しのパラメーターを使用して構成できました。

#### <a name="new-behavior"></a>新しい動作

ID UI の既定の UI フレームワークは **Bootstrap 4** です。 UI フレームワークは、`AddDefaultUI` メソッド呼び出し内ではなく、プロジェクト ファイル内で構成する必要があります。

#### <a name="reason-for-change"></a>変更理由

静的な Web 資産機能を導入するには、UI フレームワークの構成を MSBuild に移行する必要がありました。 埋め込むフレームワークは、実行時に決定されるのではなく、ビルド時に決定されます。

#### <a name="recommended-action"></a>推奨アクション

サイトの UI を見直して、新しい Bootstrap 4 コンポーネントに互換性があることを確認します。 必要に応じて、`IdentityUIFrameworkVersion` MSBuild プロパティを使用して Bootstrap 3 に戻します。 このプロパティを、プロジェクト ファイルの `<PropertyGroup>` 要素に追加します。

```xml
<IdentityUIFrameworkVersion>Bootstrap3</IdentityUIFrameworkVersion>
```

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

<xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI(Microsoft.AspNetCore.Identity.IdentityBuilder,Microsoft.AspNetCore.Identity.UI.UIFramework)?displayProperty=nameWithType>

<!-- 

#### Affected APIs

`M:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI(Microsoft.AspNetCore.Identity.IdentityBuilder,Microsoft.AspNetCore.Identity.UI.UIFramework)`

-->
