---
title: Cache Policy
ms.date: 03/30/2017
helpviewer_keywords:
- time-based cache policies
- location-based cache policies
- cache [.NET Framework], policies
- request cache policies
- freshness of cached resources
- content cache policies
- expired content
ms.assetid: 1a7e04ec-7872-41c2-96c6-52566dcb412b
ms.openlocfilehash: 2d3d85ebd80f417ebd0fa0e619097e15f2a6a39b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71048769"
---
# <a name="cache-policy"></a>Cache Policy
キャッシュ ポリシーには、要求されたリソースのキャッシュ コピーを使用して要求を満たすことができるかどうかを決定するルールを定義します。 アプリケーションは、更新のクライアント キャッシュ要件を指定しますが、実質的なキャッシュ ポリシーは、クライアントのキャッシュ要件、サーバーのコンテンツ有効期限要件、およびサーバーの再検証要件によって決まります。 最新のコンテンツをクライアント アプリケーションに確実に返すために、クライアントのキャッシュ ポリシーとサーバーの要件の相互作用によって、常に最も保守的なキャッシュ ポリシーが適用されます。  
  
 キャッシュ ポリシーは、場所ベースまたは時間ベースです。 場所ベースのキャッシュ ポリシーでは、要求されたリソースを取得できる場所に基づいて、キャッシュされたエントリの鮮度を定義します。 時間ベースのキャッシュ ポリシーは、リソースの取得時間、リソースと共に返されたヘッダー、現在時刻を利用し、キャッシュされているエントリの更新の確認間隔を定義します。 ほとんどのアプリケーションでは、既定の時間ベースのキャッシュ ポリシーを使用できます。時間ベースのキャッシュ ポリシーは、RFC 2616 ([インターネット技術標準化委員会 (IETF)](https://www.ietf.org/) Web サイトで入手可能) に規定されているキャッシュ ポリシーを実装しています。  
  
 次の表で説明されているクラスを使用して、キャッシュ ポリシーを指定します。  
  
|クラス名|[説明]|  
|----------------|-----------------|  
|<xref:System.Net.Cache.HttpRequestCachePolicy>|<xref:System.Net.HttpWebRequest> オブジェクトを使用して要求されたリソースの場所ベースと時間ベースのキャッシュ ポリシーを表します。|  
|<xref:System.Net.Cache.RequestCachePolicy>|<xref:System.Net.Cache.RequestCacheLevel.Default> オブジェクトを使用して要求されたリソースの場所ベースのキャッシュ ポリシーまたは <xref:System.Net.WebRequest> 時間ベースのキャッシュ ポリシーを表します。|  
|<xref:System.Net.Cache.HttpCacheAgeControl>|時間ベースの <xref:System.Net.Cache.HttpRequestCachePolicy> オブジェクトの作成に使用される値を指定します。|  
|<xref:System.Net.Cache.HttpRequestCacheLevel>|場所ベースと時間ベースの <xref:System.Net.Cache.HttpRequestCachePolicy> オブジェクトの作成に使用される値を指定します。|  
|<xref:System.Net.Cache.RequestCacheLevel>|場所ベースまたは <xref:System.Net.Cache.RequestCacheLevel.Default> 時間ベースの <xref:System.Net.Cache.RequestCachePolicy> オブジェクトの作成に使用される値を指定します。|  
  
 アプリケーションから発行されるすべての要求、または個々の要求に対してキャッシュ ポリシーを定義できます。 アプリケーションレベルのキャッシュ ポリシーと要求レベルのキャッシュ ポリシーの両方を指定すると、要求レベルのポリシーが使用されます。 アプリケーションレベルのキャッシュ ポリシーを指定するには、プログラム、またはアプリケーションやコンピューターの構成ファイルを使用します。 詳細については、「[\<requestCaching> 要素 (ネットワーク設定)](../configure-apps/file-schema/network/requestcaching-element-network-settings.md)」を参照してください。  
  
 キャッシュ ポリシーを作成するには、<xref:System.Net.Cache.RequestCachePolicy> クラスまたは <xref:System.Net.Cache.HttpRequestCachePolicy> クラスのインスタンスを作成して、ポリシー オブジェクトを作成する必要があります。 要求でポリシーを指定するには、要求の <xref:System.Net.WebRequest.CachePolicy%2A> プロパティをポリシー オブジェクトに設定します。 アプリケーションレベルのポリシーをプログラムで設定する場合は、<xref:System.Net.HttpWebRequest.DefaultCachePolicy%2A> プロパティをポリシー オブジェクトに設定します。  
  
 キャッシュ ポリシーを作成し、利用する方法を示すコード例については、「[Configuring Caching in Network Applications](configuring-caching-in-network-applications.md)」(ネットワーク アプリケーションでのキャッシュの構成) を参照してください。  
  
## <a name="see-also"></a>参照

- [ネットワーク アプリケーションのキャッシュ管理](cache-management-for-network-applications.md)
- [場所ベースのキャッシュ ポリシー](location-based-cache-policies.md)
- [時間ベースのキャッシュ ポリシー](time-based-cache-policies.md)
- [ネットワーク アプリケーションでのキャッシュの構成](configuring-caching-in-network-applications.md)
