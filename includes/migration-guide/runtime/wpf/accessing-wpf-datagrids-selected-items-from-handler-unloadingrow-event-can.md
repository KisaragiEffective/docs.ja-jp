---
ms.openlocfilehash: cda5df4b673412a7c8c59f80f89c19c13dc81dc1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67858513"
---
### <a name="accessing-a-wpf-datagrids-selected-items-from-a-handler-of-the-datagrids-unloadingrow-event-can-cause-a-nullreferenceexception"></a>DataGrid の UnloadingRow イベントのハンドラーから WPF DataGrid の選択された項目にアクセスすると、NullReferenceException が発生する可能性がある

|   |   |
|---|---|
|説明|.NET Framework 4.5 のバグが原因で、行の削除に関連する <xref:System.Windows.Controls.DataGrid> イベントのイベント ハンドラーにより、<xref:System.NullReferenceException?displayProperty=name> の <xref:System.Windows.Controls.DataGrid> または <xref:System.Windows.Controls.Primitives.Selector.SelectedItem?displayProperty=name> プロパティにアクセスする場合に <xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems?displayProperty=name> がスローされる可能性があります。|
|提案される解決策|この問題は .NET Framework 4.6 で修正されたため、このバージョンの .NET Framework にアップグレードすることによって対処できます。|
|スコープ|Minor|
|バージョン|4.5|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Windows.Controls.DataGrid.UnloadingRow?displayProperty=nameWithType></li><li><xref:System.Windows.Controls.DataGrid.UnloadingRowDetails?displayProperty=nameWithType></li></ul>|
