---
ms.openlocfilehash: efe8a2dd98865f6a24b65ce0f08eb0c574b708f7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67804572"
---
### <a name="currentculture-and-currentuiculture-flow-across-tasks"></a>タスク全体の CurrentCulture と CurrentUICulture のフロー

|   |   |
|---|---|
|説明|.NET Framework 4.6 より、非同期操作全体をフローする、スレッドの <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name> に <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name> と <xref:System.Threading.ExecutionContext?displayProperty=name> が格納されます。その結果、<xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name> または <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name> に対する変更は、後で非同期実行されるタスクで反映されます。 これは、すべての非同期タスクで <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name> と <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name> がリセットされていた、以前のバージョンの .NET Framework の動作とは異なります。|
|提案される解決策|この変更の影響を受けるアプリでは、非同期タスクの最初の操作として任意の <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name> または <xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name> を明示的に設定することで問題を回避できます。 あるいは、次の互換性スイッチを設定することで、<xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name>/<xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name> をフローしない以前の動作を選択できます。<pre><code class="lang-csharp">AppContext.SetSwitch(&quot;Switch.System.Globalization.NoAsyncCurrentCulture&quot;, true);&#13;&#10;</code></pre>この問題は、.NET Framework 4.6.2 の WPF で修正されました。 また、.NET Frameworks 4.6、4.6.1 では [KB 3139549](https://support.microsoft.com/kb/3139549) を通じて修正されました。 .NET Framework 4.6 以降を対象とするアプリケーションでは WPF アプリケーション (<xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=name>/<xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=name>) の正しい動作が自動的に取得されます。これは、ディスパッチャー操作にわたって維持されます。|
|スコープ|Minor|
|バージョン|4.6|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=nameWithType></li><li><xref:System.Threading.Thread.CurrentCulture?displayProperty=nameWithType></li><li><xref:System.Globalization.CultureInfo.CurrentUICulture?displayProperty=nameWithType></li><li><xref:System.Threading.Thread.CurrentUICulture?displayProperty=nameWithType></li></ul>|
