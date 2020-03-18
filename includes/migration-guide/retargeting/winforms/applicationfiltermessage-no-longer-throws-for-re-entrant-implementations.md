---
ms.openlocfilehash: 8a1e2ca0790cb62e3c2c879f2ba0bb169ef07d77
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67804333"
---
### <a name="applicationfiltermessage-no-longer-throws-for-re-entrant-implementations-of-imessagefilterprefiltermessage"></a>Application.FilterMessage は、IMessageFilter.PreFilterMessage の再入可能な実装についてスローしなくなりました

|   |   |
|---|---|
|説明|.NET Framework 4.6.1 以前は、<xref:System.Windows.Forms.Application.FilterMessage(System.Windows.Forms.Message@)> または <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage(System.Windows.Forms.Message@)> (さらに <xref:System.Windows.Forms.Application.AddMessageFilter(System.Windows.Forms.IMessageFilter)?displayProperty=name>) を呼び出す<xref:System.Windows.Forms.Application.RemoveMessageFilter(System.Windows.Forms.IMessageFilter)?displayProperty=name> で <xref:System.Windows.Forms.Application.DoEvents> を呼び出すと、<xref:System.IndexOutOfRangeException?displayProperty=name> が発生していました。<p/>.NET Framework 4.6.1 以降を対象とするアプリケーションでは、この例外がスローされなくなり、上述の再入可能フィルターを使用できます。|
|提案される解決策|上述の再入可能な <xref:System.Windows.Forms.Application.FilterMessage(System.Windows.Forms.Message@)> の動作に対して <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage(System.Windows.Forms.Message@)> はスローされなくなります。 これは、.NET Framework 4.6.1 をターゲットとするアプリケーションにのみ影響します。 .NET Framework 4.6.1 を対象とするアプリは、[DontSupportReentrantFilterMessage](~/docs/framework/migration-guide/mitigation-custom-imessagefilter-prefiltermessage-implementations.md#mitigation) 互換性スイッチを使用することによって、この変更を解除できます (または、以前の Framework をターゲットとしているアプリは、変更を受け入れることができます)。|
|スコープ|エッジ|
|バージョン|4.6.1|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.Windows.Forms.Application.FilterMessage(System.Windows.Forms.Message@)?displayProperty=nameWithType></li></ul>|
