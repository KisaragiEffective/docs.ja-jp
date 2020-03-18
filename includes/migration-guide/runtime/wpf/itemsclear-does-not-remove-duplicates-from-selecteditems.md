---
ms.openlocfilehash: 1545c807e3bef675e63e14d01ab82c1131600f39
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67857499"
---
### <a name="itemsclear-does-not-remove-duplicates-from-selecteditems"></a>Items.Clear で SelectedItems から重複部分が削除されない

|   |   |
|---|---|
|説明|セレクター (複数選択が有効になっている) の <xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems?displayProperty=name> コレクションに重複部分があるとします。その場合、同じ項目が複数回表示されます。  データ ソースからこれらの項目を削除すると (たとえば、Items.Clear を呼び出すことにより)、<xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems?displayProperty=name> からそれらの項目を削除できなくなります。最初のインスタンスのみが削除されます。 さらに、<xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems?displayProperty=name> (SelectedItems.Clear() など) を引き続き使用すると、<xref:System.ArgumentException?displayProperty=name> などの問題が発生する可能性があります。これは、<xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems?displayProperty=name> に、データ ソース内にはもう存在しない項目が含まれているためです。|
|提案される解決策|可能であれば、.NET Framework 4.6.2 にアップグレードします。|
|スコープ|Minor|
|バージョン|4.5|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems?displayProperty=nameWithType></li></ul>|
