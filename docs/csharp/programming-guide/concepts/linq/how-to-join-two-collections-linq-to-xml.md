---
title: 2 つのコレクションを結合する方法 (LINQ to XML) (C#)
ms.date: 07/20/2015
ms.assetid: 7b817ede-911a-4cff-9dd3-639c3fc228c9
ms.openlocfilehash: a5044778bbfd9529faf5fe63c72076f6a973c815
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75345857"
---
# <a name="how-to-join-two-collections-linq-to-xml-c"></a>2 つのコレクションを結合する方法 (LINQ to XML) (C#)

XML ドキュメント内の要素または属性は、別の要素または属性を参照することがあります。 たとえば、「[サンプル XML ファイル: 顧客と注文 (LINQ to XML)](./sample-xml-file-customers-and-orders-linq-to-xml-2.md)」の XML ドキュメントには、顧客の一覧と注文の一覧が含まれています。 各 `Customer` 要素には、`CustomerID` 属性が含まれています。 各 `Order` 要素には、`CustomerID` 要素が含まれています。 各注文の `CustomerID` 要素は、顧客の `CustomerID` 属性を参照します。

「[サンプル XSD ファイル: 顧客と注文](./sample-xsd-file-customers-and-orders1.md)」のトピックには、このドキュメントの検証に使用できる XSD が含まれています。 この XSD は、XSD の `xs:key` 機能と `xs:keyref` 機能を使用して、`CustomerID` 要素の `Customer` 属性がキーであることを確立し、各 `CustomerID` 要素の `Order` 要素と各 `CustomerID` 要素の `Customer` 属性の間にリレーションシップを確立します。

[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、`join` 句を使用することで、このリレーションシップを利用できます。

使用できるインデックスがないため、このような結合では実行時のパフォーマンスが低下します。

`join` の詳細については、「[結合演算](./join-operations.md)」を参照してください。

## <a name="example"></a>例

次の例では、`Customer` 要素を `Order` 要素に結合し、注文に `CompanyName` 要素が含まれている新しい XML ドキュメントを生成します。

クエリを実行する前に、この例では、ドキュメントが「[サンプル XSD ファイル: 顧客と注文](./sample-xsd-file-customers-and-orders1.md)」のスキーマに準拠しているかどうかを検証します。 これにより、結合句が常に機能するようになります。

このクエリでは、まずすべての `Customer` 要素を取得し、次にそれらを `Order` 要素に結合します。 クエリでは、"K" より大きい `CustomerID` を持つ顧客の注文のみを選択します。 次に、各注文内に顧客情報を含む新しい `Order` 要素を射影します。

この例では、「[サンプル XML ファイル: 顧客と注文 (LINQ to XML)](./sample-xml-file-customers-and-orders-linq-to-xml-2.md)」の XML ドキュメントを使用します。

この例では、「[サンプル XSD ファイル: 顧客と注文](./sample-xsd-file-customers-and-orders1.md)」の XSD スキーマを使用します。

この方式の結合ではパフォーマンスが低下します。 結合は線形探索によって実行されます。 パフォーマンスを向上させるハッシュ テーブルまたはインデックスはありません。

```csharp
XmlSchemaSet schemas = new XmlSchemaSet();
schemas.Add("", "CustomersOrders.xsd");

Console.Write("Attempting to validate, ");
XDocument custOrdDoc = XDocument.Load("CustomersOrders.xml");

bool errors = false;
custOrdDoc.Validate(schemas, (o, e) =>
                     {
                         Console.WriteLine("{0}", e.Message);
                         errors = true;
                     });
Console.WriteLine("custOrdDoc {0}", errors ? "did not validate" : "validated");

if (!errors)
{
    // Join customers and orders, and create a new XML document with
    // a different shape.

    // The new document contains orders only for customers with a
    // CustomerID > 'K'
    XElement custOrd = custOrdDoc.Element("Root");
    XElement newCustOrd = new XElement("Root",
        from c in custOrd.Element("Customers").Elements("Customer")
        join o in custOrd.Element("Orders").Elements("Order")
                   on (string)c.Attribute("CustomerID") equals
                      (string)o.Element("CustomerID")
        where ((string)c.Attribute("CustomerID")).CompareTo("K") > 0
        select new XElement("Order",
            new XElement("CustomerID", (string)c.Attribute("CustomerID")),
            new XElement("CompanyName", (string)c.Element("CompanyName")),
            new XElement("ContactName", (string)c.Element("ContactName")),
            new XElement("EmployeeID", (string)o.Element("EmployeeID")),
            new XElement("OrderDate", (DateTime)o.Element("OrderDate"))
        )
    );
    Console.WriteLine(newCustOrd);
}
```

このコードを実行すると、次の出力が生成されます。

```output
Attempting to validate, custOrdDoc validated
<Root>
  <Order>
    <CustomerID>LAZYK</CustomerID>
    <CompanyName>Lazy K Kountry Store</CompanyName>
    <ContactName>John Steel</ContactName>
    <EmployeeID>1</EmployeeID>
    <OrderDate>1997-03-21T00:00:00</OrderDate>
  </Order>
  <Order>
    <CustomerID>LAZYK</CustomerID>
    <CompanyName>Lazy K Kountry Store</CompanyName>
    <ContactName>John Steel</ContactName>
    <EmployeeID>8</EmployeeID>
    <OrderDate>1997-05-22T00:00:00</OrderDate>
  </Order>
  <Order>
    <CustomerID>LETSS</CustomerID>
    <CompanyName>Let's Stop N Shop</CompanyName>
    <ContactName>Jaime Yorres</ContactName>
    <EmployeeID>1</EmployeeID>
    <OrderDate>1997-06-25T00:00:00</OrderDate>
  </Order>
  <Order>
    <CustomerID>LETSS</CustomerID>
    <CompanyName>Let's Stop N Shop</CompanyName>
    <ContactName>Jaime Yorres</ContactName>
    <EmployeeID>8</EmployeeID>
    <OrderDate>1997-10-27T00:00:00</OrderDate>
  </Order>
  <Order>
    <CustomerID>LETSS</CustomerID>
    <CompanyName>Let's Stop N Shop</CompanyName>
    <ContactName>Jaime Yorres</ContactName>
    <EmployeeID>6</EmployeeID>
    <OrderDate>1997-11-10T00:00:00</OrderDate>
  </Order>
  <Order>
    <CustomerID>LETSS</CustomerID>
    <CompanyName>Let's Stop N Shop</CompanyName>
    <ContactName>Jaime Yorres</ContactName>
    <EmployeeID>4</EmployeeID>
    <OrderDate>1998-02-12T00:00:00</OrderDate>
  </Order>
</Root>
```
