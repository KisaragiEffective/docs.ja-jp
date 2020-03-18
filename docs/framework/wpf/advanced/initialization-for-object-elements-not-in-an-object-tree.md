---
title: オブジェクト ツリーに存在しないオブジェクト要素の初期化
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- logical tree [WPF]
- visual tree [WPF]
- elements [WPF], initializing
- initializing elements [WPF]
ms.assetid: 7b8dfc9b-46ac-4ce8-b7bb-035734d688b7
ms.openlocfilehash: 1a1d956ee7f41ac1ac0fc9bd051a18b9ff438930
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459836"
---
# <a name="initialization-for-object-elements-not-in-an-object-tree"></a>オブジェクト ツリーに存在しないオブジェクト要素の初期化
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] の初期化処理では、プロセスに処理を委任することがあり、そのプロセスは、一般的にその要素が論理ツリーまたはビジュアル ツリーのいずれかに接続されていることを前提としています。 このトピックでは、どちらのツリーにも接続されていない要素を初期化するために必要となる場合がある手順について説明します。  

## <a name="elements-and-the-logical-tree"></a>要素と論理ツリー  
 コード内で [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] クラスのインスタンスを作成するときは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] クラスのオブジェクト初期化処理の一部が、クラス コンストラクターの呼び出し時に実行されるコードから意図的に除外されていることに注意してください。 特に、コントロール クラスの場合は、コントロールの視覚的表現のほとんどが、コンストラクターでは定義されません。 代わりに、視覚的表現は、コントロールのテンプレートによって定義されます。 このテンプレートのソースには、さまざまなものがありますが、ほとんどの場合は、テーマ スタイルから取得されます。 テンプレートは、実質的には遅延バインディングです。そのコントロールがレイアウト可能になるまで、必要なテンプレートはコントロールに適用されません。 コントロールは、ルートでレンダリングするための表面に接続される論理ツリーに適用されるまで、レイアウトが可能になりません。 このルート レベル要素によって、論理ツリーで定義されているすべての子要素のレンダリングが開始されます。  
  
 このプロセスには、ビジュアル ツリーも関与します。 テンプレートによってビジュアル ツリーに組み込まれる要素も、接続されるまでは完全にインスタンス化されません。  
  
 この動作の影響で、要素の視覚的特性がすべて揃っていることを前提とする特定の操作には、追加の手順が必要になります。 構築されているが、まだツリーに適用されていないクラスの視覚的特性を取得しようとする場合などが、その例です。 たとえば、<xref:System.Windows.Media.Imaging.RenderTargetBitmap> で <xref:System.Windows.Media.Imaging.RenderTargetBitmap.Render%2A> を呼び出す必要があり、渡しているビジュアルがツリーに接続されていない要素である場合、追加の初期化手順が完了するまで、その要素は視覚的には完了しません。  
  
### <a name="using-begininit-and-endinit-to-initialize-the-element"></a>BeginInit と EndInit を使用して要素を初期化する  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] のさまざまなクラスは、<xref:System.ComponentModel.ISupportInitialize> インターフェイスを実装します。 インターフェイスの <xref:System.ComponentModel.ISupportInitialize.BeginInit%2A> および <xref:System.ComponentModel.ISupportInitialize.EndInit%2A> メソッドを使用して、初期化手順を含むコード内の領域を示します (表示に影響を与えるプロパティ値の設定など)。 シーケンス内で <xref:System.ComponentModel.ISupportInitialize.EndInit%2A> が呼び出されると、レイアウトシステムは要素を処理し、暗黙的なスタイルの検索を開始できます。  
  
 プロパティを設定する要素が <xref:System.Windows.FrameworkElement> または <xref:System.Windows.FrameworkContentElement> 派生クラスである場合は、<xref:System.ComponentModel.ISupportInitialize>にキャストするのではなく、<xref:System.Windows.FrameworkElement.BeginInit%2A> と <xref:System.Windows.FrameworkElement.EndInit%2A> のクラスバージョンを呼び出すことができます。  
  
### <a name="sample-code"></a>サンプル コード  
 次の例は、レンダリング Api を使用するコンソールアプリケーションのサンプルコードと、表示に影響を与えるプロパティを調整するその他の API 呼び出しの周囲の <xref:System.Windows.FrameworkElement.EndInit%2A> <xref:System.Windows.FrameworkElement.BeginInit%2A> の適切な配置を示すために [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ファイルの <xref:System.Windows.Markup.XamlReader.Load%28System.IO.Stream%29?displayProperty=nameWithType> を使用するコンソールアプリケーションのサンプルコードです。  
  
 この例では、main 関数のみを示します。 関数 `Rasterize` および `Save` (この例には示していません) は、イメージ処理および入出力を扱うユーティリティ関数です。  
  
 [!code-csharp[InitializeElements#Main](~/samples/snippets/csharp/VS_Snippets_Wpf/InitializeElements/CSharp/initializeelements.cs#main)]
 [!code-vb[InitializeElements#Main](~/samples/snippets/visualbasic/VS_Snippets_Wpf/InitializeElements/VisualBasic/initializeelements.vb#main)]  
  
## <a name="see-also"></a>関連項目

- [WPF のツリー](trees-in-wpf.md)
- [WPF グラフィックス レンダリングの概要](../graphics-multimedia/wpf-graphics-rendering-overview.md)
- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
