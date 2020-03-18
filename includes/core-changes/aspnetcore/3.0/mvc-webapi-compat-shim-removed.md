---
ms.openlocfilehash: 75945e7ff26c1c6db891d12cef4c16ed210da6af
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72394400"
---
### <a name="mvc-web-api-compatibility-shim-removed"></a>MVC: Web API 互換性 shim を削除

ASP.NET Core 3.0 以降、`Microsoft.AspNetCore.Mvc.WebApiCompatShim` パッケージは使用できなくなりました。

#### <a name="change-description"></a>変更の説明

`Microsoft.AspNetCore.Mvc.WebApiCompatShim` (WebApiCompatShim) パッケージは、ASP.NET Core への既存の Web API 実装の移行を簡素化するために、ASP.NET Core と ASP.NET 4.x Web API 2 の部分的な互換性を提供します。 ただし、WebApiCompatShim を使用するアプリでは、ASP.NET Core の最近のリリースで提供される API 関連の機能の利点が得られません。 このような機能として、改善された Open API 仕様の生成、標準化されたエラー処理、クライアント コードの生成などがあります。 3\.0 での API の取り組みに重点を置くために、WebApiCompatShim が削除されました。 WebApiCompatShim を使用する既存のアプリは、新しい `[ApiController]` モデルに移行する必要があります。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="reason-for-change"></a>変更理由

Web API 互換性 shim は移行ツールでした。 このツールにより、ASP.NET Core に追加された新しい機能へのユーザー アクセスが制限されます。

#### <a name="recommended-action"></a>推奨アクション

この shim の使用を削除し、ASP.NET Core 自体の同様の機能に直接移行します。

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

<xref:Microsoft.AspNetCore.Mvc.WebApiCompatShim?displayProperty=fullName>

<!--

#### Affected APIs

N:Microsoft.AspNetCore.Mvc.WebApiCompatShim

-->
