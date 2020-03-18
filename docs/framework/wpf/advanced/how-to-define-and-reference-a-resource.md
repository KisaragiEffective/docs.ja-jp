---
title: '方法 : リソースを定義および参照する'
ms.date: 03/30/2017
helpviewer_keywords:
- resources [WPF], defining
- defining resources [WPF]
- resources [WPF], referencing
- referencing resources [WPF]
ms.assetid: b86b876b-0a10-489b-9a5d-581ea9b32406
ms.openlocfilehash: 89471c45aabd9f3415c45a2e8af41d1a52900324
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73460178"
---
# <a name="how-to-define-and-reference-a-resource"></a>方法 : リソースを定義および参照する

この例では、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]の属性を使用してリソースを定義し、それを参照する方法を示します。

## <a name="example"></a>例

次の例では、2種類のリソースを定義しています。 <xref:System.Windows.Media.SolidColorBrush> リソースと、いくつかの <xref:System.Windows.Style> リソースです。 <xref:System.Windows.Media.SolidColorBrush> リソース `MyBrush` は、それぞれが <xref:System.Windows.Media.Brush> 型の値を受け取るいくつかのプロパティの値を提供するために使用されます。 <xref:System.Windows.Style> リソース `PageBackground`、`TitleText`、および `Label` は、特定のコントロールの種類を対象とします。 スタイルリソースがリソースキーによって参照され、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]で定義されているいくつかの特定のコントロール要素の <xref:System.Windows.FrameworkElement.Style%2A> プロパティを設定するために使用される場合、スタイルは対象のコントロールにさまざまなプロパティを設定します。

`Label` スタイルの setter 内のいずれかのプロパティも、前に定義した `MyBrush` リソースを参照していることに注意してください。 これは一般的な手法ですが、リソースが解析され、指定された順序でリソースディクショナリに入力されることに注意することが重要です。 また、 [StaticResource マークアップ拡張機能](staticresource-markup-extension.md)を使用して別のリソース内から参照する場合は、ディクショナリ内で見つかった順序によってリソースが要求されます。 参照するリソースがリソースコレクション内で前に定義されていることを確認してから、そのリソースを要求します。 必要に応じて、 [Dynamicresource マークアップ拡張機能](dynamicresource-markup-extension.md)を使用してリソース参照の厳密な作成順序を回避し、代わりに実行時にリソースを参照することができますが、この dynamicresource 手法はパフォーマンスを備えていることに注意する必要があります。措置. 詳細については、「 [XAML Resources](xaml-resources.md)」を参照してください。

[!code-xaml[FEResource#XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/FEResource/CS/default.xaml#xaml)]

## <a name="see-also"></a>関連項目

- [XAML リソース](xaml-resources.md)
- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
