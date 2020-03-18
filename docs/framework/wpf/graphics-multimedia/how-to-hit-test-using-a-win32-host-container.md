---
title: '方法 : Win32 ホスト コンテナーを使用してヒット テストを実行する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- hit tests [WPF], using Win32 host containers
- visual objects [WPF], hit tests on
- Win32 host containers [WPF], hit tests using
ms.assetid: 9491f7f3-d8ba-4573-a888-2f064d1349dc
ms.openlocfilehash: a86c1c36f75fa232d52731959371268a8b2593d7
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452806"
---
# <a name="how-to-hit-test-using-a-win32-host-container"></a>方法 : Win32 ホスト コンテナーを使用してヒット テストを実行する
ビジュアルオブジェクトのホストウィンドウコンテナーを指定することにより、Win32 ウィンドウ内にビジュアルオブジェクトを作成できます。 格納されているビジュアル オブジェクト用のイベント処理機能を提供するには、ホスト ウィンドウ コンテナーのメッセージ フィルター ループに渡されるメッセージを処理します。 Win32 ウィンドウでビジュアルオブジェクトをホストする方法の詳細については[、「チュートリアル: Win32 アプリケーションでのビジュアルオブジェクト](tutorial-hosting-visual-objects-in-a-win32-application.md)のホスト」を参照してください。  
  
## <a name="example"></a>例  
 次のコードは、ビジュアルオブジェクトのホストコンテナーとして使用される Win32 ウィンドウのマウスイベントハンドラーを設定する方法を示しています。  
  
 [!code-csharp[VisualsHitTesting#103](~/samples/snippets/csharp/VS_Snippets_Wpf/VisualsHitTesting/CSharp/MyWindow.cs#103)]
 [!code-vb[VisualsHitTesting#103](~/samples/snippets/visualbasic/VS_Snippets_Wpf/VisualsHitTesting/VisualBasic/MyWindow.vb#103)]  
  
 次の例は、特定のマウスイベントのトラップに応答してヒットテストを設定する方法を示しています。  
  
 [!code-csharp[VisualsHitTesting#104](~/samples/snippets/csharp/VS_Snippets_Wpf/VisualsHitTesting/CSharp/MyCircle.cs#104)]
 [!code-vb[VisualsHitTesting#104](~/samples/snippets/visualbasic/VS_Snippets_Wpf/VisualsHitTesting/VisualBasic/MyCircle.vb#104)]  
  
 <xref:System.Windows.Interop.HwndSource> オブジェクトは、Win32 ウィンドウ内で [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] コンテンツを表示します。 <xref:System.Windows.Interop.HwndSource> オブジェクトの <xref:System.Windows.Interop.HwndSource.RootVisual%2A> プロパティの値は、ビジュアルツリー階層内の最上位ノードを表します。  
  
 Win32 ホストコンテナーを使用したヒットテストオブジェクトの完全なサンプルについては、「 [Win32 相互運用のヒットテストのサンプル](https://github.com/microsoft/WPF-Samples/tree/master/Visual%20Layer/VisualsHitTesting)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Interop.HwndSource>
- [ビジュアル層でのヒット テスト](hit-testing-in-the-visual-layer.md)
- [チュートリアル : Win32 アプリケーションでのビジュアル オブジェクトのホスト](tutorial-hosting-visual-objects-in-a-win32-application.md)
