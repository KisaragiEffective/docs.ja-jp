---
ms.openlocfilehash: 4eac93d5cfea19cb83c66cd3fe35c1b0703c0cc0
ms.sourcegitcommit: 5b6d778ebb269ee6684fb57ad69a8c28b06235b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59234037"
---
### <a name="wpf-focusvisual-for-radiobutton-and-checkbox-now-displays-correctly-when-the-controls-have-no-content"></a>WPF で、コントロールに内容がないとき、RadioButton と CheckBox のフォーカス ビジュアルが正しく表示されるようになった

|   |   |
|---|---|
|説明|.NET Framework 4.7.1 およびそれより前のバージョンでは、WPF の <xref:System.Windows.Controls.CheckBox?displayProperty=nameWIthType> と <xref:System.Windows.Controls.RadioButton?displayProperty=nameWIthType> のフォーカス ビジュアルに整合性がなく、クラシック テーマとハイ コントラスト テーマでは正しくありません。  これらの問題は、コントロールに内容が何も設定されていない場合に発生します。  これにより、テーマ間の遷移が混乱し、フォーカス ビジュアルが見にくくなることがあります。 .NET Framework 4.7.2 では、これらのビジュアルのテーマ間の整合性が向上し、クラシック モードとハイ コントラスト テーマでの視認性がよくなっています。|
|提案される解決策|.NET Framework 4.7.2 を対象にしている開発者が、.NET 4.7.1 での動作に戻したい場合は、以下の AppContext フラグを設定する必要があります。<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures.2=true;&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>.NET 4.7.2 より前のバージョンの Framework を対象にしながらこの変更を利用したい開発者は、次の AppContext フラグを設定する必要があります。すべてのフラグを適切に設定する必要があり、.NET Framework 4.7.2 以降のバージョンがインストールされている必要があることに注意してください。WPF アプリケーションで最新の機能強化を利用するには、以前のすべてのアクセシビリティ機能強化を利用する必要があります。 これを行うには、AppContext スイッチ "Switch.UseLegacyAccessibilityFeatures" と "Switch.UseLegacyAccessibilityFeatures.2" の両方を false に設定する必要があります。<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>|
|スコープ|エッジ|
|Version|4.7.2|
|型|再ターゲット中|
