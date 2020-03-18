---
title: Windows ランタイムへの URI の引き渡し
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows Runtime, .NET Framework support for
- Windows Runtime, passing a URI to
ms.assetid: 3eb5ce6f-f304-4f87-8e81-0f25092f5ad4
ms.openlocfilehash: 71d427c2d602efbd92dc0128b1fada85a987904a
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77123624"
---
# <a name="passing-a-uri-to-the-windows-runtime"></a>Windows ランタイムへの URI の引き渡し
Windows ランタイムのメソッドは絶対 URI だけを受け取ります。 相対 URI を Windows ランタイムメソッドに渡すと、<xref:System.ArgumentException> 例外がスローされます。 その理由: .NET Framework コードで Windows ランタイムを使用すると、<xref:Windows.Foundation.Uri?displayProperty=nameWithType> クラスが Intellisense で <xref:System.Uri?displayProperty=nameWithType> として表示されます。 <xref:System.Uri?displayProperty=nameWithType> クラスでは相対 Uri を使用できますが、<xref:Windows.Foundation.Uri?displayProperty=nameWithType> クラスでは許可されません。 これは、Windows ランタイムコンポーネントで公開するメソッドにも当てはまります。 URI を受け取るメソッドをコンポーネントで公開する場合、コードのシグネチャには <xref:System.Uri?displayProperty=nameWithType> が含まれます。 ただし、コンポーネントのユーザーには、署名に <xref:Windows.Foundation.Uri?displayProperty=nameWithType>が含まれています。 コンポーネントに渡す URI は、絶対 URI でなければなりません。  
  
このトピックでは、絶対 URI を検出する方法と、アプリ パッケージ内のリソースを参照するときに絶対 URI を作成する方法を説明します。  
  
## <a name="detecting-and-using-an-absolute-uri"></a>絶対 URI の検出と使用  
Windows ランタイムに渡す前に URI が絶対 URI であることを確認するには、<xref:System.Uri.IsAbsoluteUri%2A?displayProperty=nameWithType> プロパティを使用します。 このプロパティを使用する方が、<xref:System.ArgumentException> 例外をキャッチして処理するより効率的です。  
  
## <a name="using-an-absolute-uri-for-a-resource-in-the-app-package"></a>アプリ パッケージのリソースに対する絶対 URI の使用  
アプリのパッケージに含まれるリソースに対して URI を指定するには、`ms-appx` スキームまたは `ms-appx-web` スキームを使用して絶対 URI を作成します。  
  
次の例では、XAML とコードの両方を使用して、ページという名前のフォルダーに格納されているリソースに対して、<xref:Windows.UI.Xaml.Controls.WebView> コントロールの <xref:Windows.UI.Xaml.Controls.WebView.Source%2A> プロパティと <xref:Windows.UI.Xaml.Controls.Image> コントロールの <xref:Windows.UI.Xaml.Controls.Image.Source%2A> プロパティを設定する方法を示します。  
  
[!code-xaml[System.URIToWindowsURI#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.uritowindowsuri/cs/mainpage.xaml#1)]  
[!code-csharp[System.URIToWindowsURI#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.uritowindowsuri/cs/mainpage.xaml.cs#2)]
[!code-vb[System.URIToWindowsURI#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.uritowindowsuri/vb/mainpage.xaml.vb#2)]  
  
これらのスキームの詳細については、Windows デベロッパーセンターの「 [URI スキーム](/windows/uwp/app-resources/uri-schemes)」を参照してください。  
  
## <a name="see-also"></a>参照

- [Windows ストア アプリおよび Windows ランタイムのための .NET Framework サポート](../../../docs/standard/cross-platform/support-for-windows-store-apps-and-windows-runtime.md)
