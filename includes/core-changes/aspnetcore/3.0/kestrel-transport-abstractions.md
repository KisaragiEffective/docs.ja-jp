---
ms.openlocfilehash: f95c3916f4da8164cf927344f60f2845f04ddc5c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72394045"
---
### <a name="kestrel-transport-abstractions-removed-and-made-public"></a>Kestrel: トランスポートの抽象化を削除して公開

"pubternal" API からの移行の一環として、Kestrel トランスポート層の API がパブリック インターフェイスとして `Microsoft.AspNetCore.Connections.Abstractions` ライブラリに公開されています。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

- トランスポート関連の抽象化は、`Microsoft.AspNetCore.Server.Kestrel.Transport.Abstractions` ライブラリで提供されていました。
- `ListenOptions.NoDelay` プロパティが使用可能でした。

#### <a name="new-behavior"></a>新しい動作

- `...Transport.Abstractions` ライブラリの最も使用されている機能を公開するために、`Microsoft.AspNetCore.Connections.Abstractions` ライブラリに `IConnectionListener` インターフェイスが導入されました。
- トランスポート オプション (`LibuvTransportOptions` および `SocketTransportOptions`) で、`NoDelay` が使用できるようになりました。
- `SchedulingMode` は使用できなくなりました。

#### <a name="reason-for-change"></a>変更理由

ASP.NET Core 3.0 は、"pubternal" API から移行しました。

#### <a name="recommended-action"></a>推奨アクション

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

None

<!-- 

### Affected APIs

Not detectable via API analysis

-->
