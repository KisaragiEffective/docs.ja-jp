---
ms.openlocfilehash: f814703e187726d3988787fac52e5049988fd4d7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67803557"
---
### <a name="workflow-xaml-checksums-for-symbols-changed-from-sha1-to-sha256"></a>ワークフロー XAML のシンボルのチェックサムが SHA1 から SHA256 に変更

|   |   |
|---|---|
|説明|Visual Studio によるデバッグをサポートするために、ワークフロー ランタイムによって、ハッシュ アルゴリズムを使用してワークフロー XAML ファイルのチェックサムが生成されます。 .NET Framework 4.6.2 以前のバージョンでは、ワークフロー チェックサムのハッシュで MD5 アルゴリズムが使用され、FIPS 対応システムで問題が発生していました。 .NET Framework 4.7 以降では、既定のアルゴリズムが SHA1 に変更されました。 .NET Framework 4.8 以降では、既定のアルゴリズムが SHA256 に変更されました。|
|提案される解決策|チェックサム エラーに起因して、コードでワークフロー インスタンスを読み込めないか、適切なシンボルを見つけられない場合、<code>AppContext</code> スイッチ &quot;Switch.System.Activities.UseSHA1HashForDebuggerSymbols&quot; を true に設定してみてください。コードは次のとおりです。<pre><code class="lang-csharp">System.AppContext.SetSwitch(&quot;Switch.System.Activities.UseSHA1HashForDebuggerSymbols&quot;, true);&#13;&#10;</code></pre>または、次のように構成します。<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Activities.UseSHA1HashForDebuggerSymbols=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>|
|スコープ|Minor|
|バージョン|4.8|
|[種類]|再ターゲット中|
