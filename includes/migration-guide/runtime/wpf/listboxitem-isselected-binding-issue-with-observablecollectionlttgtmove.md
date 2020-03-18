---
ms.openlocfilehash: 44cb833fc93caaa79000147421e1c013f755b9cb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "68238021"
---
### <a name="listboxitem-isselected-binding-issue-with-observablecollectionlttgtmove"></a>ObservableCollection&lt;T&gt;.Move に対する ListBoxItem IsSelected のバインディングの問題

|   |   |
|---|---|
|説明|項目が選択された <xref:System.Collections.ObjectModel.ObservableCollection%601.Move(System.Int32,System.Int32)> にバインドされるコレクションで <xref:System.Collections.ObjectModel.ObservableCollection%601.MoveItem(System.Int32,System.Int32)> または <xref:System.Windows.Controls.ListBox?displayProperty=name> を呼び出すと、<xref:System.Windows.Controls.ListBox?displayProperty=name> 項目の今後の選択または選択解除で不規則な動作が発生する可能性があります。|
|提案される解決策|<xref:System.Collections.ObjectModel.Collection%601.Remove(%600)?displayProperty=name> ではなく <xref:System.Collections.ObjectModel.Collection%601.Insert(System.Int32,%600)?displayProperty=name> と <xref:System.Collections.ObjectModel.ObservableCollection%601.Move(System.Int32,System.Int32)> を呼び出すことで、この問題は解決されます。 または、この問題は .NET Framework 4.6 で修正されたため、このバージョンの .NET Framework にアップグレードすることによって対処できます。|
|スコープ|Minor|
|バージョン|4.5|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Collections.ObjectModel.ObservableCollection%601.Move(System.Int32,System.Int32)?displayProperty=nameWithType></li><li><xref:System.Collections.ObjectModel.ObservableCollection%601.MoveItem(System.Int32,System.Int32)?displayProperty=nameWithType></li></ul>|
