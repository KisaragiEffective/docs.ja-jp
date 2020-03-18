---
title: '方法 : 名前空間内の XML に対するクエリを記述する'
ms.date: 07/20/2015
ms.assetid: 7d4131b5-3288-414f-b77c-b2edc2a1f465
ms.openlocfilehash: 496cf8daf5136e8aafff000312bbd730a5152e9f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344472"
---
# <a name="how-to-write-queries-on-xml-in-namespaces-visual-basic"></a>方法: 名前空間内の XML に対するクエリを記述する (Visual Basic)
名前空間内の XML に対するクエリを記述するには、正しい名前空間を持つ <xref:System.Xml.Linq.XName> オブジェクトを使用する必要があります。  
  
 Visual Basic での最も一般的な方法は、グローバル名前空間を定義し、その名前空間を使用する XML リテラルおよび XML プロパティを使用することです。 既定のグローバル名前空間を定義できます。その場合、XML リテラルの要素は既定でこの名前空間に含まれることになります。 または、プレフィックスを持つグローバル名前空間を定義し、そのプレフィックスを必要に応じて XML リテラルや XML プロパティで使用できます。 XML のその他の形式と同様に、属性は既定でどの名前空間にも含まれません。  
  
 このトピックの最初に示す一連の例では、既定の名前空間内に XML ツリーを作成する方法を示します。 2 つ目の例では、プレフィックスを持つ名前空間で XML ツリーを作成する方法を示します。  
  
## <a name="example"></a>例  
 次の例では、既定の名前空間に含まれる XML ツリーを作成しています。 さらに、要素のコレクションを取得しています。  
  
```vb  
Imports <xmlns="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = _  
            <Root>  
                <Child>1</Child>  
                <Child>2</Child>  
                <Child>3</Child>  
                <AnotherChild>4</AnotherChild>  
                <AnotherChild>5</AnotherChild>  
                <AnotherChild>6</AnotherChild>  
            </Root>  
        Dim c1 As IEnumerable(Of XElement) = _  
            From el In root.<Child> _  
            Select el  
        For Each el As XElement In c1  
            Console.WriteLine(el.Value)  
        Next  
    End Sub  
End Module  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```console  
1  
2  
3  
```  
  
## <a name="example"></a>例  
 一方、Visual Basic では、プレフィックスを持つ名前空間を使用する XML ツリーでクエリを記述する場合と、既定の名前空間内の XML ツリーに対してクエリを実行する場合とで、大きく異なります。 通常は、`Imports` ステートメントを使用してプレフィックスを持つ名前空間をインポートします。 この場合、XML ツリーを作成するときに要素および属性の名前でプレフィックスを使用します。 また、XML プロパティを使用して XML ツリーに対してクエリを実行するときにもプレフィックスを使用します。  
  
 次の例では、プレフィックスを持つ名前空間に含まれる XML ツリーを作成しています。 さらに、要素のコレクションを取得しています。  
  
```vb  
Imports <xmlns:aw="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = _  
            <aw:Root>  
                <aw:Child>1</aw:Child>  
                <aw:Child>2</aw:Child>  
                <aw:Child>3</aw:Child>  
                <aw:AnotherChild>4</aw:AnotherChild>  
                <aw:AnotherChild>5</aw:AnotherChild>  
                <aw:AnotherChild>6</aw:AnotherChild>  
            </aw:Root>  
        Dim c1 As IEnumerable(Of XElement) = _  
            From el In root.<aw:Child> _  
            Select el  
        For Each el As XElement In c1  
            Console.WriteLine(CInt(el))  
        Next  
    End Sub  
End Module  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```console  
1  
2  
3  
```  
  
## <a name="see-also"></a>関連項目

- [名前空間の概要 (LINQ to XML) (Visual Basic)](namespaces-overview-linq-to-xml.md)
