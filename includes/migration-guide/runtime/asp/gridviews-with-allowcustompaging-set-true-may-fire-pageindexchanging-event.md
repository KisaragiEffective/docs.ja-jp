---
ms.openlocfilehash: c9c46793a0f66894649796d960547848ff5ebf8c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67858441"
---
### <a name="gridviews-with-allowcustompaging-set-to-true-may-fire-the-pageindexchanging-event-when-leaving-the-final-page-of-the-view"></a>AllowCustomPaging が true に設定された GridView では、ビューの最終ページから他に移動するときに、PageIndexChanging イベントが発生する場合がある

|   |   |
|---|---|
|説明|.NET Framework 4.5 でのバグのため、<xref:System.Web.UI.WebControls.GridView.PageIndexChanging?displayProperty=name> が有効になっている <xref:System.Web.UI.WebControls.GridView?displayProperty=name> に対して <xref:System.Web.UI.WebControls.GridView.AllowCustomPaging?displayProperty=name> が発生しないことがあります。|
|提案される解決策|この問題は .NET Framework 4.6 で修正されたため、このバージョンの .NET Framework にアップグレードすることによって対処できます。 回避策として、アプリでは、これらの条件 (<code>Page_Load</code> が最終ページで、最後の <xref:System.Web.UI.WebControls.GridView?displayProperty=name> が <xref:System.Web.UI.WebControls.GridView.PageSize?displayProperty=name> と異なる) を満たす <xref:System.Web.UI.WebControls.GridView.PageSize?displayProperty=name> で明示的に BindGrid を行うことができます。 または、(カスタム ページングではなく) ページングを許可するようにアプリを変更することもできます。このシナリオでは問題は発生していません。|
|スコープ|Minor|
|バージョン|4.5|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Web.UI.WebControls.GridView.AllowCustomPaging?displayProperty=nameWithType></li></ul>|
