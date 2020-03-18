---
title: '方法: 名前空間プレフィックスを制御する (LINQ to XML)'
ms.date: 07/20/2015
ms.assetid: 2fcf28a5-31b6-409d-84ea-27c22f71fc9f
ms.openlocfilehash: 5ba415452a8671466c3a4c71a88731e5bd3cda60
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348382"
---
# <a name="how-to-control-namespace-prefixes-visual-basic-linq-to-xml"></a>方法 : 名前空間プレフィックスを制御する (Visual Basic) (LINQ to XML)
このトピックでは、名前空間プレフィックスを制御する方法について説明します。  
  
## <a name="example"></a>例  
  
### <a name="description"></a>説明  
 この例では、2 つの名前空間を宣言します。 `http://www.adventure-works.com` 名前空間にプレフィックス `aw`があり、`www.fourthcoffee.com` 名前空間のプレフィックスが `fc`であることを指定します。  
  
### <a name="code"></a>コード  
  
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
  
### <a name="comments"></a>コメント  
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
