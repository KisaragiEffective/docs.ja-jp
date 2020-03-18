---
title: ドキュメントコメントに推奨される XML タグ
ms.date: 07/20/2015
f1_keywords:
- vb.XmlDocComment
helpviewer_keywords:
- tags, XML
- XML comments, recommended tags [Visual Basic]
- comments, recommended XML tags
ms.assetid: 294e0736-ff1e-498e-af83-6db71ed41a72
ms.openlocfilehash: 093c967557b899c8661fdec348d421996e948b94
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352331"
---
# <a name="recommended-xml-tags-for-documentation-comments-visual-basic"></a>ドキュメント コメントとして推奨される XML タグ (Visual Basic)
Visual Basic コンパイラは、コード内のドキュメントコメントを XML ファイルに処理できます。 XML ファイルをドキュメントに処理するために、追加のツールを使用できます。  
  
 XML コメントは、型や型のメンバーなどのコード構造で使用できます。 部分型の場合、XML コメントを持つことができるのは型の1つの部分のみですが、そのメンバーのコメントには制限はありません。  
  
> [!NOTE]
> ドキュメントコメントを名前空間に適用することはできません。 これは、1つの名前空間が複数のアセンブリにまたがることができ、すべてのアセンブリを同時に読み込む必要がないためです。  
  
 コンパイラは、有効な XML であるすべてのタグを処理します。 次のタグは、ユーザードキュメントで一般的に使用される機能を提供します。  
  
||||  
|---|---|---|  
|[\<c>](../../../visual-basic/language-reference/xmldoc/c.md)|[\<code>](../../../visual-basic/language-reference/xmldoc/code.md)|[\<example>](../../../visual-basic/language-reference/xmldoc/example.md)|  
|[\<exception>](../../../visual-basic/language-reference/xmldoc/exception.md) <sup>1</sup>|[\<include>](../../../visual-basic/language-reference/xmldoc/include.md) <sup>1</sup>|[\<list>](../../../visual-basic/language-reference/xmldoc/list.md)|  
|[\<para>](../../../visual-basic/language-reference/xmldoc/para.md)|[\<param>](../../../visual-basic/language-reference/xmldoc/param.md) <sup>1</sup>|[\<paramref>](../../../visual-basic/language-reference/xmldoc/paramref.md)|  
|[\<permission>](../../../visual-basic/language-reference/xmldoc/permission.md) <sup>1</sup>|[\<remarks>](../../../visual-basic/language-reference/xmldoc/remarks.md)|[\<returns>](../../../visual-basic/language-reference/xmldoc/returns.md)|  
|[\<see>](../../../visual-basic/language-reference/xmldoc/see.md) <sup>1</sup>|[\<seealso>](../../../visual-basic/language-reference/xmldoc/seealso.md) <sup>1</sup>|[\<summary>](../../../visual-basic/language-reference/xmldoc/summary.md)|  
|[\<typeparam>](../../../visual-basic/language-reference/xmldoc/typeparam.md) <sup>1</sup>|[\<value>](../../../visual-basic/language-reference/xmldoc/value.md)||  
  
 (<sup>1</sup>コンパイラは構文を検証します)。  
  
> [!NOTE]
> ドキュメントコメントのテキストに山かっこを表示する場合は、`&lt;` と `&gt;`を使用します。 たとえば、文字列 `"&lt;text in angle brackets&gt;"` は `<text in angle brackets>`として表示されます。  
  
## <a name="see-also"></a>関連項目

- [XML の使用によるコードのドキュメントの作成](../../../visual-basic/programming-guide/program-structure/documenting-your-code-with-xml.md)
- [-doc](../../../visual-basic/reference/command-line-compiler/doc.md)
- [方法: XML ドキュメントを作成する](../../../visual-basic/programming-guide/program-structure/how-to-create-xml-documentation.md)
