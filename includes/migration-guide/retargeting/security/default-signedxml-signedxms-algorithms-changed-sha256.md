---
ms.openlocfilehash: db076d6799e4de5b8610cf9c1aac79b5386a7229
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67859009"
---
### <a name="default-signedxml-and-signedxms-algorithms-changed-to-sha256"></a>既定の SignedXML アルゴリズムと SignedXMS アルゴリズムが SHA256 に変更

|   |   |
|---|---|
|説明|.NET Framework 4.7 以前では、一部の操作において SignedXML と SignedCMS の初期設定が SHA1 でした。 .NET Framework 4.7.1 より、同じ操作で SHA256 が既定で有効になります。 この変更が必要になったのは、SHA1 は安全であると見なされなくなったためです。|
|提案される解決策|SHA1 (安全でない) または SHA256 を既定で使用するかどうかを制御する新しいコンテキスト スイッチ値が 2 つあります。<ul><li>Switch.System.Security.Cryptography.Xml.UseInsecureHashAlgorithms</li><li>Switch.System.Security.Cryptography.Pkcs.UseInsecureHashAlgorithms</li></ul>.NET Framework 4.7.1 以降のバージョンを対象とするアプリケーションで SHA256 の仕様が望ましくない場合、アプリの構成ファイルの [runtime](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) セクションに次の構成スイッチを追加することで既定を SHA1 に戻すことができます。<pre><code class="lang-xml">&lt;AppContextSwitchOverrides value=&quot;Switch.System.Security.Cryptography.Xml.UseInsecureHashAlgorithms=true;Switch.System.Security.Cryptography.Pkcs.UseInsecureHashAlgorithms=true&quot; /&gt;&#13;&#10;</code></pre>.NET Framework 4.7 以前のバージョンを対象とするアプリケーションの場合、アプリの構成ファイルの [runtime](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) セクションに次の構成スイッチを追加することでこの変更を選択できます。<pre><code class="lang-xml">&lt;AppContextSwitchOverrides value=&quot;Switch.System.Security.Cryptography.Xml.UseInsecureHashAlgorithms=false;Switch.System.Security.Cryptography.Pkcs.UseInsecureHashAlgorithms=false&quot; /&gt;&#13;&#10;</code></pre>|
|スコープ|マイナー|
|バージョン|4.7.1|
|種類|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.Security.Cryptography.Pkcs.CmsSigner?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.Xml.SignedXml?displayProperty=nameWithType></li><li><xref:System.Security.Cryptography.Xml.Reference?displayProperty=nameWithType></li></ul>|
