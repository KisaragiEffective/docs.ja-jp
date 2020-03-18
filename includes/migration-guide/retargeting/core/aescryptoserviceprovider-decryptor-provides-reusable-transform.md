---
ms.openlocfilehash: c008809606372c84b05a2facd1cac1293382aed4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67859324"
---
### <a name="aescryptoserviceprovider-decryptor-provides-a-reusable-transform"></a>AesCryptoServiceProvider の復号で変換が再利用可能に

|   |   |
|---|---|
|説明|.NET Framework 4.6.2 以降を対象とするアプリでは、<xref:System.Security.Cryptography.AesCryptoServiceProvider> の復号で変換を再利用できます。 <xref:System.Security.Cryptography.CryptoAPITransform.TransformFinalBlock(System.Byte[],System.Int32,System.Int32)?displayProperty=name> の呼び出しの後、変換は初期化し直され、再利用することができます。 以前のバージョンの .NET Framework を対象とするアプリでは、<xref:System.Security.Cryptography.CryptoAPITransform.TransformBlock(System.Byte[],System.Int32,System.Int32,System.Byte[],System.Int32)?displayProperty=name> の呼び出しの後に、<xref:System.Security.Cryptography.CryptoAPITransform.TransformFinalBlock(System.Byte[],System.Int32,System.Int32)?displayProperty=name> を呼び出して、復号を再利用しようとすると、<xref:System.Security.Cryptography.CryptographicException> をスローするか、破損しているデータが生成されます。|
|提案される解決策|この動作は想定済みであり、この変更の影響は最小限に抑えられているはずです。この変更の影響は前の動作に依存するアプリケーションは、そのアプリケーションの構成ファイルの <code>&lt;runtime&gt;</code> セクションに次の構成設定を追加して、この動作の使用を無効にすることができます。<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Security.Cryptography.AesCryptoServiceProvider.DontCorrectlyResetDecryptor=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>また、以前のバージョンの .NET Framework を対象とするものの、.NET Framework 4.6.2 以降のバージョンの .NET Framework で実行されているアプリでは、そのアプリケーションの構成ファイルの <code>&lt;runtime&gt;</code> セクションに次の構成設定を追加して、この動作を有効にできます。<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Security.Cryptography.AesCryptoServiceProvider.DontCorrectlyResetDecryptor=false&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>|
|スコープ|Minor|
|バージョン|4.6.2|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.Security.Cryptography.AesCryptoServiceProvider.CreateDecryptor?displayProperty=nameWithType></li></ul>|
