---
ms.openlocfilehash: 1148d040aa3b292d5c37eb50224413b6ddd202e2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67859029"
---
### <a name="servicebase-doesnt-propagate-onstart-exceptions"></a>ServiceBase で OnStart 例外を伝達させない

|   |   |
|---|---|
|説明|.NET Framework 4.7 以前のバージョンでは、サービス起動時にスローされた例外は <xref:System.ServiceProcess.ServiceBase.Run%2A?displayProperty=nameWithType> の呼び出し元に伝達されません。<br/>.NET Framework 4.7.1 を対象とするアプリケーション以降では、起動できなかったサービスに関して、ランタイムによって例外が <xref:System.ServiceProcess.ServiceBase.Run%2A?displayProperty=nameWithType> に伝達されます。|
|提案される解決策|サービスの起動時、例外が存在する場合、その例外は伝達されます。 サービスを起動できなかったとき、診断に役立ちます。 <br/>この動作が望ましくない場合、アプリケーション構成ファイルの &lt;runtime&gt; セクションに次の &lt;AppContextSwitchOverrides&gt; 要素を追加することで無効化できます。<pre><code class="lang-xml">&lt;AppContextSwitchOverrides value=&quot;Switch.System.ServiceProcess.DontThrowExceptionsOnStart=true&quot; /&gt;&#13;&#10;</code></pre>4\.7.1 より前のバージョンを対象とするアプリケーションでこの動作を望む場合、アプリケーション構成ファイルの &lt;runtime&gt; セクションに次の &lt;AppContextSwitchOverrides&gt; 要素を追加してください。<pre><code class="lang-xml">&lt;AppContextSwitchOverrides value=&quot;Switch.System.ServiceProcess.DontThrowExceptionsOnStart=false&quot; /&gt;&#13;&#10;</code></pre>|
|スコープ|Minor|
|バージョン|4.7.1|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.ServiceProcess.ServiceBase.Run(System.ServiceProcess.ServiceBase)?displayProperty=nameWithType></li><li><xref:System.ServiceProcess.ServiceBase.Run(System.ServiceProcess.ServiceBase[])?displayProperty=nameWithType></li></ul>|
