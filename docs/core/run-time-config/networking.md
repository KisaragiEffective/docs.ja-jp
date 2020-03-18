---
title: ネットワークの構成設定
description: .NET Core アプリのネットワークを構成するランタイム設定について説明します。
ms.date: 11/27/2019
ms.topic: reference
ms.openlocfilehash: 622c4afbd36d3d9004edbd9219145fa9e5d326ae
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74998835"
---
# <a name="run-time-configuration-options-for-networking"></a>ネットワークのランタイム構成オプション

## <a name="http2-protocol"></a>HTTP/2 プロトコル

- HTTP/2 プロトコルのサポートを有効にするかどうかを構成します。
- 既定:無効 (`false`)。
- .NET Core 3.0 で導入されました。

| | 設定の名前 | 値 |
| - | - |
| **runtimeconfig.json** | `System.Net.Http.SocketsHttpHandler.Http2Support` | `false` - 無効<br/>`true` - 有効 |
| **環境変数** | `DOTNET_SYSTEM_NET_HTTP_SOCKETSHTTPHANDLER_HTTP2SUPPORT` | `0` - 無効<br/>`1` - 有効 |

## <a name="sockets-http-handler"></a>ソケットの HTTP ハンドラー

- <xref:System.Net.Http.HttpClient> などのハイレベル ネットワーク API で <xref:System.Net.Http.SocketsHttpHandler?displayProperty=nameWithType> を使用するか、または [libcurl](https://curl.haxx.se/libcurl/)に基づく <xref:System.Net.Http.HttpClientHandler?displayProperty=nameWithType> の実装を使用するかを構成します。
- 既定:<xref:System.Net.Http.SocketsHttpHandler?displayProperty=nameWithType> を使用します (`true`)。
- この設定は、<xref:System.AppContext.SetSwitch%2A?displayProperty=nameWithType> メソッドを呼び出すことによりプログラムで構成できます。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | `System.Net.Http.UseSocketsHttpHandler` | `true` - <xref:System.Net.Http.SocketsHttpHandler> の使用を有効にする<br/>`false` - <xref:System.Net.Http.HttpClientHandler> の使用を有効にする |
| **環境変数** | `DOTNET_SYSTEM_NET_HTTP_USESOCKETSHTTPHANDLER` | `1` - <xref:System.Net.Http.SocketsHttpHandler> の使用を有効にする<br/>`0` - <xref:System.Net.Http.HttpClientHandler> の使用を有効にする |
