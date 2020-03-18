---
title: スタイルでアニメーション化する方法
ms.date: 03/30/2017
helpviewer_keywords:
- animation [WPF], properties [WPF], within styles
- styles [WPF], animating properties within
ms.assetid: 6a791f3d-6b1f-4972-a2f9-35880bcfd954
ms.openlocfilehash: 0b29648bf15f0046adcdee610f9565f7deb24972
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744879"
---
# <a name="how-to-animate-in-a-style"></a>スタイルでアニメーション化する方法

この例では、スタイル内のプロパティをアニメーション化する方法を示します。 スタイル内でアニメーション化する場合、スタイルが定義されているフレームワーク要素のみを直接対象にできます。 Freezable オブジェクトをターゲットにするには、スタイルが指定された要素のプロパティから "ドットダウン" する必要があります。

次の例では、いくつかのアニメーションがスタイル内で定義され、<xref:System.Windows.Controls.Button>に適用されます。 ユーザーがボタンの上にマウスを移動すると、不透明から部分的に半透明にフェードし、もう一度繰り返し戻ることができます。 ユーザーがマウスポインターをボタンの外に移動すると、完全に不透明になります。 ボタンがクリックされると、背景色がオレンジ色から白に変わり、もう一度戻るようになります。 ボタンの描画に使用される <xref:System.Windows.Media.SolidColorBrush> を直接対象にすることはできません。そのため、ボタンの <xref:System.Windows.Controls.Control.Background%2A> プロパティから、このボタンにアクセスします。

## <a name="example"></a>例

[!code-xaml[timingbehaviors_snip#21](~/samples/snippets/csharp/VS_Snippets_Wpf/timingbehaviors_snip/CSharp/StyleStoryboardsExample.xaml#21)]

スタイル内でアニメーション化する場合は、存在しないオブジェクトを対象にすることができます。 たとえば、スタイルでボタンの background プロパティを設定するために <xref:System.Windows.Media.SolidColorBrush> を使用しているときに、ある時点でスタイルがオーバーライドされ、ボタンの背景が <xref:System.Windows.Media.LinearGradientBrush>で設定されているとします。  <xref:System.Windows.Media.SolidColorBrush> をアニメーション化しようとしても、例外はスローされません。アニメーションは、単純にサイレントモードで失敗します。

ストーリーボードターゲットの構文の詳細については、「[ストーリーボードの概要](storyboards-overview.md)」を参照してください。 アニメーションの詳細については、「[アニメーションの概要](animation-overview.md)」を参照してください。 スタイルの詳細については、「スタイル[とテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)」を参照してください。
