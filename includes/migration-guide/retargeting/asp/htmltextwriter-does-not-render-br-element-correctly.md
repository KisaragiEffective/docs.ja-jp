---
ms.openlocfilehash: e600b8249096eecb13f63ea00343a771a8c12b60
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67804482"
---
### <a name="htmltextwriter-does-not-render-br-element-correctly"></a>HtmlTextWriter によって `<br/>` 要素が正しく表示されない

|   |   |
|---|---|
|説明|.NET Framework 4.6 以降では、<xref:System.Web.UI.HtmlTextWriter.RenderBeginTag(System.String)> と <xref:System.Web.UI.HtmlTextWriter.RenderEndTag> を <code>&lt;BR /&gt;</code> 要素を指定して呼び出すと、<code>&lt;BR /&gt;</code> を 1 つだけ (2 つではなく) 正しく挿入します。|
|提案される解決策|アプリが余分な <code>&lt;BR /&gt;</code> タグに依存している場合は、<xref:System.Web.UI.HtmlTextWriter.RenderBeginTag(System.String)> をもう一度呼び出す必要があります。 この動作の変更は、.NET Framework 4.6 以降を対象とするアプリにのみ影響するので、以前の動作を得るためには、以前のバージョンの .NET Framework を対象とするという方法もあります。|
|スコープ|エッジ|
|バージョン|4.6|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.Web.UI.HtmlTextWriter.RenderBeginTag(System.String)?displayProperty=nameWithType></li><li><xref:System.Web.UI.HtmlTextWriter.RenderBeginTag(System.Web.UI.HtmlTextWriterTag)?displayProperty=nameWithType></li></ul>|
