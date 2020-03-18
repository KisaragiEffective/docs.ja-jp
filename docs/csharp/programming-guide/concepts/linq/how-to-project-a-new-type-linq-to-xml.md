---
title: 新しい型を射影する方法 (LINQ to XML) (C#)
ms.date: 07/20/2015
ms.assetid: 48145cf9-1e0b-4e73-bbfd-28fc04800dc4
ms.openlocfilehash: 5205a0c56651271dea0181ed96518c0e9d7f95f3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79168994"
---
# <a name="how-to-project-a-new-type-linq-to-xml-c"></a>新しい型を射影する方法 (LINQ to XML) (C#)

このセクションにあるその他の例では、<xref:System.Collections.Generic.IEnumerable%601> の <xref:System.Xml.Linq.XElement>、<xref:System.Collections.Generic.IEnumerable%601> の `string`、および <xref:System.Collections.Generic.IEnumerable%601> の `int` として結果を返すクエリを示しています。 これらは一般的な戻り値の型ですが、すべてのシナリオに適切であるとは限りません。 多くの場合、クエリを使用して、別の型の <xref:System.Collections.Generic.IEnumerable%601> を返すようにする必要があります。

## <a name="example"></a>例

この例では、`select` 句でオブジェクトをインスタンス化する方法を示します。 コードでは、まずコンストラクターを使用して新しいクラスを定義し、次に、式が新しいクラスの新しいインスタンスになるように `select` ステートメントを変更します。

この例では、「[サンプル XML ファイル: 一般的な購買発注書 (LINQ to XML)](./sample-xml-file-typical-purchase-order-linq-to-xml-1.md)」の XML ドキュメントを使用します。

```csharp
class NameQty
{
    public string name;
    public int qty;
    public NameQty(string n, int q)
    {
        name = n;
        qty = q;
    }
};

class Program {
    public static void Main()
    {
        XElement po = XElement.Load("PurchaseOrder.xml");
  
        IEnumerable<NameQty> nqList =
            from n in po.Descendants("Item")
            select new NameQty(
                    (string)n.Element("ProductName"),
                    (int)n.Element("Quantity")
                );
  
        foreach (NameQty n in nqList)
            Console.WriteLine(n.name + ":" + n.qty);
    }
}
```

この例では、「<xref:System.Xml.Linq.XContainer.Element%2A>単一の子要素を取得する方法 (LINQ to XML) (C#)[」トピックで説明している ](how-to-retrieve-a-single-child-element-linq-to-xml.md) メソッドを使用しています。 また、キャストを使用して、<xref:System.Xml.Linq.XContainer.Element%2A> メソッドによって返された要素の値を取得します。  

この例を実行すると、次の出力が生成されます。

```output
Lawnmower:1
Baby Monitor:2
```
