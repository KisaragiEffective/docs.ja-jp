---
title: 宣言する XML 要素と属性の名前
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [XML in Visual Basic]
- element names [XML in Visual Basic]
- names in XML literals [Visual Basic]
- attribute names [XML in Visual Basic]
- XML literals [Visual Basic], element names
ms.assetid: cc110118-b6cf-4ff9-a4e4-6233c90c9fbf
ms.openlocfilehash: 12fbd1f4332391b1acdcf12e101d82627ebbeaff
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74335996"
---
# <a name="names-of-declared-xml-elements-and-attributes-visual-basic"></a>宣言する XML 要素と属性の名前 (Visual Basic)
このトピックでは、xml リテラルの XML 要素と属性に名前を付けるための Visual Basic ガイドラインについて説明します。  XML リテラルでは、ローカル名または修飾名を指定できます。 修飾名は、XML 名前空間プレフィックス、コロン、およびローカル名で構成されます。 XML 名前空間プレフィックスの詳細については、「[XML 要素リテラル](../../../../visual-basic/language-reference/xml-literals/xml-element-literal.md)」を参照してください。  
  
## <a name="rules"></a>ルール  
 Visual Basic 内の要素または属性のローカル名は、次の規則に従う必要があります。  
  
- 名前空間で開始できます。 先頭は英文字またはアンダースコア (`_`) である必要があります。  
  
- アルファベット文字、10進数字、アンダースコア、ピリオド (.)、ハイフン (-) のみを含める必要があります。  
  
- 長さは1024文字以下でなければなりません。  
  
- 名前に表示されるコロンは、名前空間の境界を示します。 したがって、コロンは、特定の名前の XML 名前空間を指定する場合にのみ使用できます。  
  
 さらに、次のガイドラインに従う必要があります。  
  
- XML 1.0 仕様では、文字列 "xml" で始まるすべての名前が、大文字と小文字の違いによって予約されています。 したがって、要素名と属性名にはこれらの名前を使用しないでください。  
  
### <a name="name-length-guidelines"></a>名前の長さのガイドライン  
 実際には、要素の性質を明確に識別しながら、可能な限り短い名前を使用する必要があります。 これにより、コードの読みやすさが向上し、行の長さとソースファイルのサイズが減少します。  
  
 ただし、要素が正しく記述されていない場合や、コードで要素が使用されている場合は、名前を短くすることはできません。 これは、コードを読みやすくするために重要です。 他のユーザーがそれを理解しようとしている場合、または記述した後に長い時間を見ている場合は、適切な要素名を使用すると時間を節約できます。  
  
## <a name="case-sensitivity-in-names"></a>名前の大文字と小文字の区別  
 XML 要素名では大文字と小文字が区別されます。 つまり、Visual Basic コンパイラでは、アルファベット順の場合にのみ異なる2つの名前を比較すると、異なる名前として解釈されます。 たとえば、`ABC` を解釈し、個別の要素を参照するように `abc` します。  
  
## <a name="xml-namespaces"></a>XML 名前空間  
 XML 要素リテラルを作成するときに、要素名の XML 名前空間プレフィックスを指定できます。 詳細については、「 [XML 要素リテラル](../../../../visual-basic/language-reference/xml-literals/xml-element-literal.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [Visual Basic での XML の作成](../../../../visual-basic/programming-guide/language-features/xml/creating-xml.md)
- [XML 要素リテラル](../../../../visual-basic/language-reference/xml-literals/xml-element-literal.md)
