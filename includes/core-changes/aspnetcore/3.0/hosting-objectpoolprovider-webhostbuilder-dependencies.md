---
ms.openlocfilehash: 4d99d0b6e99a7a9b976cf11832b33ad3bdc6d299
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901686"
---
### <a name="hosting-objectpoolprovider-removed-from-webhostbuilder-dependencies"></a>ホスティング:ObjectPoolProvider が WebHostBuilder 依存関係から削除されました

ASP.NET Core の定額課金を増やすために、`ObjectPoolProvider` が主な依存関係のセットから削除されました。 `ObjectPoolProvider` に依存する特定のコンポーネント自体で、それが追加されるようになりました。

ディスカッションについては、[dotnet/aspnetcore#5944](https://github.com/dotnet/aspnetcore/issues/5944) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

`WebHostBuilder` によって、DI コンテナーに既定で `ObjectPoolProvider` が提供されます。

#### <a name="new-behavior"></a>新しい動作

`WebHostBuilder` によって、DI コンテナーに既定で `ObjectPoolProvider` が提供されなくなりました。

#### <a name="reason-for-change"></a>変更理由

この変更は、ASP.NET Core の定額課金を増やすために行われました。

#### <a name="recommended-action"></a>推奨アクション

コンポーネントに `ObjectPoolProvider` が必要な場合は、`IServiceCollection` を介して依存関係に追加する必要があります。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
