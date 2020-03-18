---
ms.openlocfilehash: ee9bba91a2c4e11bd005fedb8adf0c2e7c7b0d3e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67804443"
---
### <a name="certificate-eku-oid-validation"></a>証明書 EKU OID の検証

|   |   |
|---|---|
|説明|.NET Framework 4.6 以降、<xref:System.Net.Security.SslStream> または <xref:System.Net.ServicePointManager> クラスで EKU (拡張キー使用法) の OID (オブジェクト識別子) が検証されます。 EKU (拡張キー使用法) 拡張は、キーを使用するアプリケーションを示すオブジェクト識別子 (OID) の集まりです。 EKU OID 検証では、リモート証明書に意図している目的にかなった OID が与えられるようにリモート証明書コールバックが使用されます。|
|提案される解決策|この変更が望ましくない場合、アプリの構成ファイルの [\<](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) の [`AppContextSwitchOverrides>](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) に次のスイッチを追加することで証明書 EKU OID 検証を無効にできます。<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides&#13;&#10;value=&quot;Switch.System.Net.DontCheckCertificateEKUs=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre> <blockquote> [!IMPORTANT] この設定は下位互換性のためにのみ提供されます。 それ以外の目的での使用はお勧めしません。</blockquote> |
|スコープ|Minor|
|バージョン|4.6|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.Net.Security.SslStream?displayProperty=nameWithType></li><li><xref:System.Net.ServicePointManager?displayProperty=nameWithType></li><li><xref:System.Net.Http.HttpClient?displayProperty=nameWithType></li><li><xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType></li><li><xref:System.Net.HttpWebRequest?displayProperty=nameWithType></li><li><xref:System.Net.FtpWebRequest?displayProperty=nameWithType></li></ul>|
