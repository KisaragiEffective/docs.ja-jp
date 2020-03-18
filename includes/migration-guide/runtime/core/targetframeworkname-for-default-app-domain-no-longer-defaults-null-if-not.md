---
ms.openlocfilehash: cbe1b32fa40e509f620845866c7a584e37f49a80
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67858615"
---
### <a name="targetframeworkname-for-default-app-domain-no-longer-defaults-to-null-if-not-set"></a>既定のアプリケーション ドメインの TargetFrameworkName は、設定されなかった場合、既定で null に設定されなくなった

|   |   |
|---|---|
|説明|<xref:System.AppDomainSetup.TargetFrameworkName?displayProperty=name> は、以前は、明示的に設定されない限り、既定のアプリケーション ドメインでは null でした。 4\.6 以降では、既定のアプリケーション ドメインの <xref:System.AppDomainSetup.TargetFrameworkName?displayProperty=name> プロパティは、TargetFrameworkAttribute (ある場合) から派生された既定値を持ちます。 既定以外のアプリケーション ドメインは、明示的にオーバーライドされない限り、既定のアプリケーション ドメイン (4.6 では既定で null に設定されない) から <xref:System.AppDomainSetup.TargetFrameworkName?displayProperty=name> を継承し続けます。|
|提案される解決策|<xref:System.AppDomainSetup.TargetFrameworkName> が既定で null に設定されることに依然しないように、コードを更新する必要があります。 このプロパティが引き続き null として評価される必要がある場合、明示的にその値に設定できます。|
|スコープ|エッジ|
|バージョン|4.6|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.AppDomainSetup.TargetFrameworkName?displayProperty=nameWithType></li></ul>|
