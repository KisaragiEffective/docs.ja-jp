---
ms.openlocfilehash: 506218195417548880a9d8d10508a570a7769682
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67859366"
---
### <a name="long-path-support"></a>長いパスのサポート

|   |   |
|---|---|
|説明|以降とするアプリをターゲットとする .NET Framework 4.6.2、長いパス (最大 32 K の文字) がサポートされている、および 260 文字 (または<code>MAX_PATH</code>) パスの長さの制限がなくなりました。 .NET Framework 4.6.2 を対象として再コンパイルされたアプリの場合は、コードをスローした以前のパス、 <xref:System.IO.PathTooLongException?displayProperty=name> 260 文字をスローがパスを超えたため、<xref:System.IO.PathTooLongException?displayProperty=name>次の条件下でのみ。<ul><li>パスの長さが <xref:System.Int16.MaxValue> (32,767) 文字を超えている。</li><li>オペレーティング システムが <code>COR_E_PATHTOOLONG</code> またはそれと同等のものを返す。</li></ul>.NET Framework 4.6.1 以前を対象とするアプリの場合、パスが 260 文字を超えるたびにランタイムで自動的に <xref:System.IO.PathTooLongException?displayProperty=name> がスローされます。|
|提案される解決策|.NET Framework 4.6.2 を対象とするアプリケーションの場合、長いパスが望ましくないときは、<code>&lt;runtime&gt;</code> ファイルの <code>app.config</code> セクションに次を追加することで長いパスのサポートを無効にできます。<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.IO.BlockLongPaths=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>以前のバージョンの .NET Framework を対象とするが、.NET Framework 4.6.2 以降で実行するアプリの場合、<code>&lt;runtime&gt;</code> ファイルの <code>app.config</code> セクションに次を追加することで長いパスのサポートを選択できます。<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.IO.BlockLongPaths=false&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>|
|スコープ|Minor|
|バージョン|4.6.2|
|[種類]|再ターゲット中|
