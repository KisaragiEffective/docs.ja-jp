---
ms.openlocfilehash: b92086c8ccf7592ce70b75bd31d4ea255c35b543
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67803465"
---
### <a name="hwndhost-now-correctly-resizes-child-hwnd-during-dpi-changes"></a>HwndHost で DPI 変更中に子の HWND のサイズが正しく変更されるようになった

|   |   |
|---|---|
|説明|.NET Framework 4.7.2 以前のバージョンでは、WPF がモニターごとの認識モードで実行されていた場合、モニター間でアプリケーションを移動するときなど、DPI の変更後に、<xref:System.Windows.Interop.HwndHost> 内でホストされていたコントロールのサイズが正しく変更されませんでした。 この修正により、確実に、ホストされるコントロールのサイズが適切に変更されるようになります。|
|提案される解決策|アプリケーションでこれらの変更を利用するには、.NET Framework 4.7.2 以降で実行する必要があり、以下の例のように、アプリ構成ファイルの [ セクションの次の ](https://docs.microsoft.com/dotnet/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element)AppContext スイッチ<code>&lt;runtime&gt;</code>を <code>false</code> に設定し、この動作を選択する必要があります。<pre><code class="lang-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;&#13;&#10;&lt;configuration&gt;&#13;&#10;&lt;startup&gt;&#13;&#10;&lt;supportedRuntime version=&quot;v4.0&quot; sku=&quot;.NETFramework,Version=v4.7&quot;/&gt;&#13;&#10;&lt;/startup&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;!-- AppContextSwitchOverrides value attribute is in the form of &#39;key1=true/false;key2=true/false  --&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.DoNotUsePresentationDpiCapabilityTier2OrGreater=false&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>|
|スコープ|Major|
|バージョン|4.8|
|[種類]|再ターゲット中|
