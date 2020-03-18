---
ms.openlocfilehash: b57e0acb03a99f33460a7b6c880280b37e01a17b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67859259"
---
### <a name="wcf-transport-security-supports-certificates-stored-using-cng"></a>WCF トランスポート セキュリティで CNG を使用して格納される証明書をサポート

|   |   |
|---|---|
|説明|.NET Framework 4.6.2 を対象とするアプリ以降では、WCF トランスポート セキュリティでは、Windows 暗号化ライブラリ (CNG) を使用して格納される証明書がサポートされます。 このサポートは、指数の長さが 32 ビット以下の公開キーを持つ証明書に限定されます。 アプリケーションが対象 .NET Framework 4.6.2、ときにこの機能は既定でオンです。 .NET Framework の以前のバージョンで X509 を使用しようとすると、証明書を CSG のキー記憶域プロバイダーが例外をスローします。|
|提案される解決策|.NET Framework 4.6.1 以前を対象とするものの、.NET Framework 4.6.2 で実行されているアプリの場合、app.config または web.config ファイルの <code>&lt;runtime&gt;</code> セクションに次の行を追加することで、CNG 証明書のサポートを有効にできます。<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.ServiceModel.DisableCngCertificates=false&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>次のコードを使用してプログラムで行うこともできます。<pre><code class="lang-cs">private const string DisableCngCertificates = @&quot;Switch.System.ServiceModel.DisableCngCertificate&quot;;&#13;&#10;AppContext.SetSwitch(disableCngCertificates, false);&#13;&#10;</code></pre><pre><code class="lang-vb">Const DisableCngCertificates As String = &quot;Switch.System.ServiceModel.DisableCngCertificates&quot;&#13;&#10;AppContext.SetSwitch(disableCngCertificates, False)&#13;&#10;</code></pre>この変更のため、CNG 証明書の失敗で、セキュリティで保護された通信を開始する試行に依存する例外処理コードは、実行されなくなることに注意してください。|
|スコープ|Minor|
|バージョン|4.6.2|
|[種類]|再ターゲット中|
