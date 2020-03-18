---
ms.openlocfilehash: 136ffc9305de79679ac9d254c026ecb0debc7c52
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "68235549"
---
### <a name="sslstream-supports-tls-alerts"></a>SslStream で TLS アラートに対応

|   |   |
|---|---|
|説明|TLS ハンドシェイクに失敗すると、最初の I/O 読み取り/書き込み操作によって <xref:System.IO.IOException?displayProperty=name> と内部例外 <xref:System.ComponentModel.Win32Exception?displayProperty=name> がスローされます。 [TLS および SSL アラートの Schannel エラー コード](https://docs.microsoft.com/windows/desktop/SecAuthN/schannel-error-codes-for-tls-and-ssl-alerts)を使用して、<xref:System.ComponentModel.Win32Exception?displayProperty=name> の <xref:System.ComponentModel.Win32Exception.NativeErrorCode?displayProperty=name> コードをリモート パーティからの TLS アラートにマップすることができます。詳しくは、[RFC 2246: セクション 7.2.2「Error alerts (エラー アラート)」](https://tools.ietf.org/html/rfc2246#section-7.2.2)を参照してください。 <br/>.NET Framework 4.6.2 およびそれより前のバージョンでの動作では、他のパーティがハンドシェイクに失敗してそのすぐ後に接続を拒否した場合、トランスポート チャネル (通常は TCP 接続) は書き込みまたは読み取り中にタイムアウトします。|
|提案される解決策|<xref:System.IO.Stream.Read(System.Byte[],System.Int32,System.Int32)>/<xref:System.IO.Stream.Write(System.Byte[],System.Int32,System.Int32)> などのネットワーク I/O API を呼び出すアプリケーションは、<xref:System.IO.IOException> または <xref:System.TimeoutException?displayProperty=name> を処理する必要があります。<br/>TLS アラート機能は、.NET Framework 4.7 以降では既定で有効になります。 .NET Framework 4.7 またはそれより後のシステムで動作する 4.0 から 4.6.2 のバージョンの .NET Framework を対象とするアプリケーションは、互換性を維持するためにこの機能を無効にします。 <br/>.NET Framework 4.7 以降で動作する .NET Framework 4.6 以降のアプリケーションでこの機能を有効または無効にするには、次の構成 API を使います。<ul><li>プログラムによる:</li></ul>ServicePointManager は 1 回のみ初期化するため、アプリケーションで行われる一番最初の動作にする必要があります。<pre><code class="lang-csharp">AppContext.SetSwitch(&quot;TestSwitch.LocalAppContext.DisableCaching&quot;, true);&#13;&#10;AppContext.SetSwitch(&quot;Switch.System.Net.DontEnableTlsAlerts&quot;, true); // Set to &#39;false&#39; to enable the feature in .NET Framework 4.6 - 4.6.2.&#13;&#10;</code></pre><ul><li>AppConfig:</li></ul><pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Net.DontEnableTlsAlerts=true&quot;/&gt;&#13;&#10;&lt;!-- Set to &#39;false&#39; to enable the feature in .NET Framework 4.6 - 4.6.2. --&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre><ul><li>レジストリ キー (マシン グローバル):</li></ul>.NET Framework 4.6 - 4.6.2 でこの機能を有効にするには、Value を <code>false</code> に設定します。<ul><li>重要:HKLM\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\AppContext\Switch.System.Net.DontEnableTlsAlerts</li><li>型:String</li><li>値: &quot;true&quot;</li></ul>|
|スコープ|エッジ|
|バージョン|4.7|
|種類|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.Net.Security.SslStream?displayProperty=nameWithType></li><li><xref:System.Net.WebRequest?displayProperty=nameWithType></li><li><xref:System.Net.HttpWebRequest?displayProperty=nameWithType></li><li><xref:System.Net.FtpWebRequest?displayProperty=nameWithType></li><li><xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType></li><li><xref:System.Net.Http?displayProperty=nameWithType></li></ul>|
