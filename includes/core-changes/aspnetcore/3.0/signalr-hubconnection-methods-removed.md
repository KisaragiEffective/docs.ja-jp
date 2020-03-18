---
ms.openlocfilehash: de06825f1031d05bc83183a83bae165e2f9512ff
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901874"
---
### <a name="signalr-hubconnection-resetsendping-and-resettimeout-methods-removed"></a>SignalR: HubConnection の ResetSendPing メソッドと ResetTimeout メソッドが削除された

`ResetSendPing` メソッドと `ResetTimeout` メソッドが、SignalR の `HubConnection` API から削除されました。 これらのメソッドはもともと内部使用のみを目的としていましたが、ASP.NET Core 2.2 で公開されました。 これらのメソッドは、ASP.NET Core 3.0 Preview 4 リリース以降では使用できません。 ディスカッションについては、[dotnet/aspnetcore#8543](https://github.com/dotnet/aspnetcore/issues/8543) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

API を使用できました。

#### <a name="new-behavior"></a>新しい動作

API は削除されています。

#### <a name="reason-for-change"></a>変更理由

これらのメソッドはもともと内部使用のみを目的としていましたが、ASP.NET Core 2.2 で公開されました。

#### <a name="recommended-action"></a>推奨アクション

これらのメソッドは使用しないでください。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

- <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.ResetSendPing?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.ResetTimeout?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:Microsoft.AspNetCore.SignalR.Client.HubConnection.ResetSendPing`
- `M:Microsoft.AspNetCore.SignalR.Client.HubConnection.ResetTimeout`

-->
