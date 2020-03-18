---
ms.openlocfilehash: 3f553f95941eaf36cf335e9765a670a05bd157f4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67858407"
---
### <a name="appdomainsetupdynamicbase-is-no-longer-randomized-by-userandomizedstringhashalgorithm"></a>AppDomainSetup.DynamicBase が UseRandomizedStringHashAlgorithm でランダム化されなくなった

|   |   |
|---|---|
|説明|.NET Framework 4.6 より前では、UseRandomizedStringHashAlgorithm がアプリの構成ファイルで有効になっている場合、<xref:System.AppDomainSetup.DynamicBase> の値がアプリケーション ドメイン間、またはプロセス間でランダム化されます。 .NET Framework 4.6 以降では、<xref:System.AppDomainSetup.DynamicBase> は実行されているアプリの異なるインスタンス間、および異なるアプリ ドメイン間で安定した結果を返します。 それでも動的ベースはアプリによって異なります。この変更では、同じアプリの異なるインスタンスのランダムな名前付け要素のみが削除されます。|
|提案される解決策|<code>UseRandomizedStringHashAlgorithm</code> を有効にすると、<xref:System.AppDomainSetup.DynamicBase> がランダム化されなくなることに注意してください。 ランダム ベースが必要な場合は、この API を使用するのではなく、アプリのコードで生成する必要があります。|
|スコープ|エッジ|
|バージョン|4.6|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.AppDomainSetup.DynamicBase?displayProperty=nameWithType></li></ul>|
