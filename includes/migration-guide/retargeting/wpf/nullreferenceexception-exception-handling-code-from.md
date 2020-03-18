---
ms.openlocfilehash: 824d75585efd40937c1a48376ec7862cba1a8fa3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "68235610"
---
### <a name="nullreferenceexception-in-exception-handling-code-from-imagesourceconverterconvertfrom"></a>ImageSourceConverter.ConvertFrom からの例外処理コード内の NullReferenceException

|   |   |
|---|---|
|説明|<xref:System.Windows.Media.ImageSourceConverter.ConvertFrom(System.ComponentModel.ITypeDescriptorContext,System.Globalization.CultureInfo,System.Object)> の例外処理コードのエラーでは、目的の例外 (<xref:System.NullReferenceException?displayProperty=name> または <xref:System.IO.DirectoryNotFoundException?displayProperty=name>) の代わりに <xref:System.IO.FileNotFoundException?displayProperty=name> が誤ってスローされていました。 この変更によりエラーが修正されるため、このメソッドは正しい例外をスローするようになりました。 <p/>既定では、.NET Framework 4.6.2 以前を対象とするすべてのアプリケーションが、引き続き互換性のために <xref:System.NullReferenceException?displayProperty=name> をスローします。 .NET Framework 4.7 以降を使用する開発者には、正しい例外が表示されます。|
|提案される解決策|NET Framework 4.7 以降を対象とする場合に <xref:System.NullReferenceException?displayProperty=name> を取得する構成に戻したい開発者は、アプリケーションの App.config ファイルに次のコードを追加/マージします。<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Media.ImageSourceConverter.OverrideExceptionWithNullReferenceException=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>|
|スコープ|エッジ|
|バージョン|4.7|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.Windows.Media.ImageSourceConverter.ConvertFrom(System.ComponentModel.ITypeDescriptorContext,System.Globalization.CultureInfo,System.Object)?displayProperty=nameWithType></li></ul>|
