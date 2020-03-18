---
ms.openlocfilehash: 705e1a22b8a5791c1103dd374a8bab19356cadfb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67803214"
---
### <a name="ef-no-longer-throws-for-queryviews-with-specific-characteristics"></a>EF で特定の特性を持つ QueryViews がスローされなくなった

|   |   |
|---|---|
|説明|クエリの一部として関連エンティティを含めようとする 0..1 ナビゲーション プロパティを使用して QueryViews に関係するクエリをアプリが実行する場合、Entity Framework で <xref:System.StackOverflowException?displayProperty=name> 例外がスローされなくなりました。 たとえば、<code>.Include(e =&gt; e.RelatedNavProp)</code> を呼び出す場合です。|
|提案される解決策|この変更は、.Include を呼び出すクエリの実行時に 1 - 0..1 の関係にある QueryViews を使用するコードにのみ影響します。 これにより信頼性が向上し、ほとんどすべてのアプリに対して透過的になります。 ただし、予期しない動作が発生する場合、次のエントリをアプリの構成ファイルの <code>&lt;appSettings&gt;</code> セクションに追加することにより、この変更を無効にできます。<pre><code class="lang-xml">&lt;add key=&quot;EntityFramework_SimplifyUserSpecifiedViews&quot; value=&quot;false&quot; /&gt;&#13;&#10;</code></pre>|
|スコープ|エッジ|
|バージョン|4.5.2|
|[種類]|ランタイム|
