---
title: OpenType フォント パックのサンプル
ms.date: 03/30/2017
helpviewer_keywords:
- OpenType font pack [WPF]
- fonts [WPF], OpenType font pack
- typography [WPF], OpenType font pack
ms.assetid: 56b46fa1-a44e-419b-8f14-25ad51c715c3
ms.openlocfilehash: be9bc6e0cddc0581b9acb319f7d1fc1c81dae268
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095113"
---
# <a name="sample-opentype-font-pack"></a>OpenType フォント パックのサンプル
このトピックでは、Windows SDK と共に配布されるサンプル OpenType フォントの概要について説明します。 サンプルフォントでは、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] アプリケーションで使用できる拡張 OpenType 機能がサポートされています。  

<a name="overview"></a>   
## <a name="fonts-in-the-opentype-font-pack"></a>OpenType フォント パックのフォント  
 Windows SDK には、[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] アプリケーションの作成に使用できる一連の OpenType フォントが用意されています。 サンプル フォントは、Ascender Corporation のライセンスを受けて提供されています。 これらのフォントは、OpenType 形式で定義されている合計機能のサブセットのみを実装します。 次の表に、サンプルの OpenType フォントの名前を示します。  
  
|**名前**|**[最近使ったファイル]**|  
|--------------|--------------|  
|Kootenay|Kooten.ttf|  
|Lindsey|Linds.ttf|  
|Miramonte|Miramo.ttf|  
|Miramonte Bold|Miramob.ttf|  
|Pericles|Peric.ttf|  
|Pericles Light|Pericl.ttf|  
|Pescadero|Pesca.ttf|  
|Pescadero Bold|Pescab.ttf|  
  
 次の図は、サンプルの OpenType フォントの外観を示しています。  
  
 ![サンプル フォント パック内のフォント名の一覧](./media/sample-opentype-font-pack/font-names-sample-pack.gif)  
  
 サンプル フォントは、Ascender Corporation のライセンスを受けて提供されています。 Ascender は、高度なフォント製品を提供する企業です。 サンプル フォントの拡張版またはカスタム版のライセンスを受けるには、[Ascender Corporation の Web サイト](https://www.monotype.com/)を参照してください。  
  
> [!NOTE]
> アプリケーションに埋め込む、または別の方法で再頒布するフォントについて、必要なライセンス権限を取得することは、開発者であるユーザーの責任で行ってください。  
  
<a name="installing_the_fonts"></a>   
## <a name="installing-the-fonts"></a>フォントのインストール  
 既定の Windows フォントディレクトリである **\WINDOWS\Fonts**にサンプルの OpenType フォントをインストールするオプションがあります。 フォントをインストールするには、コントロール パネルの [フォント] を使用します。 これらのフォントは、コンピューター上にあると、既定の Windows フォントを参照するすべてのアプリケーションからアクセスできるようになります。 フォント ファイルをダブルクリックして、各フォントの文字を異なるいくつかのフォント サイズで表示できます。 次のスクリーン ショットは、Lindsey フォント ファイル (Linds.ttf) を表示したものです。  
  
 ![Lindsey フォント &#40;OpenType&#41;](./media/typographyinwpf-04.png "TypographyInWPF_04")  
Lindsey フォントの表示  
  
<a name="using_the_fonts"></a>   
## <a name="using-the-fonts"></a>フォントの使用  
 アプリケーションでフォントを使用するには、2 とおりの方法があります。 アセンブリ内にリソースとして埋め込まれていないプロジェクト コンテンツ項目として、フォントをアプリケーションに追加できます。 あるいは、アプリケーションのアセンブリ ファイル内に埋め込まれたプロジェクト リソース項目として、フォントをアプリケーションに追加できます。 詳細については、「[アプリケーションでのフォントのパッケージング](packaging-fonts-with-applications.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Documents.Typography>
- [OpenType フォントの機能](opentype-font-features.md)
- [アプリケーションでのフォントのパッケージング](packaging-fonts-with-applications.md)
