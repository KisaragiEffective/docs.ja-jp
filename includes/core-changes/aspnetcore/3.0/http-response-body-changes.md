---
ms.openlocfilehash: cd66317bc93343e03a73dec74dff534776ca42e4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73198484"
---
### <a name="http-response-body-infrastructure-changes"></a>HTTP:応答本文のインフラストラクチャの変更

HTTP 応答本文を支えるインフラストラクチャが変更されました。 `HttpResponse` を直接使用する場合は、コードを変更する必要はありません。 `HttpResponse.Body` をラップまたは置換する場合や `HttpContext.Features` にアクセスする場合は、以下をお読みください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

HTTP 応答本文に関連付けられた次の 3 つの API がありました。

- `IHttpResponseFeature.Body`
- `IHttpSendFileFeature.SendFileAsync`
- `IHttpBufferingFeature.DisableResponseBuffering`

#### <a name="new-behavior"></a>新しい動作

`HttpResponse.Body` を置き換えると、`IHttpResponseBodyFeature` 全体が、`StreamResponseBodyFeature` を使用して予想されるすべての API の既定の実装を提供する特定のストリームのラッパーに置き換えられます。 元のストリームに戻すと、この変更が元に戻されます。

#### <a name="reason-for-change"></a>変更理由

目的は、応答本文の API を単一の新しい機能インターフェイスに結合することです。

#### <a name="recommended-action"></a>推奨アクション

`IHttpResponseFeature.Body`、`IHttpSendFileFeature`、または `IHttpBufferingFeature` を以前に使用していた場所で `IHttpResponseBodyFeature` を使用します。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

- <xref:Microsoft.AspNetCore.Http.Features.IHttpBufferingFeature?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature.Body?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature?displayProperty=nameWithType>

<!-- 

#### Affected APIs

- `T:Microsoft.AspNetCore.Http.Features.IHttpBufferingFeature`
- `P:Microsoft.AspNetCore.Http.Features.IHttpResponseFeature.Body`
- `T:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature`

-->
