---
title: インライン スタイルおよびテンプレート
ms.date: 03/30/2017
helpviewer_keywords:
- inline templates [WPF]
- styles [WPF], inline
- templates [WPF], inline
- inline styles [WPF]
ms.assetid: 69a1a3f9-acb5-4e2c-9c43-2e376c055ac4
ms.openlocfilehash: b88ef444283f4e1e85009c59b39f3cc41965d300
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73460003"
---
# <a name="inline-styles-and-templates"></a>インライン スタイルおよびテンプレート
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] には、リソース内の要素の外観を定義して複数回使用できるようにするための方法として、<xref:System.Windows.Style> オブジェクトとテンプレートオブジェクト (<xref:System.Windows.FrameworkTemplate> サブクラス) が用意されています。 このため、型 <xref:System.Windows.Style> と <xref:System.Windows.FrameworkTemplate> ほぼ同じである [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] の属性は、新しいものをインラインで定義するのではなく、常に既存のスタイルとテンプレートへのリソース参照を作成します。  
  
## <a name="limitations-of-inline-styles-and-templates"></a>インラインスタイルとテンプレートの制限事項  
 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)]では、スタイルとテンプレートのプロパティを技術的に次の2つの方法のいずれかで設定できます。 属性構文を使用すると、リソース内で定義されているスタイルを参照できます。たとえば、`<`*object*`Style="{StaticResource`*myResourceKey*`}" .../>`です。 または、プロパティ要素構文を使用して、次のようにスタイルインラインを定義することもできます。  
  
 `<`*オブジェクト*`>`  
  
 `<`*オブジェクト*`.Style>`  
  
 `<` `Style``.../>`  
  
 `</`*オブジェクト*`.Style>`  
  
 `</`*オブジェクト*`>`  
  
 属性の使用法は、より一般的です。 インラインで定義され、リソースで定義されていないスタイルは、必ず包含要素のみにスコープが設定され、リソースキーがないため簡単に再利用することはできません。 一般に、リソース定義のスタイルは汎用性が高く、便利であり、コード内のプログラムロジックをマークアップのデザインから分離するという、一般的な [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] プログラミングモデルの原則に準拠しています。  
  
 通常、スタイルまたはテンプレートをインラインで設定する理由はありません。その場所でそのスタイルまたはテンプレートを使用するだけの場合でも同様です。 スタイルまたはテンプレートを取得できる要素のほとんどは、コンテンツプロパティとコンテンツモデルもサポートしています。 スタイルまたはテンプレートを使用して作成した論理ツリーだけを使用している場合は、直接マークアップ内の同等の子要素を使用してそのコンテンツプロパティを簡単に設定できます。 これにより、スタイルとテンプレートのメカニズムが完全にバイパスされます。  
  
 オブジェクトを返すマークアップ拡張機能によって有効になるその他の構文は、スタイルやテンプレートにも使用できます。 このような2つの拡張機能には、 [TemplateBinding](templatebinding-markup-extension.md)と <xref:System.Windows.Data.Binding>があります。  
  
## <a name="see-also"></a>関連項目

- [スタイルとテンプレート](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
