---
title: XML へのアクセス
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ to XML [Visual Basic], accessing XML
- Visual Basic code, XML
- accessing XML [Visual Basic], axis properties
- XML [Visual Basic], axis properties
- XML [Visual Basic], accessing
ms.assetid: c47f88b2-3cbc-4bb1-b4b9-be60f71ffc6a
ms.openlocfilehash: 1fa1d94fc710272ac0ba9ea7167989da42a51fcd
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351749"
---
# <a name="accessing-xml-in-visual-basic"></a>Visual Basic での XML へのアクセス
Visual Basic には、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 構造体にアクセスして移動するための XML 軸プロパティが用意されています。 これらのプロパティでは、XML 名を指定して要素と属性にアクセスできるように、特殊な構文を使用します。  
  
 次の表に、Visual Basic の XML 要素と属性にアクセスできるようにする言語機能を示します。  
  
### <a name="xml-axis-properties"></a>XML 軸プロパティ  
  
|プロパティの説明|例|説明|  
|--------------------------|-------------|-----------------|  
|*子軸*|`contact.<phone>`|`contact` 要素の子要素であるすべての `phone` 要素を取得します。|  
|*属性軸*|`phone.@type`|`phone` 要素のすべての `type` 属性を取得します。|  
|*子孫軸*|`contacts...<name>`|発生した階層の深さに関係なく、`contacts` 要素のすべての `name` 要素を取得します。|  
|*拡張機能のインデクサー*|`contacts...<name>(0)`|シーケンスから最初の `name` 要素を取得します。|  
|*値*|`contacts...<name>.Value`|シーケンス内の最初のオブジェクトの文字列表現を取得します。シーケンスが空の場合は `Nothing` します。|  
  
## <a name="in-this-section"></a>このセクションの内容  
 [方法: XML 子孫要素にアクセスする](../../../../visual-basic/programming-guide/language-features/xml/how-to-access-xml-descendant-elements.md)  
 子孫軸プロパティを使用して、指定した名前を持ち、指定した XML 要素の下に含まれるすべての XML 要素にアクセスする方法を示します。  
  
 [方法: XML 子要素にアクセスする](../../../../visual-basic/programming-guide/language-features/xml/how-to-access-xml-child-elements.md)  
 子軸プロパティを使用して、XML 要素内で指定された名前を持つすべての XML 子要素にアクセスする方法を示します。  
  
 [方法: XML 属性にアクセスする](../../../../visual-basic/programming-guide/language-features/xml/how-to-access-xml-attributes.md)  
 属性軸プロパティを使用して、XML 要素内で指定した名前を持つすべての XML 属性にアクセスする方法を示します。  
  
 [方法 : XML 名前空間プレフィックスを宣言して使用する](../../../../visual-basic/programming-guide/language-features/xml/how-to-declare-and-use-xml-namespace-prefixes.md)  
 XML 名前空間プレフィックスを宣言し、それを使用して XML 要素を作成およびアクセスする方法を示します。  
  
## <a name="related-sections"></a>関連セクション  
 [XML 軸プロパティ](../../../../visual-basic/language-reference/xml-axis/index.md)  
 さまざまな XML アクセスプロパティについて説明するセクションへのリンクを示します。  
  
 [Visual Basic における LINQ to XML の概要](../../../../visual-basic/programming-guide/language-features/xml/overview-of-linq-to-xml.md)  
 Visual Basic での [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の使用方法について説明します。  
  
 [Visual Basic での XML の作成](../../../../visual-basic/programming-guide/language-features/xml/creating-xml.md)  
 Visual Basic での XML リテラルの使用方法について説明します。  
  
 [Visual Basic での XML の操作](../../../../visual-basic/programming-guide/language-features/xml/manipulating-xml.md)  
 Visual Basic での XML の読み込みと変更に関するセクションへのリンクを示します。  
  
 [XML](../../../../visual-basic/programming-guide/language-features/xml/index.md)  
 Visual Basic での [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] の使用方法を説明するセクションへのリンクを示します。
