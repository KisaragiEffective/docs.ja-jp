---
title: '方法 : コードを使用してイベント ハンドラーを追加する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- event handlers [WPF], adding
- XAML [WPF], adding event handlers
ms.assetid: 269c61e0-6bd9-4291-9bed-1c5ee66da486
ms.openlocfilehash: 457b8cf5c68096b20df7fe39f1cc3f40358f34d0
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73460432"
---
# <a name="how-to-add-an-event-handler-using-code"></a>方法 : コードを使用してイベント ハンドラーを追加する
この例では、コードを使用して、イベントハンドラーを要素に追加する方法を示します。  
  
 イベントハンドラーを [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 要素に追加し、その要素を含むマークアップページが既に読み込まれている場合は、コードを使用してハンドラーを追加する必要があります。 また、コードを使用してアプリケーションの要素ツリーを完全に構築し、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]を使用して要素を宣言しない場合は、特定のメソッドを呼び出して、構築された要素ツリーにイベントハンドラーを追加できます。  
  
## <a name="example"></a>例  
 次の例では、最初に [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]で定義された既存のページに新しい <xref:System.Windows.Controls.Button> を追加します。 分離コードファイルは、イベントハンドラーメソッドを実装し、そのメソッドを <xref:System.Windows.Controls.Button>の新しいイベントハンドラーとして追加します。  
  
 このC#例では、`+=` 演算子を使用して、ハンドラーをイベントに割り当てています。 これは、共通言語ランタイム (CLR) イベント処理モデルでハンドラーを割り当てるために使用される演算子と同じです。 Microsoft Visual Basic では、イベントハンドラーを追加する手段として、この演算子はサポートされていません。 代わりに、次の2つの方法のいずれかが必要です。  
  
- <xref:System.Windows.UIElement.AddHandler%2A> メソッドを `AddressOf` 演算子と共に使用して、イベントハンドラーの実装を参照します。  
  
- イベントハンドラー定義の一部として、`Handles` キーワードを使用します。 この手法は、ここでは示していません。「 [Visual Basic と WPF のイベント処理」を](visual-basic-and-wpf-event-handling.md)参照してください。  
  
 [!code-xaml[RoutedEventAddRemoveHandler#XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/RoutedEventAddRemoveHandler/CSharp/default.xaml#xaml)]  
  
 [!code-csharp[RoutedEventAddRemoveHandler#Handler](~/samples/snippets/csharp/VS_Snippets_Wpf/RoutedEventAddRemoveHandler/CSharp/default.xaml.cs#handler)]
 [!code-vb[RoutedEventAddRemoveHandler#Handler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/RoutedEventAddRemoveHandler/VisualBasic/default.xaml.vb#handler)]  
  
> [!NOTE]
> 最初に解析された [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ページでイベントハンドラーを追加する方がはるかに簡単です。 イベントハンドラーを追加するオブジェクト要素内で、処理するイベントの名前と一致する属性を追加します。 次に、[[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]] ページの分離コードファイルで定義したイベントハンドラーメソッドの名前として、その属性の値を指定します。 詳細については、「 [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md) 」または「[ルーティングイベントの概要](routed-events-overview.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [ルーティング イベントの概要](routed-events-overview.md)
- [方法トピック](events-how-to-topics.md)
