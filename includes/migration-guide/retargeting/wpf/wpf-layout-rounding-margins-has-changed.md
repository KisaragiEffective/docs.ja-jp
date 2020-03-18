---
ms.openlocfilehash: 73096e5f61e5257e062df9743cae0f5464892357
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67804517"
---
### <a name="wpf-layout-rounding-of-margins-has-changed"></a>WPF レイアウトでの余白の丸め方の変更

|   |   |
|---|---|
|説明|余白、境界線、およびそれらの内部の背景の丸め処理の方法が変更されました。 この変更の結果、以下のようになります。<ul><li>要素の幅または高さが最大で 1 ピクセル拡大または縮小することがあります。</li><li>オブジェクトの配置が最大で 1 ピクセル移動することがあります。</li><li>中央揃えの要素が中央から最大で 1 ピクセル垂直まは水平方向にずれることがあります。</li></ul>既定では、この新しいレイアウトは .NET Framework の 4.6 を対象とするアプリに対してのみ有効となります。|
|提案される解決策|この変更では、DPI が高いときにWPF コントロールの一番右または一番下でクリッピングの発生を除去する傾向があるため、app.config ファイルの <code>&lt;runtime&gt;</code> セクションに次の行を追加することによって、以前のバージョンの .NET Framework を対象としながら .NET Framework 4.6 上で実行されているアプリがこの新しい動作を選択ことができます。<pre><code class="lang-xml">&lt;AppContextSwitchOverrides value=&quot;Switch.MS.Internal.DoNotApplyLayoutRoundingToMarginsAndBorderThickness=false&quot; /&gt;&#39;&#13;&#10;</code></pre>.NET Framework 4.6 を対象としながら以前のレイアウト アルゴリズムを使用して WPF コントロールをレンダリングする必要があるアプリの場合、app.config ファイルの <code>&lt;runtime&gt;</code> セクションに次の行を追加することによってそれを行うことができます。<pre><code class="lang-xml">&lt;AppContextSwitchOverrides value=&quot;Switch.MS.Internal.DoNotApplyLayoutRoundingToMarginsAndBorderThickness=true&quot; /&gt;&#39;.&#13;&#10;</code></pre>|
|スコープ|Minor|
|バージョン|4.6|
|[種類]|再ターゲット中|
