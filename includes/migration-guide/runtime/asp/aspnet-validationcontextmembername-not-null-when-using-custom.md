---
ms.openlocfilehash: c0be08023f80bf0f96cc08f34b9ea8c5a73839e3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77466043"
---
### <a name="aspnet-validationcontextmembername-is-not-null-when-using-custom-dataannotationsvalidationattribute"></a>ASP.NET カスタムの DataAnnotations.ValidationAttribute を使用すると、ValidationContext.MemberName が NULL にならない

|   |   |
|---|---|
|説明|.NET Framework 4.7.2 以前のバージョンでは、カスタムの <xref:System.ComponentModel.DataAnnotations.ValidationAttribute?displayProperty=nameWithType> を使用すると、<xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName?displayProperty=nameWithType> プロパティで `null` が返されます。 October 2019 Update より前の .NET Framework 4.8 バージョンでは、メンバー名が返されます。 .NET Framework 4.8 の [.NET Framework October 2019 Preview の品質ロールアップ](https://devblogs.microsoft.com/dotnet/net-framework-october-2019-preview-of-quality-rollup/)以降、既定では `null` が返されますが、代わりにメンバー名を返すように設定することもできます。 |
|提案される解決策|.NET Framework 4.8 以降のバージョン用の *.NET Framework October 2019 Preview の品質ロールアップ*で、メンバー名を返すプロパティに対して次の設定を [web.config](https://devblogs.microsoft.com/dotnet/net-framework-october-2019-preview-of-quality-rollup/) ファイルに追加します。<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;appSettings&gt;&#13;&#10;...&#13;&#10;&lt;add key=&quot;aspnet:GetValidationMemberName&quot;  value=&quot;true&quot;/&gt;&#13;&#10;...&#13;&#10;&lt;/appSettings&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>October 2019 Update より前の .NET Framework 4.8 バージョンでは、これを *web.config* ファイルに追加すると、以前の動作が復元され、プロパティで `null` が返されます。|
|スコープ|不明|
|バージョン|4.8|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName?displayProperty=nameWithType></li></ul>|
