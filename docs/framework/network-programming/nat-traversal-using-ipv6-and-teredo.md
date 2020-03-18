---
title: IPv6 および Teredo を使用した NAT トラバーサル
ms.date: 03/30/2017
ms.assetid: 568cd245-3300-49ef-a995-d81bf845d961
ms.openlocfilehash: f617dc8912091576727b90da1e9efb9ebd5f9bda
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "61642179"
---
# <a name="nat-traversal-using-ipv6-and-teredo"></a>IPv6 および Teredo を使用した NAT トラバーサル
ネットワーク アドレス変換 (NAT) トラバーサルをサポートする機能強化が行われました。 これらの変更は、IPv6 および Teredo での使用を想定して設計されていますが、他の IP トンネリング テクノロジにも適用されます。 これらの拡張機能は、<xref:System.Net> および関連する名前空間のクラスに影響します。  
  
 これらの変更は、IP トンネリング テクノロジを使用するクライアント サーバー アプリケーションに影響を与える可能性があります。  
  
 NAT トラバーサルをサポートするための変更は、.NET Framework Version 4 を使用しているアプリケーションでのみ機能します。 これらの機能は、.NET Framework の以前のバージョンではご利用いただけません。  
  
## <a name="overview"></a>概要  
 Internet Protocol version 4 (IPv4) では、IPv4 アドレスが 32 ビット長として定義されました。 その結果、IPv4 は約 40 億個の一意の IP アドレスをサポートしています (2 ^32)。 1990 年代にインターネット上のコンピューターやネットワーク デバイスの数が拡大したため、IPv4 アドレス空間の逼迫が明らかになりました。  
  
 IPv4 の枯渇を遅らせるために採用された複数の手法の 1 つが、単一の一意なパブリック IP アドレスで多数のプライベート IP アドレス (プライベート イントラネット) を表すことができる NAT の展開でした。 NAT デバイスの背後にあるプライベート IP アドレスは、単一のパブリック IPv4 アドレスを共有しています。 専用ハードウェア デバイス (安価なワイヤレス アクセス ポイントとルーターなど) や、NAT を提供するサービスを実行しているコンピューターを NAT デバイスとすることができます。 このパブリック IP アドレスを提供するデバイスまたはサービスは、パブリック インターネットとプライベート イントラネットの間で IP ネットワーク パケットを変換します。  
  
 プライベート イントラネットで実行され、インターネット上の他の IP アドレス (通常はサーバー) に要求を送信するクライアント アプリケーションの場合、このスキームが効果的です。 NAT デバイスまたはサーバーはクライアント要求のマッピングを維持しており、応答が返されたときに応答の送信先を認識できます。 一方、NAT デバイスの背後のプライベート イントラネットで実行され、サービスを提供し、パケットをリッスンして応答するアプリケーションの場合、このスキームには問題があります。 ピアツーピア アプリケーションの場合は特に顕著です。  
  
 IPv6 プロトコルは、IPv4 アドレスを 128 ビット長と定義しています。 その結果、IPv6 は、3.2 x 10^38 個の一意のアドレス (2 ^128) が使用できる非常に大規模な IP アドレス空間をサポートしています。 このサイズのアドレス空間があるため、インターネットに接続されたすべてのデバイスに、それぞれ固有のアドレスを付与することができます。 ところが、これには問題があります。 世界の大部分では、まだ IPv4 だけを使用しています。 具体的には、小規模な企業、組織、家庭で使用されている既存のルーターやワイヤレス アクセス ポイントは、IPv6 をサポートしていません。 それらの顧客にサービスを提供する一部のインターネット サービス プロバイダーも、IPv6 をサポートしていないか、IPv6 のサポートを構成に組み込んでいません。  
  
 IPv4 パケットで IPv6 アドレスをトンネリングするために、複数の IPv6 移行テクノロジが開発されています。 そのようなテクノロジとして、6to4、ISATAP、Teredo のトンネルがあります。これらのテクノロジは、IPv6 ホストが他の IPv6 ネットワークに到達するために IPv4 ネットワークを経由する必要があるときに、ユニキャスト IPv6 トラフィックに対するアドレス割り当てとホスト間の自動トンネリングを提供します。 IPv6 パケットは、IPv4 パケットとしてトンネリングされて送信されます。 NAT デバイスを介した IPv6 アドレスの NAT トラバーサルを可能にする複数のトンネリング手法が使用されています。  
  
 Teredo は、IPv4 ネットワークで IPv6 接続を実現する IPv6 移行テクノロジの 1 つです。 Teredo は、インターネット技術標準化委員会 (IETF) が公開している RFC 4380 で規定されています。 Windows XP SP2 以降では、2001:0::/32 の範囲でパブリック IPv6 アドレスを提供する仮想 Teredo アダプターがサポートされています。 この IPv6 アドレスは、インターネットからの着信接続をリッスンするために使用でき、リッスンしているサービスに接続しようとする IPv6 対応クライアントに提供できます。 これにより、IPv6 Teredo アドレスを使用してそのコンピューターに接続できるようになるため、アプリケーションは NAT デバイスの背後にあるコンピューターのアドレスを指定する方法について考慮する必要がなくなります。  
  
## <a name="enhancements-to-support-nat-traversal-and-teredo"></a>NAT トラバーサルと Teredo をサポートするための機能強化  
 IPv6 および Teredo を使用した NAT トラバーサルをサポートするために、<xref:System.Net>、<xref:System.Net.NetworkInformation>、および <xref:System.Net.Sockets> 名前空間に強化機能が追加されました。  
  
 <xref:System.Net.NetworkInformation.IPGlobalProperties?displayProperty=nameWithType> クラスに、ホスト上のユニキャスト IP アドレスの一覧を取得するための複数のメソッドが追加されています。 <xref:System.Net.NetworkInformation.IPGlobalProperties.BeginGetUnicastAddresses%2A> メソッドは、ローカル コンピューター上の安定したユニキャスト IP アドレス テーブルを取得するための非同期要求を開始します。 <xref:System.Net.NetworkInformation.IPGlobalProperties.EndGetUnicastAddresses%2A> メソッドは、ローカル コンピューター上の安定したユニキャスト IP アドレス テーブルを取得するための保留中の非同期要求を終了します。 <xref:System.Net.NetworkInformation.IPGlobalProperties.GetUnicastAddresses%2A> メソッドは、ローカル コンピューター上の安定したユニキャスト IP アドレス テーブルを取得するための同期要求であり、アドレス テーブルが安定化されるまで必要に応じて待機します。  
  
 <xref:System.Net.IPAddress.IsIPv6Teredo%2A?displayProperty=nameWithType> プロパティは、<xref:System.Net.IPAddress> が IPv6 Teredo アドレスであるかどうかを確認するために使用できます。  
  
 これらの新しい <xref:System.Net.NetworkInformation.IPGlobalProperties> クラス メソッドを <xref:System.Net.IPAddress.IsIPv6Teredo%2A> プロパティと組み合わせて使用することで、アプリケーションは Teredo アドレスを簡単に見つけることができます。 通常、アプリケーションが知る必要があるのは、ローカルの Teredo アドレスだけです (アプリケーションがこの情報をリモート アプリケーションに送信する場合)。 たとえば、ピアツーピア アプリケーションがすべての IPv6 アドレスをマッチメイキング サーバーに送信し、サーバーが受け取ったアドレスを他のピアに転送して直接通信ができるようにする場合です。  
  
 アプリケーションは通常、ローカルの Teredo アドレスではなく <xref:System.Net.IPAddress.IPv6Any?displayProperty=nameWithType> でリッスンするようにリッスン サービスを設定する必要があります。 そのため、リモート クライアントまたはピアが、リッスンしているサービスのホストへの直接的な IPv6 ルートを持っている場合、クライアントまたはピアは、IPv6 を使用して直接接続することができ、Teredo を使用してパケットをトンネリングする必要はありません。  
  
 TCP アプリケーションの場合、<xref:System.Net.Sockets.TcpListener?displayProperty=nameWithType> クラスに、NAT トラバーサルを有効にする <xref:System.Net.Sockets.TcpListener.AllowNatTraversal%2A> メソッドがあります。 UDP アプリケーションの場合、<xref:System.Net.Sockets.UdpClient?displayProperty=nameWithType> クラスに、NAT トラバーサルを有効にする <xref:System.Net.Sockets.UdpClient.AllowNatTraversal%2A> メソッドがあります。  
  
 <xref:System.Net.Sockets.Socket?displayProperty=nameWithType> および関連したクラスを使用するアプリケーションでは、<xref:System.Net.Sockets.Socket.GetSocketOption%2A> メソッドと <xref:System.Net.Sockets.Socket.SetSocketOption%2A> メソッドを <xref:System.Net.Sockets.SocketOptionName.IPProtectionLevel?displayProperty=nameWithType> ソケット オプションと共に使用して、NAT トラバーサルのクエリ、有効化、または無効化を行うことができます。  
  
## <a name="see-also"></a>参照

- <xref:System.Net.IPAddress.IsIPv6Teredo%2A?displayProperty=nameWithType>
- <xref:System.Net.NetworkInformation.IPGlobalProperties.BeginGetUnicastAddresses%2A?displayProperty=nameWithType>
- <xref:System.Net.NetworkInformation.IPGlobalProperties.EndGetUnicastAddresses%2A?displayProperty=nameWithType>
- <xref:System.Net.NetworkInformation.IPGlobalProperties.GetUnicastAddresses%2A?displayProperty=nameWithType>
- <xref:System.Net.Sockets.IPProtectionLevel?displayProperty=nameWithType>
- <xref:System.Net.Sockets.Socket.SetIPProtectionLevel%2A?displayProperty=nameWithType>
- <xref:System.Net.Sockets.TcpListener.AllowNatTraversal%2A?displayProperty=nameWithType>
- <xref:System.Net.Sockets.UdpClient.AllowNatTraversal%2A?displayProperty=nameWithType>
