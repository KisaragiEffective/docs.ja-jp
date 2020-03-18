---
ms.openlocfilehash: 06d5f48566c239e37355496c3f27163d952602c6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901825"
---
### <a name="kestrel-connection-adapters-removed"></a>Kestrel: 接続アダプターが削除されました

"pubternal" API を `public` に移すための移行の一環として、`IConnectionAdapter` の概念が Kestrel から削除されました。 接続アダプターは、接続ミドルウェア (ASP.NET Core パイプラインの HTTP ミドルウェアに似ていますが、下位レベルの接続用) で置き換えられつつあります。 HTTPS および接続のログ記録は接続アダプターから接続ミドルウェアに移されました。 それらの拡張メソッドは引き続きシームレスに動作しますが、実装の詳細が変更されています。

詳細については、[dotnet/aspnetcore#11412](https://github.com/dotnet/aspnetcore/pull/11412) を参照してください。 ディスカッションについては、[dotnet/aspnetcore#11475](https://github.com/dotnet/aspnetcore/issues/11475) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

Kestrel 拡張コンポーネントは `IConnectionAdapter` を使用して作成されていました。

#### <a name="new-behavior"></a>新しい動作

Kestrel 拡張コンポーネントは、[ミドルウェア](https://github.com/dotnet/aspnetcore/pull/11412/files#diff-89acc06acf1b2e96bbdb811ce523619f)として作成されます。

#### <a name="reason-for-change"></a>変更理由

この変更は、より柔軟な拡張性アーキテクチャを提供することを目的としています。

#### <a name="recommended-action"></a>推奨アクション

[ここに](https://github.com/dotnet/aspnetcore/pull/11412/files#diff-89acc06acf1b2e96bbdb811ce523619f)示すように、`IConnectionAdapter` のすべての実装を、新しいミドルウェア パターンを使用するように変換します。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

`Microsoft.AspNetCore.Server.Kestrel.Core.Adapter.Internal.IConnectionAdapter`

<!-- 

#### Affected APIs

`T:Microsoft.AspNetCore.Server.Kestrel.Core.Adapter.Internal.IConnectionAdapter`

-->
