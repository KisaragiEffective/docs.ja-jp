---
title: mc:ProcessContent 属性
ms.date: 03/30/2017
helpviewer_keywords:
- mc:ProcessContent attribute
- XAML [WPF], mc:ProcessContent attribute
ms.assetid: 2689b2c8-b4dc-4b71-b9bd-f95e619122d7
ms.openlocfilehash: bcf55668bdc70902e346c401549a88f6ccb9072e
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77095126"
---
# <a name="mcprocesscontent-attribute"></a>mc:ProcessContent 属性
[Mc: Ignorable 属性](mc-ignorable-attribute.md)を指定することにより、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] のプロセッサによって直接の親要素が無視される可能性がある場合でも、関連する親要素によって処理されるコンテンツを保持する [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 要素を指定します。 `mc:ProcessContent` 属性は、カスタム名前空間マッピングと [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] バージョン管理の両方で、マークアップ互換性をサポートしています。  
  
## <a name="xaml-attribute-usage"></a>XAML 属性の使用  
  
```xaml  
<object  
  xmlns:ignorablePrefix="ignorableUri"  
  xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"  
  mc:Ignorable="ignorablePrefix"...  
  mc:ProcessContent="ignorablePrefix:ThisElementCanBeIgnored"  
>  
    <ignorablePrefix:ThisElementCanBeIgnored>  
        [content]  
    </ignorablePrefix:ThisElementCanBeIgnored>  
</object>  
```  
  
## <a name="xaml-values"></a>XAML の値  
  
|||  
|-|-|  
|*ignorablePrefix*|XML 1.0 仕様に従って、任意の有効なプレフィックス文字列。|  
|*ignorableUri*|XML 1.0 仕様に従って、名前空間を指定するための任意の有効な URI。|  
|*ThisElementCanBeIgnored*|基になる型を解決できない場合に [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] のプロセッサ実装によって無視される要素。|  
|*情報*|*ThisElementCanBeIgnored*は無視としてマークされます。 プロセッサがその要素を無視した場合、 *[content]* は*オブジェクト*によって処理されます。|  
  
## <a name="remarks"></a>解説  
 既定では、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] のプロセッサは無視された要素内のコンテンツを無視します。 `mc:ProcessContent`によって特定の要素を指定できます。また、[!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] プロセッサは、無視された要素内のコンテンツを処理し続けます。 これは通常、コンテンツが複数のタグで入れ子になっている場合に使用されます。少なくとも1つは無視可能で、少なくとも1つは無視可能ではありません。  
  
 属性では、複数のプレフィックスを指定できます。たとえば、`mc:ProcessContent="ignore:Element1 ignore:Element2"`のようにスペース区切り文字を使用します。  
  
 `http://schemas.openxmlformats.org/markup-compatibility/2006` 名前空間は、SDK のこの領域内には記載されていない他の要素と属性を定義します。 詳細については、「 [XML マークアップ互換性の仕様](https://docs.microsoft.com/office/open-xml/introduction-to-markup-compatibility#markup-compatibility-in-the-open-xml-file-formats-specification)」を参照してください。  
  
## <a name="see-also"></a>参照

- [mc:Ignorable 属性](mc-ignorable-attribute.md)
- [XAML の概要 (WPF)](../../../desktop-wpf/fundamentals/xaml.md)
