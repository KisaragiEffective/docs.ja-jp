---
title: 技術領域の概要
ms.date: 03/30/2017
helpviewer_keywords:
- window regions [WPF]
- Win32 code [WPF], WPF interoperation
- Win32 code [WPF], airspace
- airspace [WPF]
- interoperability [WPF], airspace
- Win32 code [WPF], window regions
ms.assetid: b7cc350f-b9e2-48b1-be14-60f3d853222e
ms.openlocfilehash: 0b6ad222737f992da3074770fb5a2bff17860c00
ms.sourcegitcommit: 9a97c76e141333394676bc5d264c6624b6f45bcf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2020
ms.locfileid: "75740294"
---
# <a name="technology-regions-overview"></a>技術領域の概要
WPF、Win32、DirectX などのアプリケーションで複数のプレゼンテーションテクノロジが使用されている場合は、一般的なトップレベルウィンドウ内でレンダリング領域を共有する必要があります。 このトピックでは、WPF 相互運用アプリケーションのプレゼンテーションと入力に影響する可能性がある問題について説明します。  
  
## <a name="regions"></a>領域  
 最上位レベルのウィンドウでは、相互運用アプリケーションのいずれかのテクノロジを構成する各 HWND が独自のリージョン ("空域" とも呼ばれます) を持つことを概念にすることができます。 ウィンドウ内の各ピクセルは、その HWND の領域を構成する1つの HWND に属します。 (厳密に言えば、複数の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] HWND がある場合は、複数の [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 領域がありますが、この説明のために、1つだけを想定できます)。 この領域は、アプリケーションの有効期間中にそのピクセルの上にレンダリングを試みるすべてのレイヤーまたはその他のウィンドウが、同じレンダリングレベルのテクノロジに含まれる必要があることを意味します。 Win32 で [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ピクセルを表示しようとすると、望ましくない結果が発生し、相互運用 Api を使用して可能な限り許可されなくなります。  
  
### <a name="region-examples"></a>領域の例  
 次の図は、Win32、DirectX、および [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]を混在させるアプリケーションを示しています。 各テクノロジでは、重複しない個別のピクセルセットが使用され、リージョンに関する問題はありません。  
  
 ![Win32、DirectX、WPF を混在させるアプリケーションの例。](./media/technology-regions-overview/win32-directx-windows-presentation-foundation-application.png)  
  
 このアプリケーションがマウスポインターの位置を使用して、これら3つの領域のいずれかにレンダリングしようとするアニメーションを作成するとします。 どのテクノロジがアニメーション自体を担当するかにかかわらず、そのテクノロジは他の2つの領域に違反します。 次の図は、Win32 領域に WPF の円を表示しようとした場合を示しています。  
  
 ![Win32 領域の上に WPF の円を表示しようとしました。](./media/technology-regions-overview/render-windows-presentation-foundation-circle-over-win32-region.png)  
  
 異なるテクノロジ間で透明度/アルファブレンドを使用しようとすると、別の違反があります。  次の図では、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ボックスは Win32 と DirectX の領域に違反しています。 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ボックス内のピクセルは半透明であるため、DirectX と [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]の両方によって共同で所有されている必要がありますが、これは不可能です。  これは別の違反であるため、ビルドできません。  
  
 ![Win32 および DirectX 領域に違反している WPF ボックスを示す図。](./media/technology-regions-overview/windows-foundation-presentation-box-violate-win32-directx-region.png)  
  
 前の3つの例では、四角形の領域を使用していましたが、さまざまな形状が考えられます。  たとえば、領域には穴を付けることができます。 次の図は、四角形の穴を持つ Win32 領域を示しています。これは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] と DirectX 領域の組み合わせのサイズです。  
  
 ![四角形の穴を持つ Win32 領域を示す図。](./media/technology-regions-overview/win32-region-rectangular-hole.png)  
  
 領域は、完全に四角形ではなく、または Win32 HRGN (region) によって describable された任意の形でもかまいません。  
  
 ![四角形以外の領域を示す図。](./media/technology-regions-overview/nonrectangular-win32-region.png)  
  
## <a name="transparency-and-top-level-windows"></a>透明度とトップレベルウィンドウ  
 Windows のウィンドウマネージャーは、実際には Win32 Hwnd のみを処理します。 したがって、すべての [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] <xref:System.Windows.Window> は HWND です。 <xref:System.Windows.Window> HWND は、すべての HWND の一般的な規則に従う必要があります。 その HWND 内で [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] コードは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] Api 全体でサポートされているあらゆる操作を実行できます。 ただし、デスクトップ上の他の Hwnd との対話については、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は Win32 の処理と表示の規則を遵守する必要があります。  [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、Win32 Api を使用して四角形以外のウィンドウをサポートしています。つまり、四角形以外のウィンドウの場合は HRGNs、ピクセル単位のアルファの場合はレイヤードウィンドウです。  
  
 定数アルファおよびカラーキーはサポートされていません。  Win32 のレイヤードウィンドウの機能は、プラットフォームによって異なります。  
  
 レイヤードウィンドウでは、ウィンドウ内のすべてのピクセルに適用するアルファ値を指定することで、ウィンドウ全体を半透明 (半透明) にすることができます。  (実際には、Win32 ではピクセル単位のアルファがサポートされますが、このモードでは、ダイアログやドロップダウンを含む子 HWND を自分で描画する必要があるため、実際のプログラムでは使用するのが非常に困難です。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は HRGNs をサポートしています。ただし、この機能にはマネージ Api はありません。 プラットフォーム呼び出しと <xref:System.Windows.Interop.HwndSource> を使用して、関連する Win32 Api を呼び出すことができます。 詳細については、「[マネージコードからのネイティブ関数の呼び出し](/cpp/dotnet/calling-native-functions-from-managed-code)」を参照してください。  
  
 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] の階層化されたウィンドウの機能は、オペレーティングシステムによって異なります。 これは、[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] が DirectX を使用してレンダリングするためです。また、主にレイヤーウィンドウは、DirectX レンダリングではなく GDI レンダリング用に設計されています。  
  
- WPF は、ハードウェアアクセラレータによるレイヤードウィンドウをサポートしています。  
  
- [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、特にレンダリングがハードウェアアクセラレータを使用する場合に、要求した色を正確にレンダリングすることを保証できない [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ため、透明度カラーキーはサポートされません。  
  
## <a name="see-also"></a>関連項目

- [WPF と Win32 の相互運用性](wpf-and-win32-interoperation.md)
- [チュートリアル: Win32 での WPF クロックのホスト](walkthrough-hosting-a-wpf-clock-in-win32.md)
- [WPF での Win32 コンテンツのホスト](hosting-win32-content-in-wpf.md)
