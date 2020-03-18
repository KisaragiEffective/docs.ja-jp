---
title: ネットワーク アプリケーションのキャッシュ管理
ms.date: 03/30/2017
helpviewer_keywords:
- cache [.NET Framework], network applications
- network resources, caching
- Internet, caching
ms.assetid: fc258a40-f370-434f-ae09-4a8cb11ddaeb
ms.openlocfilehash: 7e131963999db3e3d5e0e6f3fa110da36e6452a1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71048884"
---
# <a name="cache-management-for-network-applications"></a>ネットワーク アプリケーションのキャッシュ管理
このトピックと関連するサブトピックでは、<xref:System.Net.WebClient>、<xref:System.Net.WebRequest>、<xref:System.Net.HttpWebRequest>、および <xref:System.Net.FtpWebRequest> クラスを使用して取得したリソースのキャッシュ処理について説明します。  
  
 キャッシュは、アプリケーションから要求されたリソースの一時的な記憶域として機能します。 アプリケーションから同じリソースを複数回要求される場合、キャッシュからリソースを返すことで、サーバーから再要求するオーバーヘッドを回避できます。 キャッシュによって、要求されたリソースの取得にかかる時間が短縮されるので、アプリケーションのパフォーマンスを改善できます。 また、サーバーへのトリップ回数が減るので、ネットワーク トラフィックを減らすこともできます。 キャッシュによってパフォーマンスは改善されますが、アプリケーションに古いリソースが返される危険性もあります。つまり、キャッシュを使用していなかった場合にサーバーから送信されたリソースと、キャッシュのリソースは同じではないということです。  
  
 また、キャッシュによって、権限のないユーザーまたはプロセスが機密データを閲覧できる可能性があります。 キャッシュされている認証済みの応答が、追加の承認なしでキャッシュから取得される可能性があるからです。 キャッシュを有効にする場合は、<xref:System.Net.WebRequest.CachePolicy%2A> を <xref:System.Net.Cache.RequestCacheLevel.BypassCache> または <xref:System.Net.Cache.RequestCacheLevel.NoCacheNoStore> に変更して、この要求のキャッシュを無効にしてください。  
  
 セキュリティの懸念事項があるため、中間層のシナリオにキャッシュを使用することは**推奨されません**。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [キャッシュ ポリシー](cache-policy.md)  
 キャッシュ ポリシーの概要と定義方法について説明します。  
  
 [場所ベースのキャッシュ ポリシー](location-based-cache-policies.md)  
 ハイパーテキスト転送プロトコル (http と https) リソースに使用できる各種の場所ベースのキャッシュ ポリシーを定義します。  
  
 [時間ベースのキャッシュ ポリシー](time-based-cache-policies.md)  
 時間ベースのキャッシュ ポリシーのカスタマイズに使用できる条件について説明します。  
  
 [ネットワーク アプリケーションでのキャッシュの構成](configuring-caching-in-network-applications.md)  
 キャッシュ ポリシーとキャッシュを使用する要求をプログラムで作成する方法について説明します。  
  
## <a name="reference"></a>リファレンス  
 <xref:System.Net.Cache>  
 取得したリソースのキャッシュ ポリシーを <xref:System.Net.WebRequest>、<xref:System.Net.HttpWebRequest>、および <xref:System.Net.FtpWebRequest> クラスを使用して定義するために使用する型および列挙体を定義します。
