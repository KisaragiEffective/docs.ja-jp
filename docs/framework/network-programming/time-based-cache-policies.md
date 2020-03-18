---
title: 時間ベースのキャッシュ ポリシー
ms.date: 03/30/2017
helpviewer_keywords:
- time-based cache policies
- cache synchronization date policy
- cache [.NET Framework], time-based policies
- freshness of cached resources
- time, cached resources
- maximum age policy
- synchronization, cache
- staleness of cached resources
- default time-based cache policy
- maximum staleness policy
- content cache policies
- expired content
- minimum freshness policy
- age of cached resources
ms.assetid: 74f0bcaf-5c95-40c1-9967-f3bbf1d2360a
ms.openlocfilehash: 0edde8e716d5ce3b1444e994234def5835341475
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71047124"
---
# <a name="time-based-cache-policies"></a>時間ベースのキャッシュ ポリシー
時間ベースのキャッシュ ポリシーは、リソースの取得時間、リソースと共に返されたヘッダー、現在時刻を利用し、キャッシュされているエントリの更新の確認間隔を定義します。 時間ベースのキャッシュ ポリシーを設定するとき、<xref:System.Net.Cache.HttpRequestCacheLevel.Default> 時間ベース キャッシュ ポリシーを利用するか、カスタマイズした時間ベース ポリシーを作成できます。 ハイパーテキスト転送プロトコル (HTTP) を利用して取得されるリソースに既定の時間ベース ポリシーを利用するとき、厳密なキャッシュ動作は、キャッシュされている応答に含まれているヘッダーと、RFC 2616 のセクション 13 とセクション 14 に指定されている動作で決定されます。RFC 2616 は [インターネット技術標準化委員会 (IETF)](https://www.ietf.org/) Web サイトで確認できます。 HTTP リソースの既定の時間ベース ポリシーを設定する方法を示すコード例については、「[How to: Set the Default Time-Based Cache Policy for an Application](how-to-set-the-default-time-based-cache-policy-for-an-application.md)」(方法: アプリケーションの既定の時間ベースのキャッシュ ポリシーを設定する) を参照してください。 キャッシュ ポリシーを作成し、利用する方法を示すコード例については、「[Configuring Caching in Network Applications](configuring-caching-in-network-applications.md)」(ネットワーク アプリケーションでのキャッシュの構成) を参照してください。  
  
## <a name="criteria-to-determine-freshness-of-cached-entries"></a>キャッシュされたエントリの更新の確認間隔を決定する基準  
 時間ベースのキャッシュ ポリシーをカスタマイズするとき、次の条件を 1 つまたは複数利用し、キャッシュされているエントリの更新の確認間隔を決定するように指定できます。  
  
- 最大有効期間  
  
- 最大期限延長  
  
- 最小鮮度  
  
- キャッシュ同期日付  
  
> [!NOTE]
> 既定の時間ベース キャッシュ ポリシーを使用することと、アプリケーションの既定のキャッシュ ポリシーを設定することを混同しないでください。 既定の時間ベース ポリシーは、要求またはアプリケーション レベルで利用できる特定のポリシーです。 アプリケーションの既定のキャッシュ ポリシーは、要求にポリシーが設定されていないときに有効なポリシーです (場所ベースまたは時間ベース)。 アプリケーションの既定のキャッシュ ポリシーを設定する方法については、「<xref:System.Net.WebRequest.DefaultCachePolicy%2A>」を参照してください。  
  
### <a name="maximum-age"></a>最大有効期間  
 最大有効期間というポリシー基準は、リソースのキャッシュ済みコピーを利用できる時間を指定します。 そのリソースのキャッシュ済みコピーが指定された時間より古い場合、サーバーのコンテンツに対して確認し、リソースを再検証する必要があります。 最大有効期間で、失効後にリソースの利用が許可される場合、最大期限延長も指定されない限り、この基準は守られません。  
  
### <a name="maximum-staleness"></a>最大期限延長  
 最大期限延長というポリシー基準は、リソースのキャッシュ済みコピーを利用できる時間をコンテンツの失効後に指定します。 これは、失効後、リソースの利用を許可する唯一のキャッシュ ポリシー基準です。  
  
### <a name="minimum-freshness"></a>最小鮮度  
 最小鮮度というポリシー基準は、リソースのキャッシュ済みコピーを利用できる時間をコンテンツの失効前に指定します。 このポリシーにより、有効期限前にキャッシュ エントリが失効します。最小鮮度と最大期限延長は互いに排他的になります。  
  
## <a name="cache-synchronization-date"></a>キャッシュ同期日付  
 キャッシュ同期日付というポリシー基準は、サーバーのコンテンツに対して確認することで、リソースのキャッシュ済みコピーを再検証するタイミングを決定します。 アイテムのキャッシュ後にコンテンツが変更された場合、そのコンテンツはサーバーから取得され、キャッシュに保存され、アプリケーションに返されます。 コンテンツが変わっていない場合、そのタイムスタンプが更新され、アプリケーションはキャッシュ済みコンテンツを取得します。  
  
 キャッシュ同期日付を利用し、キャッシュ済みコンテンツを再検証する絶対的日付を指定できます。 新しいキャッシュ エントリが最後に再検証されたのがキャッシュ同期日付より前であれば、サーバーにより再検証が発生します。 キャッシュ同期日付後にキャッシュ エントリが再検証されたとき、キャッシュ済みエントリを無効にする、付加的な更新確認要件またはサーバー再検証要件がなければ、キャッシュからのエントリが利用されます。 キャッシュ同期日付が未来の日付に設定されている場合、キャッシュ同期日付が過ぎるまで、要求のたびにエントリが再検証されます。  
  
 後続のトピックでは、時間ベース キャッシュ ポリシー基準の組み合わせについて説明します。  
  
- [キャッシュ ポリシーの相互作用 — 最大有効期間と最大期限延長](cache-policy-interaction-maximum-age-and-maximum-staleness.md)  
  
- [キャッシュ ポリシーの相互作用 - 最大有効期間と最小鮮度](cache-policy-interaction-maximum-age-and-minimum-freshness.md)  
  
## <a name="see-also"></a>参照

- [ネットワーク アプリケーションのキャッシュ管理](cache-management-for-network-applications.md)
- [キャッシュ ポリシー](cache-policy.md)
- [場所ベースのキャッシュ ポリシー](location-based-cache-policies.md)
- [ネットワーク アプリケーションでのキャッシュの構成](configuring-caching-in-network-applications.md)
- [\<requestCaching> 要素 (ネットワーク設定)](../configure-apps/file-schema/network/requestcaching-element-network-settings.md)
