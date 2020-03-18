---
title: Peer Name Resolution Protocol
ms.date: 03/30/2017
ms.assetid: 11940511-c124-4d91-ae31-d4ed6e81ee58
ms.openlocfilehash: c8b7b2190349323bf212d816a77f5f7810f6ca2c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74428215"
---
# <a name="peer-name-resolution-protocol"></a>Peer Name Resolution Protocol
ピアツーピア環境において、ピアは特定の名前解決システムを使用して、互いのネットワーク上の場所 (アドレス、プロトコル、およびポート) をその名前や他の識別子から解決します。 これまで、ドメイン ネーム システム (DNS) でのピア名の解決は、本質的に一時的な接続やその他の不具合によって複雑化していました。  
  
 Microsoft® Windows® ピアツーピア ネットワーク プラットフォームは、ピア名解決プロトコル (PNRP) を使用してこの問題を解決します。PNRP は、セキュリティで保護されたスケーラブルかつ動的な名前登録および名前解決プロトコルで、当初 Windows XP 用に開発され、Windows Vista™ でアップグレードされました。 PNRP は、従来の名前解決システムとは機能が大きく異なり、アプリケーション開発者にとってエキサイティングかつ新しい可能性を開拓します。  
  
 PNRP によって、コンピューターまたはコンピューター上の個々のアプリケーションやサービスにピア名を適用できます。 ピア名解決では、アドレス、ポート、拡張ペイロードの名前解決が可能です。 このシステムには、フォールト トレランスがある、ボトルネックが存在しない、古いアドレスが返されない名前解決という利点があり、そのためこのプロトコルは、モバイル ユーザーを検索するための優れたソリューションになります。  
  
 セキュリティに関しては、セキュリティで保護するか、セキュリティで保護せずにピア名を公開できます。 PNRP は公開キーの暗号化を使用して、セキュリティで保護されたピア名のなりすましを防止します。コンピューターとサービスの両方に PNRP で名前を付けることができます。  
  
ピア名解決プロトコルには次の特性があります。  
  
- 分散型で、ほぼすべてサーバーレス。 サーバーはブートストラップ プロセスのみに必要です。  
  
- 第三者が関与しないセキュリティで保護された名前の公開。 DNS の名前公開とは異なり、PNRP では即時に名前を公開でき、金銭的コストがかかりません。  
  
- PNRP はリアルタイムで更新されるため、古いアドレスの解決が発生しません。  
  
- PNRP による名前の解決はコンピューター以外にも拡張でき、サービスの名前解決も可能です。  
  
## <a name="the-systemnetpeertopeer-namespace"></a>System.Net.PeerToPeer 名前空間  
  
- PNRP の機能は、.NET Framework Version 3.5 内で <xref:System.Net.PeerToPeer> 名前空間によって定義されます。 この機能は、使用可能な PNRP サービスにピア名を登録して解決するために使用できる型のセットを提供します。  
  
- (<xref:System.ServiceModel.PeerResolvers> 名前空間で提供される型を使用して、PNRP とカスタム ピア リゾルバーを作成し、インスタンス化することができます。)  
  
- 使用可能な PNRP サービスに名前を登録して解決するために使用する基本的な型は、次のとおりです。  
  
- <xref:System.Net.PeerToPeer.Cloud>: 使用可能な PNRP クラウドとそのスコープを説明する情報を定義します。  
  
- <xref:System.Net.PeerToPeer.PeerName>: クラウド内でピアを登録し、その後解決するために使用できるピア名を定義します。  
  
- <xref:System.Net.PeerToPeer.PeerNameRecord>: ピアの登録情報を含む、PNRP クラウド内のレコードを定義します。この情報は、ピアに接続できるネットワーク エンドポイントを含みます。  
  
- <xref:System.Net.PeerToPeer.PeerNameRegistration>: ピア名の登録を開始および停止するメソッドを含む、ピア名の登録プロセスを定義します。  
  
- <xref:System.Net.PeerToPeer.PeerNameResolver>: 解決の同期および非同期のメソッドなど、ピア名をネットワーク エンドポイントに解決するためのプロセスを定義します。  
  
## <a name="see-also"></a>参照

- <xref:System.ServiceModel.PeerResolvers>
- <xref:System.Net.PeerToPeer>
- [ネットワーク プログラミングのサンプル](network-programming-samples.md)

<!-- to-do: review sample links
- [PeerToPeer Technology Sample](https://go.microsoft.com/fwlink/?LinkID=179571)
-->
