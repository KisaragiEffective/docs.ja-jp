---
ms.openlocfilehash: 2c1362d6982206b14475f77700add0bae61da173
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75902007"
---
### <a name="caching-compactonmemorypressure-property-removed"></a>キャッシュ:CompactOnMemoryPressure プロパティが削除された

ASP.NET Core 3.0 のリリースでは、[古い MemoryCacheOptions API](https://github.com/dotnet/extensions/blob/dc5c593da7b72c82e6fe85abb91d03818f9b700c/src/Caching/Memory/src/MemoryCacheOptions.cs#L17-L18) が削除されました。

#### <a name="change-description"></a>変更の説明

この変更は、[aspnet/Caching#221](https://github.com/aspnet/Caching/issues/221) に対するフォローアップです。 ディスカッションについては、[dotnet/extensions#1062](https://github.com/dotnet/extensions/issues/1062) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

`MemoryCacheOptions.CompactOnMemoryPressure` プロパティを使用できました。

#### <a name="new-behavior"></a>新しい動作

`MemoryCacheOptions.CompactOnMemoryPressure` プロパティは削除されました。

#### <a name="reason-for-change"></a>変更理由

キャッシュを自動的に圧縮すると、問題が発生しました。 予期しない動作を避けるため、キャッシュは必要なときにのみ圧縮する必要があります。

#### <a name="recommended-action"></a>推奨アクション

キャッシュを圧縮するには、`MemoryCache` にダウンキャストし、必要に応じて `Compact` を呼び出します。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

<xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.CompactOnMemoryPressure%2A?displayProperty=nameWithType>

<!--

#### Affected APIs

`Overload:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.CompactOnMemoryPressure`

-->
