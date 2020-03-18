---
title: 名前空間プレフィックスを制御する方法 (C#) (LINQ to XML)
ms.date: 07/20/2015
ms.assetid: 64de5186-b81a-4ddd-8327-8693df59a01b
ms.openlocfilehash: 9f43c0804d8c830fa75f1e1390cb578c5f5d5106
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74141383"
---
# <a name="how-to-control-namespace-prefixes-c-linq-to-xml"></a>名前空間プレフィックスを制御する方法 (C#) (LINQ to XML)
このトピックでは、XML ツリーをシリアル化する場合に名前空間プレフィックスを制御する方法について説明します。  
  
 多くの場合、名前空間プレフィックスを制御する必要はありません。  
  
 ただし、特定の XML プログラミング ツールでは、名前空間プレフィックスに対する特定の制御が必要です。 たとえば、特定の名前空間プレフィックスを参照する組み込みの XPath 式を含む XSLT スタイル シートや XAML ドキュメントを操作する場合は、ドキュメントをそれらの特定のプレフィックスでシリアル化することが重要になります。  
  
 これは、名前空間プレフィックスを制御する最も一般的な理由です。  
  
 名前空間プレフィックスを制御するもう 1 つの一般的な理由として、ユーザーが XML ドキュメントを手動で編集できるようにし、ユーザーにとって入力しやすい名前空間プレフィックスを作成する必要性が挙げられます。 たとえば、XSD ドキュメントを生成するとします。 スキーマの規則では、スキーマ名前空間のプレフィックスとして `xs` または `xsd` のいずれかを使用することが推奨されています。  
  
 名前空間プレフィックスを制御するには、名前空間を宣言する属性を挿入します。 特定のプレフィックスを持つ名前空間を宣言すると、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] はシリアル化の際にその名前空間プレフィックスの使用を試みます。  
  
 プレフィックスを持つ名前空間を宣言する属性を作成するには、属性の名前の名前空間が <xref:System.Xml.Linq.XNamespace.Xmlns%2A> で、属性の名前が名前空間プレフィックスであるような属性を作成します。 属性の値は、名前空間の URI です。  
  
## <a name="example"></a>例  
 この例では、2 つの名前空間を宣言します。 `http://www.adventure-works.com` 名前空間のプレフィックスを `aw` と指定し、`www.fourthcoffee.com` 名前空間のプレフィックスを `fc` と指定します。  
  
```csharp  
XNamespace aw = "http://www.adventure-works.com";  
XNamespace fc = "www.fourthcoffee.com";  
XElement root = new XElement(aw + "Root",  
    new XAttribute(XNamespace.Xmlns + "aw", "http://www.adventure-works.com"),  
    new XAttribute(XNamespace.Xmlns + "fc", "www.fourthcoffee.com"),  
    new XElement(fc + "Child",  
        new XElement(aw + "DifferentChild", "other content")  
    ),  
    new XElement(aw + "Child2", "c2 content"),  
    new XElement(fc + "Child3", "c3 content")  
);  
Console.WriteLine(root);  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```xml  
<aw:Root xmlns:aw="http://www.adventure-works.com" xmlns:fc="www.fourthcoffee.com">  
  <fc:Child>  
    <aw:DifferentChild>other content</aw:DifferentChild>  
  </fc:Child>  
  <aw:Child2>c2 content</aw:Child2>  
  <fc:Child3>c3 content</fc:Child3>  
</aw:Root>  
```  
  
## <a name="see-also"></a>参照

- [名前空間の概要 (LINQ to XML)](namespaces-overview-linq-to-xml.md)
