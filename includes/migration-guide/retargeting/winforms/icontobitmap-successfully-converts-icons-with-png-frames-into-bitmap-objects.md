---
ms.openlocfilehash: f4a5911787fa5f72be1dcd15c67b3f132c3f1110
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67804606"
---
### <a name="icontobitmap-successfully-converts-icons-with-png-frames-into-bitmap-objects"></a>PNG フレームを含んだアイコンが Icon.ToBitmap によって、Bitmap オブジェクトに正常に変換されます

|   |   |
|---|---|
|説明|.NET Framework 4.6 以降を対象にしたアプリでは、<xref:System.Drawing.Icon.ToBitmap%2A?displayProperty=nameWithType> メソッドで、PNG フレームを含んだアイコンを正常に Bitmap オブジェクトに変換できます。<p/>.NET Framework 4.5.2 以前のバージョンを対象としたアプリでは、Icon オブジェクトに PNG フレームが含まれていると、<xref:System.Drawing.Icon.ToBitmap%2A?displayProperty=nameWithType> メソッドが <xref:System.ArgumentOutOfRangeException> の例外をスローします。<p/>この変更は、.NET Framework 4.6 を対象として再コンパイルされたアプリのうち、Icon オブジェクトに PNG フレームが含まれている場合は <xref:System.ArgumentOutOfRangeException> をスローするように特別な処理が実装されているアプリに影響します。 .NET Framework 4.6 で実行している場合は、正常に変換が行われ、 <xref:System.ArgumentOutOfRangeException> がスローされることはないため、例外ハンドラーは呼び出されません。|
|提案される解決策|この動作に不都合がある場合は、次に示す要素を app.config ファイルの <code>&lt;runtime&gt;</code> セクションに追加することで、以前の動作を維持できます。<pre><code class="lang-xml">&lt;AppContextSwitchOverrides&#13;&#10;value=&quot;Switch.System.Drawing.DontSupportPngFramesInIcons=true&quot; /&gt;&#13;&#10;</code></pre>app.config ファイルに既に <code>AppContextSwitchOverrides</code> 要素が含まれている場合は、次に示すように新しい値を value 属性にマージする必要があります。<pre><code class="lang-xml">&lt;AppContextSwitchOverrides&#13;&#10;value=&quot;Switch.System.Drawing.DontSupportPngFramesInIcons=true;&lt;previous key&gt;=&lt;previous value&gt;&quot; /&gt;&#13;&#10;</code></pre>|
|スコープ|Minor|
|バージョン|4.6|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.Drawing.Icon.ToBitmap?displayProperty=nameWithType></li></ul>|
