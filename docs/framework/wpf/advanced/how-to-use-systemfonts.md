---
title: '方法: SystemFonts を使用する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- system fonts [WPF]
- fonts [WPF], system fonts
- classes [WPF], SystemFonts
ms.assetid: 3f46a4ec-2225-408a-8123-8838a8f7057a
ms.openlocfilehash: 157565ceb9057049aef8b2bf274847d58c6b8dc8
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559964"
---
# <a name="how-to-use-systemfonts"></a>方法: SystemFonts を使用する
この例では、ボタンのスタイル設定やカスタマイズを行うために、<xref:System.Windows.SystemFonts> クラスの静的リソースを使用する方法を示します。  
  
## <a name="example"></a>使用例  
 システム リソースは、システム設定と一貫性のあるビジュアルを作成できるようにするために、いくつかのシステムによって決定される値を、リソースおよびプロパティの両方として公開します。 <xref:System.Windows.SystemFonts> は、静的なプロパティとしてのシステムフォント値と、実行時にそれらの値に動的にアクセスするために使用できるリソースキーを参照するプロパティの両方を含むクラスです。 たとえば、<xref:System.Windows.SystemFonts.CaptionFontFamily%2A> は <xref:System.Windows.SystemFonts> 値で、<xref:System.Windows.SystemFonts.CaptionFontFamilyKey%2A> は対応するリソースキーです。  
  
 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]では、<xref:System.Windows.SystemFonts> のメンバーを静的プロパティまたは動的リソース参照 (キーとして静的なプロパティ値を持つ) のいずれかとして使用できます。 アプリケーションの実行時にフォント メトリックを自動的に更新する場合は、動的リソース参照を使用します。それ以外の場合は、静的な値参照を使用します。  
  
> [!NOTE]
> リソース キーでは、プロパティ名に"Key"というサフィックスが付いています。  
  
 次の例では、ボタンのスタイル設定やカスタマイズを行うために、<xref:System.Windows.SystemFonts> のプロパティを静的な値としてアクセスして使用する方法を示します。 このマークアップの例では <xref:System.Windows.SystemFonts> 値をボタンに割り当てています。  
  
 [!code-xaml[SystemRes_snip#FontStaticResources](~/samples/snippets/csharp/VS_Snippets_Wpf/SystemRes_snip/CSharp/Pane1.xaml#fontstaticresources)]  
  
 コードで <xref:System.Windows.SystemFonts> の値を使用するには、静的な値と動的リソース参照のどちらを使用する必要もありません。 代わりに、<xref:System.Windows.SystemFonts> クラスの非キープロパティを使用します。 キー以外のプロパティは静的なプロパティとして定義されていますが、システムによってホストされている [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の実行時の動作によって、リアルタイムでプロパティが再評価され、ユーザーによるシステム値の変更が適切に考慮されます。 次の例では、ボタンのフォント設定を指定する方法を示します。  
  
 [!code-csharp[SystemRes_snip#FontResourcesCode](~/samples/snippets/csharp/VS_Snippets_Wpf/SystemRes_snip/CSharp/Pane1.xaml.cs#fontresourcescode)]
 [!code-vb[SystemRes_snip#FontResourcesCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SystemRes_snip/VisualBasic/Pane1.xaml.vb#fontresourcescode)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.SystemFonts>
- [システム ブラシで領域を塗りつぶす](../graphics-multimedia/how-to-paint-an-area-with-a-system-brush.md)
- [SystemParameters を使用する](how-to-use-systemparameters.md)
- [システム フォント キーを使用する](how-to-use-system-fonts-keys.md)
- [方法のトピック](resources-how-to-topics.md)
- [x:Static のマークアップ拡張機能](../../../desktop-wpf/xaml-services/xstatic-markup-extension.md)
- [XAML リソース](../../../desktop-wpf/fundamentals/xaml-resources-define.md)
- [DynamicResource マークアップ拡張](dynamicresource-markup-extension.md)
