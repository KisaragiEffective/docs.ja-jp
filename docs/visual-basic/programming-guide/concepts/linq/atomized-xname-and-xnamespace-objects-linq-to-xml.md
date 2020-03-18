---
title: アトミック化された XName および XNamespace オブジェクト (LINQ to XML)
ms.date: 07/20/2015
ms.assetid: 21ee7585-7df9-40b4-8c76-a12bb5f29bb3
ms.openlocfilehash: 0ffed5d00364f6614b439480607ed521f52754ec
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345727"
---
# <a name="atomized-xname-and-xnamespace-objects-linq-to-xml-visual-basic"></a>アトミック化された XName および XNamespace オブジェクト (LINQ to XML) (Visual Basic)

<xref:System.Xml.Linq.XName> オブジェクトと <xref:System.Xml.Linq.XNamespace> オブジェクトは "*アトミック化*" されています。つまり、同じ修飾名を含んでいる場合は、同じオブジェクトを参照します。 これによってクエリのパフォーマンスが向上します。これは、2 つのアトミック化された名前の等価性を比べる場合に、基になる中間言語が、2 つの参照が同じオブジェクトを指しているかどうかを判別するだけで済むためです。 基になるコードは、時間のかかる文字列比較を行う必要がありません。

## <a name="atomization-semantics"></a>アトミック化のセマンティクス

アトミック化とは、2 個の <xref:System.Xml.Linq.XName> オブジェクトのローカル名が同じであり、これらのオブジェクトが同じ名前空間にある場合に、同じインスタンスを共有するという意味です。 同様に、2 個の <xref:System.Xml.Linq.XNamespace> オブジェクトに同じ名前空間 URI がある場合も、これらのオブジェクトは同じインスタンスを共有します。

アトミック化されたオブジェクトをクラスに対して有効にするには、クラスのコンストラクターがパブリックではなく、プライベートであることが必要です。 その理由は、コンストラクターがパブリックであれば、アトミック化されていないオブジェクトを作成できるからです。 <xref:System.Xml.Linq.XName> クラスと <xref:System.Xml.Linq.XNamespace> クラスは暗黙的な変換演算子を実装して、文字列を <xref:System.Xml.Linq.XName> または <xref:System.Xml.Linq.XNamespace> に変換します。 この方法でこれらのオブジェクトのインスタンスを取得します。 コンストラクターにはアクセスできないため、コンストラクターを使用してインスタンスを取得することはできません。

<xref:System.Xml.Linq.XName> と <xref:System.Xml.Linq.XNamespace> は、等値演算子と非等値演算子も実装して、比較対象の2つのオブジェクトが同じインスタンスへの参照であるかどうかを判断します。

## <a name="example"></a>例

次のコードは <xref:System.Xml.Linq.XElement> オブジェクトを作成して、同じ名前が同じインスタンスを共有することを示します。

```vb
Dim r1 As New XElement("Root", "data1")
Dim r2 As XElement = XElement.Parse("<Root>data2</Root>")

If DirectCast(r1.Name, Object) = DirectCast(r2.Name, Object) Then
    Console.WriteLine("r1 and r2 have names that refer to the same instance.")
Else
    Console.WriteLine("Different")
End If

Dim n As XName = "Root"

If DirectCast(n, Object) = DirectCast(r1.Name, Object) Then
    Console.WriteLine("The name of r1 and the name in 'n' refer to the same instance.")
Else
    Console.WriteLine("Different")
End If
```

この例を実行すると、次の出力が生成されます。

```console
r1 and r2 have names that refer to the same instance.
The name of r1 and the name in 'n' refer to the same instance.
```

前述のように、アトミック化されたオブジェクトの利点は、パラメーターとして <xref:System.Xml.Linq.XName> がある軸メソッドの 1 つを使用するときに、目的の要素を選択するために、軸メソッドが、2 つの名前が同じインスタンスを参照していることを判別するだけで済むことです。

次の例では、<xref:System.Xml.Linq.XName> を <xref:System.Xml.Linq.XContainer.Descendants%2A> メソッド呼び出しに渡すので、アトミック化パターンによってパフォーマンスが向上します。

```vb
Dim root As New XElement("Root", New XElement("C1", 1), New XElement("Z1", New XElement("C1", 2), New XElement("C1", 1)))

Dim query = From e In root.Descendants("C1") Where CInt(e) = 1e

For Each z As var In query
    Console.WriteLine(z)
Next
```

この例を実行すると、次の出力が生成されます。

```xml
<C1>1</C1>
<C1>1</C1>
```

## <a name="see-also"></a>関連項目

- [パフォーマンス (LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/performance-linq-to-xml.md)
