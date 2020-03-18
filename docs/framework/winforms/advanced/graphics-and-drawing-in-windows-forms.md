---
title: グラフィックスと描画
ms.date: 03/30/2017
helpviewer_keywords:
- graphics [Windows Forms]
- graphics [Windows Forms], using in Windows Forms
- GDI+, using in managed code
- drawing [Windows Forms]
ms.assetid: 362532c5-1a06-4257-bdc8-723461009ede
ms.openlocfilehash: 10ad18d38c84f6e447601ab6c8bf1a953dabb7cf
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746402"
---
# <a name="graphics-and-drawing-in-windows-forms"></a>Windows フォームにおけるグラフィックスと描画
共通言語ランタイムは、GDI + と呼ばれる Windows グラフィックスデバイスインターフェイス (GDI) の高度な実装を使用します。 GDI + を使用すると、グラフィックスの作成、テキストの描画、およびオブジェクトとしてのグラフィックイメージの操作を行うことができます。 GDI + は、パフォーマンスと使いやすさを提供するように設計されています。 GDI + を使用して、Windows フォームとコントロールにグラフィカルイメージを表示できます。 GDI + は Web フォームで直接使用することはできませんが、画像 Web サーバーコントロールを使用してグラフィカルイメージを表示できます。  
  
 このセクションでは、GDI + プログラミングの基礎について説明するトピックを紹介します。 このセクションは、包括的なリファレンスを意図したものではありませんが、<xref:System.Drawing.Graphics>、<xref:System.Drawing.Pen>、<xref:System.Drawing.Brush>、および <xref:System.Drawing.Color> の各オブジェクトに関する情報が含まれ、図形の描画、テキストの描画、イメージの表示などのタスクを実行する方法について説明します。 詳細については、「 [Gdi + リファレンス](/windows/desktop/gdiplus/-gdiplus-class-gdi-reference)」を参照してください。  
  
 すぐに開始する場合、「[グラフィックス プログラミングについて](getting-started-with-graphics-programming.md)」を参照してください。 Windows フォームで線、図形、テキストなどを描画するためのコードの使用方法に関するトピックがあります。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [グラフィックスの概要](graphics-overview-windows-forms.md)  
 グラフィックスに関連するマネージド クラスの概要を提供します。  
  
 [GDI+ マネージド コードについて](about-gdi-managed-code.md)  
 マネージ GDI + クラスに関する情報を提供します。  
  
 [マネージド グラフィックス クラスの使用](using-managed-graphics-classes.md)  
 GDI + マネージクラスを使用してさまざまなタスクを完了する方法を示します。  
  
## <a name="reference"></a>リファレンス  
 <xref:System.Drawing>  
 GDI + の基本的なグラフィックス機能へのアクセスを提供します。  
  
 <xref:System.Drawing.Drawing2D>  
 2 次元グラフィックスおよびベクター グラフィックス機能の詳細を提供します。  
  
 <xref:System.Drawing.Imaging>  
 高度な GDI + イメージング機能を提供します。  
  
 <xref:System.Drawing.Text>  
 高度な GDI + タイポグラフィ機能を提供します。 この名前空間のクラスを使用して、フォントのコレクションを作成して使用することができます。  
  
 <xref:System.Drawing.Printing>  
 印刷機能を提供します。  
  
## <a name="related-sections"></a>関連項目  
 [コントロールのカスタム描画およびレンダリング](../controls/custom-control-painting-and-rendering.md)  
 描画コントロールのコードを提供する方法について詳しく説明します。
