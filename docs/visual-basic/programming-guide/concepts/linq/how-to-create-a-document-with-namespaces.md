---
title: '方法: 名前空間を持つドキュメントを作成する (LINQ to XML)'
ms.date: 07/20/2015
ms.assetid: cc5b0d4d-360c-4ada-94fa-2d2916e989be
ms.openlocfilehash: bbd23840b0356cf14d2c7d6cb71591fe6461a8bd
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74332586"
---
# <a name="how-to-create-a-document-with-namespaces-linq-to-xml-visual-basic"></a>方法 : 名前空間を持つドキュメントを作成する (LINQ to XML) (Visual Basic)
このトピックでは、Visual Basic で名前空間を持つドキュメントを作成する方法について説明します。  
  
 Visual Basic で XML リテラルを使用している場合、ユーザーは 1 つのグローバルな既定の XML 名前空間を定義できます。 この名前空間は、XML リテラルと XML プロパティの両方の既定の名前空間です。 既定の XML 名前空間は、プロジェクト レベルまたはファイル レベルで定義できます。 ファイル レベルで定義すると、プロジェクト レベルの既定の名前空間がオーバーライドされます。  
  
 他の名前空間を定義して、それらの名前空間のプレフィックスを指定することもできます。  
  
 既定の名前空間およびプレフィックスを持つ名前空間の両方を定義する場合は、`Imports` キーワードを使用します。  
  
 詳細については、「 [Visual Basic での XML リテラルの概要](../../../../visual-basic/programming-guide/concepts/linq/introduction-to-xml-literals.md)」を参照してください。  
  
 既定の XML 名前空間は要素だけに適用され、属性には適用されないことに注意してください。 属性は、既定では常に名前空間に含まれません。 ただし、名前空間プレフィックスを使用して属性を名前空間に含めることができます。  
  
## <a name="example"></a>例  
 この例では、1 つの名前空間を含むドキュメントを作成します。  
  
```vb  
Imports <xmlns:aw="http://www.adventure-works.com">  
Module Module1  
    Sub Main()  
        Dim root As XElement = _  
            <aw:Root>  
                <aw:Child aw:Att="attvalue"/>  
            </aw:Root>  
        Console.WriteLine(root)  
    End Sub  
End Module  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<aw:Root xmlns:aw="http://www.adventure-works.com">  
  <aw:Child aw:Att="attvalue" />  
</aw:Root>  
```  
  
## <a name="example"></a>例  
 この例では、2 つの名前空間を含むドキュメントを作成します。このうちの 1 つは既定の名前空間です。  
  
```vb  
Imports <xmlns="http://www.adventure-works.com">  
Imports <xmlns:fc="www.fourthcoffee.com">  
  
Module Module1  
  
    Sub Main()  
        Dim root As XElement = _  
            <Root>  
                <Child Att="attvalue"/>  
                <fc:Child2>child2 content</fc:Child2>  
            </Root>  
        Console.WriteLine(root)  
    End Sub  
  
End Module  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Root xmlns:fc="www.fourthcoffee.com" xmlns="http://www.adventure-works.com">  
  <Child Att="attvalue" />  
  <fc:Child2>child2 content</fc:Child2>  
</Root>  
```  
  
## <a name="example"></a>例  
 次の例では、名前空間プレフィックスを持つ名前空間を 2 つ含むドキュメントを作成します。  
  
 XML ツリーをシリアル化するとき、各要素が指定された名前空間に含まれるように、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] によって必要に応じて名前空間宣言が生成されます。  
  
```vb  
Imports <xmlns:aw="http://www.adventure-works.com">  
Imports <xmlns:fc="www.fourthcoffee.com">  
  
Module Module1  
  
    Sub Main()  
        Dim root As XElement = _  
            <aw:Root>  
                <fc:Child>  
                    <aw:DifferentChild>other content</aw:DifferentChild>  
                </fc:Child>  
                <aw:Child2>c2 content</aw:Child2>  
                <fc:Child3>c3 content</fc:Child3>  
            </aw:Root>  
        Console.WriteLine(root)  
    End Sub  
  
End Module  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<aw:Root xmlns:fc="www.fourthcoffee.com" xmlns:aw="http://www.adventure-works.com">  
  <fc:Child>  
    <aw:DifferentChild>other content</aw:DifferentChild>  
  </fc:Child>  
  <aw:Child2>c2 content</aw:Child2>  
  <fc:Child3>c3 content</fc:Child3>  
</aw:Root>  
```  
  
## <a name="see-also"></a>関連項目

- [名前空間の概要 (LINQ to XML) (Visual Basic)](namespaces-overview-linq-to-xml.md)
