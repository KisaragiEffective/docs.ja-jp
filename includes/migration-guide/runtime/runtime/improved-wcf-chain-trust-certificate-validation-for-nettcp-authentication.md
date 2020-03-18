---
ms.openlocfilehash: e69ed64390504af872fa6785aa0b7d6c4db84ef0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "69671035"
---
### <a name="improved-wcf-chain-trust-certificate-validation-for-nettcp-certificate-authentication"></a>Net.Tcp 証明書認証の WCF チェーン信頼証明書の検証が向上した

|   |   |
|---|---|
|説明|.NET Framework 4.7.2 では、WCF とのトランスポート セキュリティで証明書認証を使用するときのチェーン信頼証明書の検証が向上しています。 この改善により、サーバーに対する認証に使用されるクライアント証明書を、クライアント認証用に構成する必要があります。  同様に、サーバーを認証するためのサーバー証明書は、サーバー認証用に構成する必要があります。 この変更では、ルート証明書が無効になっている場合、証明書チェーンの検証が失敗します。 同じ変更が、Windows セキュリティ ロールアップによって .NET Framework 3.5 以降のバージョンにも行われました。 詳細については、[こちら](https://support.microsoft.com/help/4055269/security-only-update-for-net-framework-3-5-1-4-5-2-4-6-4-6-1-4-6-2-4-7)をご覧ください。この変更は既定で有効になり、構成設定によって無効にできます。|
|提案される解決策|<ul><li>サーバー証明書とクライアント証明書に必要な EKU OID があるかどうかを確認します。 ない場合は、証明書を更新します。</li><li>ルート証明書が無効かどうかを確認します。 無効である場合は、ルート証明書を更新します。</li><li>変更をオプトアウトする方法: 証明書を更新できない場合は、次の構成設定により互換性に影響する変更を一時的に回避できます。ただし、変更をオプトアウトすると、システムはセキュリティの問題に対して脆弱なままになります。</li></ul><pre><code class="lang-xml">&lt;appSettings&gt;&#13;&#10;&lt;add key=&quot;wcf:useLegacyCertificateUsagePolicy&quot; value=&quot;true&quot; /&gt;&#13;&#10;&lt;/appSettings&gt;&#13;&#10;</code></pre>|
|スコープ|Minor|
|バージョン|4.7.2|
|[種類]|ランタイム|
