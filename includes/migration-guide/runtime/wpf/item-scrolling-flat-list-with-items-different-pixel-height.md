---
ms.openlocfilehash: 3a82822aae281ea7e873ba649f05b1d68686ec69
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "70997655"
---
### <a name="item-scrolling-a-flat-list-with-items-of-different-pixel-height"></a>ピクセルの高さが異なる項目を含むフラット リストでの項目スクロール

|   |   |
|---|---|
|説明|<xref:System.Windows.Controls.ItemsControl?displayProperty=name> に仮想化 (<code>IsVirtualizing=true</code>) と項目スクロール (<code>ScrollUnit=Item</code>) を使うコレクションが表示される場合、およびコントロールがスクロールされ、近隣項目と異なる高さ (ピクセル単位) の項目が表示される場合、<xref:System.Windows.Controls.VirtualizingStackPanel?displayProperty=name> ではコレクション内のすべての項目に対してイテレーションが行われます。 このイテレーション中に UI は応答しません。コレクションのサイズが大きい場合、ハングとして認識される場合があります。 .NET Framework の以前のリリースであっても、このようなイテレーションは他の状況で発生します。 たとえば、これは、ピクセル スクロール (<code>ScrollUnit=Pixel</code>) が高さの異なる項目 (ピクセル単位) を検出した場合、および階層データの項目スクロール (<xref:System.Windows.Controls.TreeView?displayProperty=name> や、グループ化が有効になっている <xref:System.Windows.Controls.ItemsControl?displayProperty=name> など) が近隣項目と異なる数の子項目を持つ項目を検出した場合に発生します。項目スクロールと高さが異なる (ピクセル単位) 場合の対処方として、階層データのレイアウトのバグを修正できるようイテレーションが .NET Framework 4.6.1 で導入されました。  データがフラットである場合 (階層がない場合) は必要ないため、そのような状況では .NET Framework 4.6.2 はイテレーションを行いません。|
|提案される解決策|.NET Framework 4.6.1 でイテレーションが発生し、以前のリリースでは発生しない場合、つまり <xref:System.Windows.Controls.ItemsControl?displayProperty=name> が異なる高さの (ピクセル単位) の項目を含むフラットなリストを項目スクロールしている場合、実行できる対応策は以下の 2 つです。<ol><li>.NET Framework 4.6.2 をインストールします。</li><li>.NET Framework 4.6.1 の修正プログラム HR 1605 をインストールします。</li></ol>|
|スコープ|Minor|
|バージョン|4.6.1|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Windows.Controls.VirtualizingStackPanel?displayProperty=nameWithType></li></ul>|
