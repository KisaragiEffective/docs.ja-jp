---
title: キャッシュ ポリシーの相互作用 — 最大有効期間と最小鮮度
ms.date: 03/30/2017
helpviewer_keywords:
- time-based cache policies
- Revalidate policy
- cache [.NET Framework], time-based policies
- freshness of cached resources
- maximum age policy
- minimum freshness policy
- age of cached resources
ms.assetid: 6567d451-ecec-496c-95a3-a415b99ba52a
ms.openlocfilehash: 2ec958cc035ac62086cdd3e2844811accc181d47
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71048811"
---
# <a name="cache-policy-interactionmaximum-age-and-minimum-freshness"></a>キャッシュ ポリシーの相互作用 — 最大有効期間と最小鮮度
最新のコンテンツをクライアント アプリケーションに確実に返すために、クライアントのキャッシュ ポリシーとサーバーの再検証要件の相互作用によって、常に最も保守的なキャッシュ ポリシーが適用されます。 このトピックの例はいずれも、1 月 1 日にキャッシュされ、1 月 4 日に期限切れになるリソースのキャッシュ ポリシーを示しています。  
  
 以下の例は、最大有効期間 (`maxAge`) と最小鮮度 (`minFresh`) 値の相互作用の結果となるキャッシュ ポリシーを示しています。  
  
- キャッシュ ポリシーで `maxAge` = 2 日間と設定され、`minFresh` が指定されていない場合、コンテンツは 1 月 3 日に再検証されます。  
  
- キャッシュ ポリシーで `maxAge` = 2 日間と `minFresh` = 1 日間が設定されている場合、`maxAge` 値に従い、コンテンツは 1 月 3 日まで最新です。 `minFresh` 値に従い、コンテンツは 1 月 3 日まで最新です。 そのため、コンテンツは 1 月 3 日に再検証する必要があります。  
  
- キャッシュ ポリシーで `maxAge` = 2 日間と `minFresh` = 2 日間が設定されている場合、`maxAge` 値に従い、コンテンツは 1 月 3 日まで最新です。 `minFresh` 値に従い、コンテンツは 1 月 2 日まで最新です。 そのため、コンテンツは 1 月 2 日に再検証する必要があります。  
  
## <a name="see-also"></a>参照

- [ネットワーク アプリケーションのキャッシュ管理](cache-management-for-network-applications.md)
- [キャッシュ ポリシー](cache-policy.md)
- [場所ベースのキャッシュ ポリシー](location-based-cache-policies.md)
- [時間ベースのキャッシュ ポリシー](time-based-cache-policies.md)
- [ネットワーク アプリケーションでのキャッシュの構成](configuring-caching-in-network-applications.md)
- [キャッシュ ポリシーの相互作用 — 最大有効期間と最大期限延長](cache-policy-interaction-maximum-age-and-maximum-staleness.md)
