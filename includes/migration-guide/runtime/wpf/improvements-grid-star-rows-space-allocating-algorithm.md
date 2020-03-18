---
ms.openlocfilehash: 8e0c934cd8c3cb8c08a526c7e145436db6ecf4a5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "68237617"
---
### <a name="improvements-to-grid-star-rows-space-allocating-algorithm"></a>グリッド スター行領域の割り当てアルゴリズムの改善

|   |   |
|---|---|
|説明|.NET Framework 4.7 で導入された [ で](https://github.com/Microsoft/dotnet/blob/master/Documentation/compatibility/wpf-grid-allocation-of-space-to-star-columns.md)サイズを割り当てるためのアルゴリズム<xref:System.Windows.Controls.Grid>) のバグを修正しました。  空の行を含む <code>Height=&quot;Auto&quot;</code> のグリッドなど、場合によっては、行が正しくない位置に配置され、すべてグリッドの外側に示されることがありました。|
|提案される解決策|アプリケーションでこれらの変更を利用するには、.NET Framework 4.8 以降で実行する必要があります。|
|スコープ|Major|
|バージョン|4.8|
|[種類]|ランタイム|
