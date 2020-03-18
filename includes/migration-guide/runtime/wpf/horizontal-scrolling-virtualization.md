---
ms.openlocfilehash: ac7a56dc654ef4fd966077dd25012f0c50b0fc8d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67857541"
---
### <a name="horizontal-scrolling-and-virtualization"></a>水平方向スクロールと仮想化

|   |   |
|---|---|
|説明|この変更は、メインのスクロール方向と直交する方向で独自に仮想化を行う <xref:System.Windows.Controls.ItemsControl?displayProperty=name> に適用されます (代表的な例は、EnableColumnVirtualization=<xref:System.Windows.Controls.DataGrid?displayProperty=name>True&quot; である &quot; です)。  特定の水平方向スクロール操作の結果が変更され、比較可能な垂直操作の結果により類似した、より直感的な結果が生成されるようになりました。<p/>操作には &quot;ここにスクロール&quot; や &quot;右端&quot; などがあり、水平スクロール バーを右クリックすると表示されるメニューから名前を使用することができます。  これらの両方でオフセット候補が計算され、<xref:System.Windows.Controls.Primitives.IScrollInfo.SetHorizontalOffset(System.Double)> が呼び出されます。<p/>新しいオフセットにスクロールすると、&quot;ここ&quot; または &quot;右端&quot; の概念が変更されている場合があります。これは、新しく非仮想化されたコンテンツで <xref:System.Windows.Controls.Primitives.IScrollInfo.ExtentWidth?displayProperty=name> の値が変更されたためです。<p/>.NET Framework 4.6.2 より前では、もう &quot;ここ&quot; や &quot;右端&quot; にない場合でも、スクロール操作では単にオフセット候補を使用します。  その結果、スクロールつまみの &quot;バウンス&quot; などの効果が得られます。次に例を示します。 たとえば、<xref:System.Windows.Controls.DataGrid?displayProperty=name> で ExtentWidth=1000 および Width=200 であるものとします。  &quot;右端&quot; へのスクロールでは、1000 - 200 = 800 という候補オフセットを使用します。  そのオフセットへのスクロール時に、新しい列が非仮想化されます。これらの列が非常に広いため、<xref:System.Windows.Controls.Primitives.IScrollInfo.ExtentWidth?displayProperty=name> を 2000 に変更したとします。  スクロールは HorizontalOffset=800 で終了し、つまみはスクロールバーの中央付近 (正確には 800/2000 = 40%) に &quot;戻り&quot; ます。<p/>変更するには、この状況が発生したら新しいオフセット候補を再計算してやり直します (垂直方向スクロールの動作は既にこのようになっています)。 <p/>この変更により、エンド ユーザーにはより予測可能で直感的なエクスペリエンスが提供されるようになりますが、水平方向スクロール後の <xref:System.Windows.Controls.Primitives.IScrollInfo.HorizontalOffset?displayProperty=name> の正確な値に依存するアプリに影響する可能性もあります。これは、エンド ユーザーによる呼び出しであるか、<xref:System.Windows.Controls.Primitives.IScrollInfo.SetHorizontalOffset(System.Double)> の明示的な呼び出しであるかに関係ありません。|
|提案される解決策|<xref:System.Windows.Controls.Primitives.IScrollInfo.HorizontalOffset?displayProperty=name> の予測値を使用するアプリを、非仮想化により <xref:System.Windows.Controls.Primitives.IScrollInfo.ExtentWidth?displayProperty=name> が変更される可能性のある水平方向スクロール後の実際の値 (および <xref:System.Windows.Controls.Primitives.IScrollInfo.ExtentWidth?displayProperty=name> の値) を取得するように変更する必要があります。|
|スコープ|Minor|
|バージョン|4.6.2|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Windows.Controls.Primitives.IScrollInfo?displayProperty=nameWithType></li></ul>|
