---
title: グローバル名前空間の使用 (LINQ to XML)
ms.date: 07/20/2015
ms.assetid: 0a8064d5-e02f-4315-ad48-6deaa443a2f0
ms.openlocfilehash: 80510e370e0a9c7ab27cb5177d9b547ead82715c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350996"
---
# <a name="working-with-global-namespaces-visual-basic-linq-to-xml"></a>グローバル名前空間の使用 (Visual Basic) (LINQ to XML)
Visual Basic の XML リテラルの主な機能の1つに、`Imports` ステートメントを使用して XML 名前空間を宣言する機能があります。 この機能を使用することで、プレフィックスを使用する XML 名前空間または既定の XML 名前空間を宣言できます。  
  
 この機能は 2 つの状況で役立ちます。 1 つは、XML リテラルで宣言された名前空間が組み込み式に引き継がれない場合です。 グローバル名前空間を宣言すると、名前空間を伴う組み込み式を使用する必要がある場合の作業が軽減されます。 もう 1 つは、名前空間と XML プロパティを併用するためにグローバル名前空間を宣言する必要がある場合です。  
  
 グローバル名前空間はプロジェクト レベルで宣言できます。 また、モジュール レベルでもグローバル名前空間を宣言できます。その場合、プロジェクト レベルのグローバル名前空間はオーバーライドされます。 最終的に、グローバル名前空間は XML リテラルでオーバーライドできます。  
  
 グローバルに宣言された名前空間に含まれる XML リテラルまたは XML プロパティを使用する場合は、Visual Studio で XML リテラルまたはプロパティにカーソルを合わせることで、それらの展開名が表示されます。 拡張名はツールヒントに表示されます。  
  
 グローバル名前空間に対応する <xref:System.Xml.Linq.XNamespace> オブジェクトを取得するには、`GetXmlNamespace` メソッドを使用します。  
  
## <a name="examples-of-global-namespaces"></a>グローバル名前空間の例  
 次の例では、`Imports` ステートメントを使用して既定のグローバル名前空間を宣言し、XML リテラルを使用してその名前空間内の <xref:System.Xml.Linq.XElement> オブジェクトを初期化します。  
  
```vb  
Imports <xmlns="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = <Root/>  
        Console.WriteLine(root)  
    End Sub  
End Module  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Root xmlns="http://www.adventure-works.com" />  
```  
  
 次の例では、プレフィックスを持つグローバル名前空間を宣言し、XML リテラルを使用して要素を初期化します。  
  
```vb  
Imports <xmlns:aw="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = <aw:Root/>  
        Console.WriteLine(root)  
    End Sub  
End Module  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<aw:Root xmlns:aw="http://www.adventure-works.com" />  
```  
  
## <a name="global-namespaces-and-embedded-expressions"></a>グローバル名前空間と組み込み式  
 XML リテラルで宣言される名前空間は、組み込み式に引き継がれません。 次の例では、既定の名前空間を宣言しています。 さらに、`Child` 要素に組み込み式を使用しています。  
  
```vb  
Dim root As XElement = _  
    <Root xmlns="http://www.adventure-works.com">  
        <%= <Child/> %>  
    </Root>  
Console.WriteLine(root)  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Root xmlns="http://www.adventure-works.com">  
  <Child xmlns="" />  
</Root>  
```  
  
 このように、結果の XML では、`Child` 要素がどの名前空間にも属さないように、既定の名前空間の宣言が含まれています。  
  
 次のように、組み込み式で名前空間を再宣言できます。  
  
```vb  
Dim root As XElement = _  
    <Root xmlns="http://www.adventure-works.com">  
        <%= <Child xmlns="http://www.adventure-works.com"/> %>  
    </Root>  
Console.WriteLine(root)  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Root xmlns="http://www.adventure-works.com">  
  <Child xmlns="http://www.adventure-works.com" />  
</Root>  
```  
  
 ただし、この方法は既定のグローバル名前空間よりも使い方が複雑であり、既定のグローバル名前空間を使用するアプローチの方がより適切です。 既定のグローバル名前空間を使用すると、名前空間を宣言せずに XML リテラルを使用できます。 結果の XML は、グローバルに宣言された既定の名前空間に含まれることになります。  
  
```vb  
Imports <xmlns="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = <Root>  
                                   <%= <Child/> %>  
                               </Root>  
        Console.WriteLine(root)  
    End Sub  
End Module  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<Root xmlns="http://www.adventure-works.com">  
  <Child />  
</Root>  
```  
  
## <a name="using-namespaces-with-xml-properties"></a>名前空間と XML プロパティの併用  
 名前空間に含まれている XML ツリーを操作する場合に XML プロパティを使用するときは、XML プロパティが正しい名前空間に含まれるように、グローバル名前空間を使用する必要があります。 次の例では、名前空間内で XML ツリーを宣言しています。 さらに、`Child` 要素の数を出力しています。  
  
```vb  
Dim root As XElement = _  
    <Root xmlns="http://www.adventure-works.com">  
        <Child>content</Child>  
    </Root>  
Console.WriteLine(root.<Child>.Count())  
```  
  
 この例は `Child` 要素が存在しないことを示しています。 生成される出力は次のとおりです。  
  
```console  
0  
```  
  
 ただし、既定のグローバル名前空間を宣言すると、XML リテラルと XML プロパティの両方が既定のグローバル名前空間に含まれます。  
  
```vb  
Imports <xmlns="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = _  
            <Root>  
                <Child>content</Child>  
            </Root>  
        Console.WriteLine(root.<Child>.Count())  
    End Sub  
End Module  
```  
  
 この例は、`Child` 要素が 1 つ存在することを示しています。 生成される出力は次のとおりです。  
  
```console  
1  
```  
  
 プレフィックスを持つグローバル名前空間を宣言すると、そのプレフィックスを XML リテラルと XML プロパティの両方で使用できます。  
  
```vb  
Imports <xmlns:aw="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = _  
            <aw:Root>  
                <aw:Child>content</aw:Child>  
            </aw:Root>  
        Console.WriteLine(root.<aw:Child>.Count())  
    End Sub  
End Module  
```  
  
## <a name="xnamespace-and-global-namespaces"></a>XNamespace とグローバル名前空間  
 <xref:System.Xml.Linq.XNamespace> オブジェクトを取得するには、`GetXmlNamespace` メソッドを使用します。  
  
```vb  
Imports <xmlns:aw="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = <aw:Root/>  
        Dim xn As XNamespace = GetXmlNamespace(aw)  
        Console.WriteLine(xn)  
    End Sub  
End Module  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```console  
http://www.adventure-works.com  
```  
  
## <a name="see-also"></a>参照

- [名前空間の概要 (LINQ to XML) (Visual Basic)](namespaces-overview-linq-to-xml.md)
