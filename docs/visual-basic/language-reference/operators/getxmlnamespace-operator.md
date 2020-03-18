---
title: GetXmlNamespace 演算子
ms.date: 07/20/2015
f1_keywords:
- vb.GetXmlNamespace
- GetXmlNamespace
helpviewer_keywords:
- GetXmlNamespace operator [Visual Basic]
- GetXmlNamespace keyword [Visual Basic]
ms.assetid: d0d28cfd-0755-4896-ae0b-4981aa35517c
ms.openlocfilehash: 4d6e738f4e3a47d73e37c395dd74fe19e99d81bd
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353192"
---
# <a name="getxmlnamespace-operator-visual-basic"></a>GetXmlNamespace 演算子 (Visual Basic)
指定した XML 名前空間プレフィックスに対応する <xref:System.Xml.Linq.XNamespace> オブジェクトを取得します。  
  
## <a name="syntax"></a>構文  
  
```vb  
GetXmlNamespace(xmlNamespacePrefix)  
```  
  
## <a name="parts"></a>指定項目  
 `xmlNamespacePrefix`  
 省略可能。 XML 名前空間プレフィックスを識別する文字列です。 指定する場合、この文字列は有効な XML 識別子である必要があります。 詳細については、「[宣言する XML 要素と属性の名前](../../../visual-basic/programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)」を参照してください。 プレフィックスが指定されていない場合は、既定の名前空間が返されます。 既定の名前空間が指定されていない場合は、空の名前空間が返されます。  
  
## <a name="return-value"></a>戻り値  
 XML 名前空間プレフィックスに対応する <xref:System.Xml.Linq.XNamespace> オブジェクト。  
  
## <a name="remarks"></a>コメント  
 `GetXmlNamespace` 演算子は、XML 名前空間プレフィックス `xmlNamespacePrefix`に対応する <xref:System.Xml.Linq.XNamespace> オブジェクトを取得します。  
  
 Xml 名前空間プレフィックスは、xml リテラルおよび XML 軸プロパティで直接使用できます。 ただし、コードで使用するには、`GetXmlNamespace` 演算子を使用して、名前空間プレフィックスを <xref:System.Xml.Linq.XNamespace> オブジェクトに変換する必要があります。 <xref:System.Xml.Linq.XNamespace> オブジェクトに修飾されていない要素名を追加して、完全修飾 <xref:System.Xml.Linq.XName> オブジェクトを取得できます。これには、多くの [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] メソッドが必要です。  
  
## <a name="example"></a>例  
 次の例では、`ns` を XML 名前空間プレフィックスとしてインポートします。 次に、名前空間のプレフィックスを使用して XML リテラルを作成し、修飾名 `ns:phone`を持つ最初の子ノードにアクセスします。 次に、その子ノードを `ShowName` サブルーチンに渡します。これにより、`GetXmlNamespace` 演算子を使用して修飾名が構築されます。 次に、`ShowName` サブルーチンは修飾名を <xref:System.Xml.Linq.XNode.Ancestors%2A> メソッドに渡して、親 `ns:contact` ノードを取得します。  
  
 [!code-vb[VbXMLSamples#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/GetXmlNamespace.vb#38)]  
  
 `TestGetXmlNamespace.RunSample()`を呼び出すと、次のテキストを含むメッセージボックスが表示されます。  
  
 `Name: Patrick Hines`  
  
## <a name="see-also"></a>関連項目

- [Imports ステートメント (XML 名前空間)](../../../visual-basic/language-reference/statements/imports-statement-xml-namespace.md)
- [Visual Basic での XML へのアクセス](../../../visual-basic/programming-guide/language-features/xml/accessing-xml.md)
