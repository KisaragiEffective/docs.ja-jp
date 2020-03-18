---
ms.openlocfilehash: b0e1d6d720a1c9b827fb4585606e64b545d395d7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72394374"
---
### <a name="kestrel-request-trailer-headers-moved-to-new-collection"></a>Kestrel: 要求トレーラー ヘッダーを新しいコレクションに移動

以前のバージョンでは、要求本文が最後まで読み取られると、Kestrel によって HTTP/1.1 チャンク トレーラー ヘッダーが要求ヘッダー コレクションに追加されていました。 この動作により、ヘッダーとトレーラー間のあいまいさに関する懸念が生じていました。 トレーラーを新しいコレクションに移動することが決定されました。

HTTP/2 要求トレーラーは ASP.NET Core 2.2 では使用できませんでしたが、ASP.NET Core 3.0 ではこの新しいコレクションでも使用できるようになりました。

これらのトレーラーにアクセスするために、新しい要求拡張メソッドが追加されました。

HTTP/1.1 トレーラーは、要求本文全体が読み取られると利用可能になります。

HTTP/2 トレーラーは、クライアントから受信すると利用可能になります。 クライアントは、要求本文全体がサーバーによって少なくともバッファーされるまで、トレーラーを送信しません。 バッファー領域を解放するために、要求本文を読み取ることが必要な場合があります。 要求本文を最後まで読み取ると、トレーラーをいつでも使用できます。 トレーラーは本文の終わりをマークします。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

要求トレーラー ヘッダーは、`HttpRequest.Headers` コレクションに追加されます。

#### <a name="new-behavior"></a>新しい動作

要求トレーラー ヘッダーは、`HttpRequest.Headers` コレクションには**存在しません**。 トレーラー ヘッダーにアクセスするには、`HttpRequest` で次の拡張メソッドを使用します。

- `GetDeclaredTrailers()` - 本文の後に予想されるトレーラーを示す、要求の "Trailer" ヘッダーを取得します。
- `SupportsTrailers()` - 要求でトレーラー ヘッダーの受信がサポートされているかどうかを示します。
- `CheckTrailersAvailable()` - 要求でトレーラーがサポートされているかどうかと、それらを読み取りに使用できるかどうかを確認します。
- `GetTrailer(string trailerName)` - 要求された末尾のヘッダーを応答から取得します。

#### <a name="reason-for-change"></a>変更理由

トレーラーは、gRPC などのシナリオにおける重要な機能です。 要求ヘッダーへのトレーラーのマージは、ユーザーの混乱を招いていました。

#### <a name="recommended-action"></a>推奨アクション

トレーラーにアクセスするには、`HttpRequest` でトレーラー関連の拡張メソッドを使用します。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

<xref:Microsoft.AspNetCore.Http.HttpRequest.Headers?displayProperty=nameWithType>

<!--

#### Affected APIs

`P:Microsoft.AspNetCore.Http.HttpRequest.Headers`

-->
