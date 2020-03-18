---
ms.openlocfilehash: b91cdc7a0d2e4258662155a840500ce21ab35760
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74101175"
---
### <a name="authorization-addauthorization-overload-moved-to-different-assembly"></a>承認:AddAuthorization のオーバーロードが別のアセンブリに移動した

`Microsoft.AspNetCore.Authorization` に以前あったコア `AddAuthorization` メソッドが `AddAuthorizationCore` に名前変更されました。 古い `AddAuthorization` メソッドはまだありますが、代わりに `Microsoft.AspNetCore.Authorization.Policy` アセンブリに含まれています。 両方のメソッドを使用するアプリには影響はありません。 `Microsoft.AspNetCore.Authorization.Policy` は、「[共有フレームワーク: Microsoft.AspNetCore.App から削除されたアセンブリ](#shared-framework-assemblies-removed-from-microsoftaspnetcoreapp)」に説明されているスタンドアロン パッケージではなく共有フレームワークで出荷されるようになりました。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作
`AddAuthorization` メソッドは `Microsoft.AspNetCore.Authorization` にありました。

#### <a name="new-behavior"></a>新しい動作

`AddAuthorization` メソッドは `Microsoft.AspNetCore.Authorization.Policy` にあります。 `AddAuthorizationCore` は、古いメソッドの新しい名前です。

#### <a name="reason-for-change"></a>変更理由

`AddAuthorization` は、承認に必要な一般的なすべてのサービスを追加するのにより適切なメソッド名です。

#### <a name="recommended-action"></a>推奨アクション

`Microsoft.AspNetCore.Authorization.Policy` に参照を追加するか、代わりに `AddAuthorizationCore` を使用します。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

<xref:Microsoft.Extensions.DependencyInjection.AuthorizationServiceCollectionExtensions.AddAuthorization(Microsoft.Extensions.DependencyInjection.IServiceCollection,System.Action{Microsoft.AspNetCore.Authorization.AuthorizationOptions})?displayProperty=fullName>

<!--

#### Affected APIs

`M:Microsoft.Extensions.DependencyInjection.AuthorizationServiceCollectionExtensions.AddAuthorization(Microsoft.Extensions.DependencyInjection.IServiceCollection,System.Action{Microsoft.AspNetCore.Authorization.AuthorizationOptions})`

-->
