---
title: '方法: ページがブラウザーでホストされているかどうかを確認する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- hosted pages in browser [WPF]
- pages [WPF], hosted in browser
ms.assetid: 737e0f26-8371-49b4-9579-70879e51e1aa
ms.openlocfilehash: c4cb1065807d16c1d1f5a95c8ac9c9cbe5a0fdab
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73424691"
---
# <a name="how-to-determine-if-a-page-is-browser-hosted"></a>方法: ページがブラウザーでホストされているかどうかを確認する
この例では、<xref:System.Windows.Controls.Page> がブラウザーでホストされているかどうかを確認する方法を示します。  
  
## <a name="example"></a>例  
 <xref:System.Windows.Controls.Page> はホストに依存しない可能性があり、その結果、<xref:System.Windows.Controls.Frame>、<xref:System.Windows.Navigation.NavigationWindow>、ブラウザーなど、さまざまな種類のホストに読み込むことができます。 これは、1つまたは複数のページを含むライブラリアセンブリがあり、複数のスタンドアロンおよび参照可能な (XAML ブラウザーアプリケーション (XBAP)) ホストアプリケーションによって参照されている場合に発生する可能性があります。  
  
 次の例では、<xref:System.Windows.Interop.BrowserInteropHelper.IsBrowserHosted%2A?displayProperty=nameWithType> を使用して、<xref:System.Windows.Controls.Page> がブラウザーでホストされているかどうかを確認する方法を示します。  
  
 [!code-csharp[HOWTOBrowserInteropHelperSnippets#IsBrowserHostedCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/HOWTOBrowserInteropHelperSnippets/CSharp/Page1.xaml.cs#isbrowserhostedcode)]
 [!code-vb[HOWTOBrowserInteropHelperSnippets#IsBrowserHostedCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HOWTOBrowserInteropHelperSnippets/visualbasic/page1.xaml.vb#isbrowserhostedcode)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Controls.Frame>
- <xref:System.Windows.Controls.Page>
