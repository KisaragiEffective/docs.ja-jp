---
ms.openlocfilehash: 2f960942bda54505690cbac3151ef74ec0ab5ebb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "68235488"
---
### <a name="deadlock-may-result-when-using-reentrant-services"></a>再入可能なサービスを使用していると、デッドロックが発生する可能性がある

|   |   |
|---|---|
|説明|サービスのインスタンスの実行が一度に 1 つのスレッドに制限される再入可能なサービスでは、デッドロックが発生します。 この問題が発生しやすいサービスのコードには、次の <xref:System.ServiceModel.ServiceBehaviorAttribute> が含まれています。<pre><code class="lang-csharp">[ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Reentrant)]&#13;&#10;</code></pre>|
|提案される解決策|この問題に対処するために、次の操作を行うことができます。<ul><li>サービスのコンカレンシー モードを <xref:System.ServiceModel.ConcurrencyMode.Single?displayProperty=nameWithType> または &lt;System.ServiceModel.ConcurrencyMode.Multiple?displayProperty=nameWithType&gt; に設定します。 次に例を示します。</li></ul><pre><code class="lang-csharp">[ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Single)]&#13;&#10;</code></pre><ul><li>最新の更新プログラムを .NET framework 4.6.2 にインストールするか、最新バージョンの .NET Framework にアップグレードします。 これにより、<xref:System.Threading.ExecutionContext> の <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> のフローが無効になります。 この動作は構成可能です。構成ファイルに次のアプリ設定を追加することと同じです。</li></ul><pre><code class="lang-xml">&lt;appSettings&gt;&#13;&#10;&lt;add key=&quot;Switch.System.ServiceModel.DisableOperationContextAsyncFlow&quot; value=&quot;true&quot; /&gt;&#13;&#10;&lt;/appSettings&gt;&#13;&#10;</code></pre>Rentran サービスの場合、<code>Switch.System.ServiceModel.DisableOperationContextAsyncFlow</code> の値は <code>false</code> に設定しないでください。|
|スコープ|Minor|
|バージョン|4.6.2|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.ServiceModel.ServiceBehaviorAttribute?displayProperty=nameWithType></li><li><xref:System.ServiceModel.ConcurrencyMode.Reentrant?displayProperty=nameWithType></li></ul>|
