---
ms.openlocfilehash: e2d63d85adce64db6e00b62ec17f55ae71ce3052
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67803160"
---
### <a name="calling-datagridcommitedit-from-a-celleditending-handler-drops-focus"></a>CellEditEnding ハンドラーから DataGrid.CommitEdit を呼び出すと、フォーカスが削除される

|   |   |
|---|---|
|説明|<xref:System.Windows.Controls.DataGrid.CommitEdit> の <xref:System.Windows.Controls.DataGrid?displayProperty=name> イベント ハンドラーのいずれかから <xref:System.Windows.Controls.DataGrid.CellEditEnding?displayProperty=name> を呼び出すと、<xref:System.Windows.Controls.DataGrid?displayProperty=name> からフォーカスが失われます。|
|提案される解決策|このバグは .NET framework 4.5.2 で修正されたため、.NET Framework をアップグレードすることによって回避できます。 または、<xref:System.Windows.Controls.DataGrid?displayProperty=name> を呼び出した後で <xref:System.Windows.Controls.DataGrid.CommitEdit?displayProperty=name> を明示的に再選択することによって回避できます。|
|スコープ|エッジ|
|バージョン|4.5|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Windows.Controls.DataGrid.CommitEdit?displayProperty=nameWithType></li><li><xref:System.Windows.Controls.DataGrid.CommitEdit(System.Windows.Controls.DataGridEditingUnit,System.Boolean)?displayProperty=nameWithType></li></ul>|
