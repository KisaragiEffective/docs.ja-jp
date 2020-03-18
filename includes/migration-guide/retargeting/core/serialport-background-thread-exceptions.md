---
ms.openlocfilehash: 81b104d8e5a9ccc8e790c3b16e4837cfa0c0def5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67859058"
---
### <a name="serialport-background-thread-exceptions"></a>SerialPort バックグラウンド スレッドの例外

|   |   |
|---|---|
|説明|OS 例外がスローされたとき、<xref:System.IO.Ports.SerialPort> ストリームで作成されたバックグラウンド スレッドによってプロセスが終了することがなくなりました。 <br/>.NET Framework 4.7 以前のバージョンを対象とするアプリケーションでは、<xref:System.IO.Ports.SerialPort> ストリームで作成されたバックグラウンド スレッドでオペレーティング システム例外がスローされたとき、プロセスが終了します。 <br/>.NET Framework 4.7.1 以降のバージョンを対象とするアプリケーションでは、バックグラウンド スレッドはアクティブなシリアル ポートに関連付けられている OS イベントを待ち、シリアル ポートが急に削除されるなどした場合にクラッシュすることがあります。|
|提案される解決策|.NET Framework 4.7.1 を対象とするアプリケーションの場合、例外処理が望ましくないときは、<code>&lt;runtime&gt;</code> ファイルの <code>app.config</code> セクションに次を追加することで例外処理を無効にできます。<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.IO.Ports.DoNotCatchSerialStreamThreadExceptions=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>以前のバージョンの .NET Framework を対象とするが、.NET Framework 4.7.1 以降で実行するアプリの場合、<code>&lt;runtime&gt;</code> ファイルの <code>app.config</code> セクションに次を追加することで例外処理を選択できます。<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.IO.Ports.DoNotCatchSerialStreamThreadExceptions=false&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>|
|スコープ|Minor|
|バージョン|4.7.1|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.IO.Ports.SerialPort?displayProperty=nameWithType></li></ul>|
