---
ms.openlocfilehash: 94c582d25ae1cd2249ed2e3774134a86cf77327b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73085539"
---
### <a name="dataannotationsdatatypeattributedisableregex-app-setting-is-on-by-default-in-net-framework-472"></a>.NET Framework 4.7.2 で "dataAnnotations:dataTypeAttribute:disableRegEx" アプリ設定が既定で有効になっている

|   |   |
|---|---|
|説明|.NET framework 4.6.1 で、データ型属性 (<code>&quot;dataAnnotations:dataTypeAttribute:disableRegEx&quot;</code>、<xref:System.ComponentModel.DataAnnotations.EmailAddressAttribute?displayProperty=nameWithType>、<xref:System.ComponentModel.DataAnnotations.UrlAttribute?displayProperty=nameWithType> など) で正規表現の使用を無効にするアプリ設定 (<xref:System.ComponentModel.DataAnnotations.PhoneAttribute?displayProperty=nameWithType>) が導入されました。 これにより、特定の正規表現を使用するサービス拒否攻撃の可能性を回避できるなど、セキュリティの脆弱性を軽減できます。<br/>.NET Framework 4.6.1 で、正規表現の使用を無効にするこのアプリ設定が、既定で <code>false</code> に設定されました。 .NET Framework 4.7.2 からは、この構成スイッチが既定で <code>true</code> に設定され、.NET Framework 4.7.2 以降を対象とする Web アプリケーションのセキュリティの脆弱性がさらに軽減されています。|
|提案される解決策|.NET Framework 4.7.2 へのアップグレード後に Web アプリケーションの正規表現が動作しない場合は、<code>&quot;dataAnnotations:dataTypeAttribute:disableRegEx&quot;</code> 設定の値を <code>false</code> に更新して以前の動作に戻すことができます。<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;appSettings&gt;&#13;&#10;...&#13;&#10;&lt;add key=&quot;dataAnnotations:dataTypeAttribute:disableRegEx&quot; value=&quot;false&quot;/&gt;&#13;&#10;...&#13;&#10;&lt;/appSettings&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>|
|スコープ|Minor|
|バージョン|4.7.2|
|[種類]|ランタイム|
