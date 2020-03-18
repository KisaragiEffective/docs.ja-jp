---
title: IPv6 アドレス指定
ms.date: 03/30/2017
helpviewer_keywords:
- Internet Protocol version 6, addresses in
- Neighbor Discovery
- IPv6, enabling
- IPv6, mobility and
- Internet Protocol version 6, anycast addresses
- IPv6, Neighbor Discovery
- Internet Protocol version 6, auto-configuring
- Internet Protocol version 6, enabling
- IPv6, unicast addresses
- IPv6, auto-configuring
- Internet Protocol version 6, routing
- IPv6, RFC documents
- IPv6, routing
- Internet Protocol version 6, disabling
- Internet Protocol version 6, unicast addresses
- IPv6, multicast addresses
- Internet Protocol version 6, mobility and
- Internet Protocol version 6, RFC documents
- Internet Protocol version 6, Neighbor Discovery
- IPv6, anycast addresses
- Internet Protocol version 6, multicast addresses
- IPv6, addresses in
- IPv6, disabling
ms.assetid: 20a104ae-1649-4649-a005-531a5cf74c93
ms.openlocfilehash: 1bad43b96fc6f66724e5e40cdf0ae6d76b46d867
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71047850"
---
# <a name="ipv6-addressing"></a>IPv6 アドレス指定

インターネット プロトコル バージョン 6 (IPv6) では、アドレスの長さは 128 ビットです。 このような大きいアドレス空間の 1 つの理由は、インターネットのトポロジを反映したルーティング ドメインの階層構造に利用可能なアドレスを細分化するためです。 別の理由は、ネットワークにデバイスを接続するネットワーク アダプター (インターフェイス) のアドレスをマップするためです。 IPv6 は、ネットワーク インターフェイス レベルである最下位レベルでアドレスを解決する固有の機能、および自動構成機能を備えています。

## <a name="text-representation"></a>テキスト表現

IPv6 アドレスをテキスト文字列として表すために使用する 3 つの従来型形式を次に示します。

- **コロン 16 進数形式**。 これは、優先される形式 (n:n:n:n:n:n:n:n) です。 各 n は、アドレスの 8 つの 16 ビット要素の 1 つの 16 進数の値を表します。 たとえば、`3FFE:FFFF:7654:FEDA:1245:BA98:3210:4562` のように指定します。

- **圧縮形式**。 アドレスの長さのために、アドレスが 0 の長い文字列を含むのが一般的です。 これらのアドレスの記述を簡略化するために、0 ブロックの 1 つの連続するシーケンスがダブル コロン記号 (:) で表される圧縮形式を使用します。 この記号は、アドレスで 1 回だけ使用できます。 たとえば、マルチキャスト アドレス `FFED:0:0:0:0:BA98:3210:4562` の圧縮形式は `FFED::BA98:3210:4562` です。 ユニキャスト アドレス `3FFE:FFFF:0:0:8:800:20C4:0` の圧縮形式は `3FFE:FFFF::8:800:20C4:0` です。 ループバック アドレス `0:0:0:0:0:0:0:1` の圧縮形式は `::`1 です。 未指定アドレス `0:0:0:0:0:0:0:0` の圧縮形式は `::` です。

- **混在形式**。 この形式は、IPv4 アドレスと IPv6 アドレスを結合します。 この場合、アドレス形式は n:n:n:n:n:n:d.d.d.d です。ここで、各 n は 6 つの IPv6 上位 16 ビットのアドレス要素の 16 進数値を表し、各 d は、IPv4 アドレスの 10 進数値を表します。

## <a name="address-types"></a>アドレスの種類

アドレスの先頭のビットは、特定の IPv6 アドレスの種類を定義します。 これらの先頭のビットを含む可変長のフィールドは、フォーマット プレフィックス (FP) と呼ばれます。

IPv6 ユニキャスト アドレスは、2 つの部分に分かれています。 最初の部分には、アドレス プレフィックスが含まれており、2 番目の部分にはインターフェイス識別子が含まれています。 IPv6 アドレス/プレフィックスの組み合わせを表現するための簡潔な方法は、ipv6 アドレス/プレフィックス長です。

64 ビットのプレフィックスを使用するアドレスの例を次に示します。

[https://login.microsoftonline.com/consumers/](`3FFE:FFFF:0:CD30:0:0:0:0/64`)

この例のプレフィックスは `3FFE:FFFF:0:CD30` です。 アドレスは、`3FFE:FFFF:0:CD30::/64` のように圧縮形式で書き込むこともできます。

IPv6 では、次のアドレスの種類を定義します。

- **ユニキャスト アドレス**。 1 つのインターフェイスの識別子。 このアドレスに送信されるパケットは、識別されたインターフェイスに配信されます。 ユニキャスト アドレスは、上位のオクテットによってマルチキャスト アドレスと区別されます。 マルチキャスト アドレスの上位のオクテットには、FF の 16 進数値があります。 このオクテットの他のすべての値は、ユニキャスト アドレスであることを示します。 ユニキャスト アドレスの種類を次に示します。

  - **リンクローカル アドレス**。 これらのアドレスは 1 つのリンクで使用され、形式は FE80::*InterfaceID* です。 リンク ローカル アドレスは、自動アドレス構成、近隣探索、またはルーターが存在しない場合に、リンク上のノード間で使用されます。 リンク ローカル アドレスは、主に起動時に、システムが大きい範囲のアドレスをまだ取得していないときに使用されます。

  - **サイトローカル アドレス**。 これらのアドレスは、1 つのサイトで使用され、形式は FEC0::*SubnetID*:*InterfaceID* です。 サイト ローカル アドレスは、グローバル プレフィックスが必要ないサイト内でのアドレス指定に使用されます。

  - **グローバル IPv6 ユニキャスト アドレス**。 これらのアドレスは、インターネット経由で使用可能であり、形式は 010 (FP、3 ビット) TLA ID (13 ビット) 予約済み (8 ビット) NLA ID (24 ビット) SLA ID (16 ビット) *InterfaceID* (64 ビット) です。

- **マルチキャスト アドレス**。 (通常は別々のノードに属する) インターフェイスのセットの識別子。 このアドレスに送信されるパケットは、アドレスで識別されたすべてのインターフェイスに配信されます。 マルチキャスト アドレスの種類は、IPv4 ブロードキャスト アドレスよりも優先されます。

- **エニーキャスト アドレス**。 (通常は別々のノードに属する) インターフェイスのセットの識別子。 このアドレスに送信されるパケットは、アドレスで識別された 1 つのインターフェイスのみに配信されます。 これは、ルーティング メトリックで識別される最も近いインターフェイスです。 エニーキャスト アドレスは、ユニキャスト アドレス空間からが取得されるため、構文的に区別できません。 アドレス指定されたインターフェイスは、構成の機能として、ユニキャスト アドレスとエニーキャスト アドレスの区別を実行します。

一般に、ノードは常にリンク ローカル アドレスを持ちます。 サイト ローカル アドレスおよび 1 つまたは複数のグローバル アドレスを持つ場合があります。

## <a name="see-also"></a>参照

- [インターネット プロトコル バージョン 6](internet-protocol-version-6.md)
- [ソケット](sockets.md)
