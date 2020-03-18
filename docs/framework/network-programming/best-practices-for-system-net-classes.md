---
title: System.Net クラスのベスト プラクティス
ms.date: 03/30/2017
helpviewer_keywords:
- sending data, best practices
- requesting data from Internet, best practices
- WebRequest class, best practices
- data requests, best practices
- WebResponse class, best practices
- best practices, data requests
- receiving data, best practices
ms.assetid: 716decc6-5952-47b7-9c5a-ba6fc5698684
ms.openlocfilehash: c7324dcbc27c95c7d799592700d46c195e7d952b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71048895"
---
# <a name="best-practices-for-systemnet-classes"></a>System.Net クラスのベスト プラクティス
次の推奨事項は、<xref:System.Net> に含まれるクラスを最大限に活用するのに役立ちます。  
  
- トランスポート層セキュリティ (TLS) のベスト プラクティスについては、「[.NET Framework でのトランスポート層セキュリティ (TLS) のベスト プラクティス](tls.md)」をご覧ください。

- 派生クラスに型キャストするのではなく、可能な限り、<xref:System.Net.WebRequest> および <xref:System.Net.WebResponse> を使用します。 **WebRequest** と **WebResponse** を使用するアプリケーションでは、コードを大幅に変更せずに新しいインターネット プロトコルを利用できます。  
  
- **System.Net** クラスを使用するサーバー上で実行する ASP.NET アプリケーションを作成する場合、一般に、パフォーマンスを考慮して、非同期メソッドを <xref:System.Net.WebRequest.GetResponse%2A> および <xref:System.Net.WebResponse.GetResponseStream%2A> に対して使用することをお勧めします。  
  
- インターネット リソースに対して開いている接続の数が、ネットワークのパフォーマンスやスループットに大きく影響する場合があります。 既定では、**System.Net** は、各ホストのアプリケーションごとに 2 つの接続を使用します。 アプリケーションの <xref:System.Net.ServicePoint.ConnectionLimit%2A> に <xref:System.Net.ServicePoint> プロパティを設定すると、特定のホストでこの数を増やすことができます。 <xref:System.Net.ServicePointManager.DefaultPersistentConnectionLimit?displayProperty=nameWithType> プロパティを設定すると、すべてのホストでこの既定値を増やすことができます。  
  
- ソケット レベルのプロトコルを作成する場合は、<xref:System.Net.Sockets.TcpClient>に直接書き込むのではなく、可能な限り、<xref:System.Net.Sockets.UdpClient> または <xref:System.Net.Sockets.Socket> を使用するようにしてください。 これら 2 つのクライアント クラスは TCP ソケットおよび UDP ソケットの作成をカプセル化するため、接続の詳細を処理する必要がなくなります。  
  
- 資格情報を必要とするサイトにアクセスする場合、要求ごとに資格情報を入力するのではなく、<xref:System.Net.CredentialCache> クラスを使用して資格情報のキャッシュを作成します。 **CredentialCache** クラスがキャッシュを検索して要求で表示する適切な資格情報を検索するため、ユーザーは URL に基づいて資格情報を作成したり、表示したりする必要がなくなります。  
  
## <a name="see-also"></a>参照

- [.NET Framework のネットワーク プログラミング](index.md)
