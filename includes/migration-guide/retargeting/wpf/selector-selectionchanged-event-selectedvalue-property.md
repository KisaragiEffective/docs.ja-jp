---
ms.openlocfilehash: e6c93a1bc31c041f36fca3704bca32012a2b42ac
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67859022"
---
### <a name="selector-selectionchanged-event-and-selectedvalue-property"></a>セレクター SelectionChanged イベントおよび SelectedValue プロパティ

|   |   |
|---|---|
|説明|NET Framework 4.7.1 以降、<xref:System.Windows.Controls.Primitives.Selector> は選択の変更が発生すると必ず、<xref:System.Windows.Controls.Primitives.Selector.SelectedValue%2A> イベントが発生する前に <xref:System.Windows.Controls.Primitives.Selector.SelectionChanged> プロパティの値を更新します。 これにより、SelectedValue プロパティと、イベント発生前に更新されるその他の選択プロパティ (<xref:System.Windows.Controls.Primitives.Selector.SelectedItem%2A> および <xref:System.Windows.Controls.Primitives.Selector.SelectedIndex%2A>) との整合性が確保されます。<p/>.NET Framework 4.7 以前のバージョンでは、ほとんどの場合、イベントの前に SelectedValue に対する更新が行われました。しかし、選択の変更が <xref:System.Windows.Controls.Primitives.Selector.SelectedValue%2A> プロパティの変更に起因している場合、SelectedValue に対する更新はイベントの後に行われていました。|
|提案される解決策|.NET Framework 4.7.1 以降を対象とするアプリでこの変更を無効にし、従来の動作を使用することができます。その場合、アプリケーション構成ファイルの <code>&lt;runtime&gt;</code> セクションに次の行を追加します。<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides&#13;&#10;value=&quot;Switch.System.Windows.Controls.TabControl.SelectionPropertiesCanLagBehindSelectionChangedEvent=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>.NET Framework 4.7 以前を対象とするものの、.NET Framework 4.7.1 以降で実行されているアプリでは、新しい動作を有効にすることができます。その場合、アプリケーション構成ファイルの <code>&lt;runtime&gt;</code> セクションに次の行を追加します。<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Controls.TabControl.SelectionPropertiesCanLagBehindSelectionChangedEvent=false&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>|
|スコープ|Minor|
|バージョン|4.7.1|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.Windows.Controls.TabControl.SelectedContent?displayProperty=nameWithType></li><li><xref:System.Windows.Controls.Primitives.Selector.SelectionChanged?displayProperty=nameWithType></li></ul>|
