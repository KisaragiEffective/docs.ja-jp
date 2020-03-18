---
ms.openlocfilehash: 53d2c989120c92f4e2d18f50ce4b364bd4c9b604
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75901886"
---
### <a name="http-synchronous-io-disabled-in-all-servers"></a>HTTP:すべてのサーバーで同期 IO が無効になっています

ASP.NET Core 3.0 以降では、同期サーバー操作は既定で無効になっています。

#### <a name="change-description"></a>変更の説明

`AllowSynchronousIO` は、`HttpRequest.Body.Read`、`HttpResponse.Body.Write`、`Stream.Flush` などの同期 IO API を有効または無効にする各サーバーのオプションです。 これらの API は長い間、スレッドの枯渇とアプリのハングの原因になっていました。 ASP.NET Core 3.0 Preview 3 以降では、これらの同期操作は既定で無効になっています。

影響を受けるサーバー:

- Kestrel
- HttpSys
- IIS インプロセス
- TestServer

次のようなエラーが発生する可能性があります。

- `Synchronous operations are disallowed. Call ReadAsync or set AllowSynchronousIO to true instead.`
- `Synchronous operations are disallowed. Call WriteAsync or set AllowSynchronousIO to true instead.`
- `Synchronous operations are disallowed. Call FlushAsync or set AllowSynchronousIO to true instead.`

各サーバーには、この動作を制御する `AllowSynchronousIO` オプションがあり、そのすべての既定値は現在、`false` です。

この動作は、一時的な軽減策として、要求ごとにオーバーライドすることもできます。 次に例を示します。

```csharp
var syncIOFeature = HttpContext.Features.Get<IHttpBodyControlFeature>();
if (syncIOFeature != null)
{
    syncIOFeature.AllowSynchronousIO = true;
}
```

`TextWriter`、または `Dispose` で同期 API を呼び出す別のストリームで問題が発生した場合は、代わりに新しい `DisposeAsync` API を呼び出します。

ディスカッションについては、[dotnet/aspnetcore#7644](https://github.com/dotnet/aspnetcore/issues/7644) を参照してください。

#### <a name="version-introduced"></a>導入されたバージョン

3.0

#### <a name="old-behavior"></a>以前の動作

`HttpRequest.Body.Read`、`HttpResponse.Body.Write`、および `Stream.Flush` が既定で許可されていました。

#### <a name="new-behavior"></a>新しい動作

これらの同期 API は、既定では許可されません。

次のようなエラーが発生する可能性があります。

- `Synchronous operations are disallowed. Call ReadAsync or set AllowSynchronousIO to true instead.`
- `Synchronous operations are disallowed. Call WriteAsync or set AllowSynchronousIO to true instead.`
- `Synchronous operations are disallowed. Call FlushAsync or set AllowSynchronousIO to true instead.`

#### <a name="reason-for-change"></a>変更理由

これらの同期 API は長い間、スレッドの枯渇とアプリのハングの原因になっていました。 ASP.NET Core 3.0 Preview 3 以降では、同期操作は既定で無効になっています。

#### <a name="recommended-action"></a>推奨アクション

非同期バージョンのメソッドを使用します。 この動作は、一時的な軽減策として、要求ごとにオーバーライドすることもできます。

```csharp
var syncIOFeature = HttpContext.Features.Get<IHttpBodyControlFeature>();
if (syncIOFeature != null)
{
    syncIOFeature.AllowSynchronousIO = true;
}
```

#### <a name="category"></a>カテゴリ

ASP.NET Core

#### <a name="affected-apis"></a>影響を受ける API

- <xref:System.IO.Stream.Flush%2A?displayProperty=nameWithType>
- <xref:System.IO.Stream.Read%2A?displayProperty=nameWithType>
- <xref:System.IO.Stream.Write%2A?displayProperty=nameWithType>

<!--

#### Affected APIs

- `Overload:System.IO.Stream.Flush`
- `Overload:System.IO.Stream.Read`
- `Overload:System.IO.Stream.Write`

-->
