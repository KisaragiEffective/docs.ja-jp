---
title: XML シリアル化の例
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- XML serialization, examples
- arrays, serializing
- ICollection interface, serializing
- XmlSerializer class, serializing
- serialization, examples
- DataSet class, serializing
- XML Schema, serializing
ms.assetid: eec46337-9696-435b-a375-dc5effae6992
ms.openlocfilehash: 17ad1b4b5eae38a4f1dc90e154841b1315dea1b2
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349776"
---
# <a name="examples-of-xml-serialization"></a>XML シリアル化の例

XML シリアル化は、単純な形式から複雑な形式に至るまで、さまざまな形で実行できます。 たとえば、「[XML シリアル化の概要](../../../docs/standard/serialization/introducing-xml-serialization.md)」に示すように、パブリック フィールドとパブリック プロパティだけで構成されるクラスをシリアル化できます。 次の例では、XML シリアル化を使用して特定の XML スキーマ (XSD) ドキュメントに準拠する XML ストリームを生成する方法など、各種の高度なシナリオに対応するコード例を示します。

## <a name="serializing-a-dataset"></a>データセットのシリアル化

次のコード例に示すように、パブリック クラスのインスタンスだけでなく、<xref:System.Data.DataSet> のインスタンスもシリアル化できます。

```vb
Private Sub SerializeDataSet(filename As String)
    Dim ser As XmlSerializer = new XmlSerializer(GetType(DataSet))
    ' Creates a DataSet; adds a table, column, and ten rows.
    Dim ds As DataSet = new DataSet("myDataSet")
    Dim t As DataTable = new DataTable("table1")
    Dim c As DataColumn = new DataColumn("thing")
    t.Columns.Add(c)
    ds.Tables.Add(t)
    Dim r As DataRow
    Dim i As Integer
    for i = 0 to 10
        r = t.NewRow()
        r(0) = "Thing " &  i
        t.Rows.Add(r)
    Next
    Dim writer As TextWriter = new StreamWriter(filename)
    ser.Serialize(writer, ds)
    writer.Close()
End Sub
```

```csharp
private void SerializeDataSet(string filename){
    XmlSerializer ser = new XmlSerializer(typeof(DataSet));

    // Creates a DataSet; adds a table, column, and ten rows.
    DataSet ds = new DataSet("myDataSet");
    DataTable t = new DataTable("table1");
    DataColumn c = new DataColumn("thing");
    t.Columns.Add(c);
    ds.Tables.Add(t);
    DataRow r;
    for(int i = 0; i<10;i++){
        r = t.NewRow();
        r[0] = "Thing " + i;
        t.Rows.Add(r);
    }
    TextWriter writer = new StreamWriter(filename);
    ser.Serialize(writer, ds);
    writer.Close();
}
```

## <a name="serializing-an-xmlelement-and-xmlnode"></a>XmlElement および XmlNode のシリアル化

また、次のコード例に示すように、<xref:System.Xml.XmlElement> または <xref:System.Xml.XmlNode> クラスのインスタンスをシリアル化することもできます。

```vb
private Sub SerializeElement(filename As String)
    Dim ser As XmlSerializer = new XmlSerializer(GetType(XmlElement))
    Dim myElement As XmlElement = _
    new XmlDocument().CreateElement("MyElement", "ns")
    myElement.InnerText = "Hello World"
    Dim writer As TextWriter = new StreamWriter(filename)
    ser.Serialize(writer, myElement)
    writer.Close()
End Sub

Private Sub SerializeNode(filename As String)
    Dim ser As XmlSerializer = _
    new XmlSerializer(GetType(XmlNode))
    Dim myNode As XmlNode = new XmlDocument(). _
    CreateNode(XmlNodeType.Element, "MyNode", "ns")
    myNode.InnerText = "Hello Node"
    Dim writer As TextWriter = new StreamWriter(filename)
    ser.Serialize(writer, myNode)
    writer.Close()
End Sub
```

```csharp
private void SerializeElement(string filename){
    XmlSerializer ser = new XmlSerializer(typeof(XmlElement));
    XmlElement myElement=
    new XmlDocument().CreateElement("MyElement", "ns");
    myElement.InnerText = "Hello World";
    TextWriter writer = new StreamWriter(filename);
    ser.Serialize(writer, myElement);
    writer.Close();
}

private void SerializeNode(string filename){
    XmlSerializer ser = new XmlSerializer(typeof(XmlNode));
    XmlNode myNode= new XmlDocument().
    CreateNode(XmlNodeType.Element, "MyNode", "ns");
    myNode.InnerText = "Hello Node";
    TextWriter writer = new StreamWriter(filename);
    ser.Serialize(writer, myNode);
    writer.Close();
}
```

## <a name="serializing-a-class-that-contains-a-field-returning-a-complex-object"></a>複雑なオブジェクトを返すフィールドを含むクラスのシリアル化

プロパティやフィールドが複雑なオブジェクト (配列やクラス インスタンスなど) を返す場合、<xref:System.Xml.Serialization.XmlSerializer> は、そのオブジェクトを、メイン XML ドキュメント内で入れ子になっている要素に変換します。 たとえば、次のコード例の最初のクラスは、2 番目のクラスのインスタンスを返します。

```vb
Public Class PurchaseOrder
    Public MyAddress As Address
End Class

Public Class Address
    Public FirstName As String
End Class
```

```csharp
public class PurchaseOrder
{
    public Address MyAddress;
}
public class Address
{
    public string FirstName;
}
```

シリアル化された XML 出力は次のようになります。

```xml
<PurchaseOrder>
    <MyAddress>
        <FirstName>George</FirstName>
    </MyAddress>
</PurchaseOrder>
```

## <a name="serializing-an-array-of-objects"></a>オブジェクト配列のシリアル化

次のコード例に示すように、オブジェクトの配列を返すフィールドをシリアル化することもできます。

```vb
Public Class PurchaseOrder
    public ItemsOrders () As Item
End Class

Public Class Item
    Public ItemID As String
    Public ItemPrice As decimal
End Class
```

```csharp
public class PurchaseOrder
{
    public Item [] ItemsOrders;
}

public class Item
{
    public string ItemID;
    public decimal ItemPrice;
}
```

シリアル化されたクラスのインスタンスは、2 つの品目が注文された場合、次のようになります。

```xml
<PurchaseOrder xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <ItemsOrders>
        <Item>
            <ItemID>aaa111</ItemID>
            <ItemPrice>34.22</ItemPrice>
        </Item>
        <Item>
            <ItemID>bbb222</ItemID>
            <ItemPrice>2.89</ItemPrice>
        </Item>
    </ItemsOrders>
</PurchaseOrder>
```

## <a name="serializing-a-class-that-implements-the-icollection-interface"></a>ICollection インターフェイスを実装するクラスのシリアル化

<xref:System.Collections.ICollection> インターフェイスを実装して独自のコレクション クラスを作成し、<xref:System.Xml.Serialization.XmlSerializer> を使用して、これらのクラスのインスタンスをシリアル化できます。 クラスが <xref:System.Collections.ICollection> インターフェイスを実装する場合、そのクラスに含まれているコレクションだけがシリアル化されます。 このクラスに追加されたパブリック プロパティやパブリック フィールドはシリアル化されません。 シリアル化するには、クラスに **Add** メソッドと **Item** プロパティ (C# の場合はインデクサー) を含める必要があります。

```vb
Imports System.Collections
Imports System.IO
Imports System.Xml.Serialization

Public Class Test
    Shared Sub Main()
        Dim t As Test= new Test()
        t.SerializeCollection("coll.xml")
    End Sub

    Private Sub SerializeCollection(filename As String)
        Dim Emps As Employees  = new Employees()
        ' Note that only the collection is serialized -- not the
        ' CollectionName or any other public property of the class.
        Emps.CollectionName = "Employees"
        Dim John100 As Employee = new Employee("John", "100xxx")
        Emps.Add(John100)
        Dim x As XmlSerializer = new XmlSerializer(GetType(Employees))
        Dim writer As TextWriter = new StreamWriter(filename)
        x.Serialize(writer, Emps)
        writer.Close()
    End Sub
End Class

Public Class Employees
    Implements ICollection
    Public CollectionName As String
    Private empArray As ArrayList = new ArrayList()

    Public ReadOnly Default Overloads _
    Property Item(index As Integer) As Employee
        get
        return CType (empArray(index), Employee)
        End Get
    End Property

    Public Sub CopyTo(a As Array, index As Integer) _
    Implements ICollection.CopyTo
        empArray.CopyTo(a, index)
    End Sub

    Public ReadOnly Property Count () As integer Implements _
    ICollection.Count
        get
            Count = empArray.Count
        End Get

    End Property

    Public ReadOnly Property SyncRoot ()As Object _
    Implements ICollection.SyncRoot
        get
        return me
        End Get
    End Property

    Public ReadOnly Property IsSynchronized () As Boolean _
    Implements ICollection.IsSynchronized
        get
        return false
        End Get
    End Property

    Public Function GetEnumerator() As IEnumerator _
    Implements IEnumerable.GetEnumerator

        return empArray.GetEnumerator()
    End Function

    Public Function Add(newEmployee As Employee) As Integer
        empArray.Add(newEmployee)
        return empArray.Count
    End Function
End Class

Public Class Employee
    Public EmpName As String
    Public EmpID As String

    Public Sub New ()
    End Sub

    Public Sub New (newName As String , newID As String )
        EmpName = newName
        EmpID = newID
    End Sub
End Class
```

```csharp
using System;
using System.Collections;
using System.IO;
using System.Xml.Serialization;

public class Test {
    static void Main(){
        Test t = new Test();
        t.SerializeCollection("coll.xml");
    }

    private void SerializeCollection(string filename){
        Employees Emps = new Employees();
        // Note that only the collection is serialized -- not the
        // CollectionName or any other public property of the class.
        Emps.CollectionName = "Employees";
        Employee John100 = new Employee("John", "100xxx");
        Emps.Add(John100);
        XmlSerializer x = new XmlSerializer(typeof(Employees));
        TextWriter writer = new StreamWriter(filename);
        x.Serialize(writer, Emps);
    }
}
public class Employees:ICollection {
    public string CollectionName;
    private ArrayList empArray = new ArrayList();

    public Employee this[int index]{
        get{return (Employee) empArray[index];}
    }

    public void CopyTo(Array a, int index){
        empArray.CopyTo(a, index);
    }
    public int Count{
        get{return empArray.Count;}
    }
    public object SyncRoot{
        get{return this;}
    }
    public bool IsSynchronized{
        get{return false;}
    }
    public IEnumerator GetEnumerator(){
        return empArray.GetEnumerator();
    }

    public void Add(Employee newEmployee){
        empArray.Add(newEmployee);
    }
}

public class Employee {
    public string EmpName;
    public string EmpID;
    public Employee(){}
    public Employee(string empName, string empID){
        EmpName = empName;
        EmpID = empID;
    }
}
```

## <a name="purchase-order-example"></a>注文書の例

次のコード例を切り取って、.cs または .vb というファイル名拡張子を付けて名前を変更したテキスト ファイルに貼り付けます。 そのファイルを、C# コンパイラまたは Visual Basic コンパイラを使用してコンパイルします。 その後、実行可能ファイルの名前を使用して、このコードを実行します。

この例では、単純なシナリオとして、<xref:System.Xml.Serialization.XmlSerializer.Serialize%2A> メソッドを使用してオブジェクトのインスタンスを作成し、ファイル ストリームにシリアル化する方法を示しています。 この XML ストリームをファイルに保存した後、<xref:System.Xml.Serialization.XmlSerializer.Deserialize%2A> メソッドを使用して同じファイルを再び読み込み、元のオブジェクトのコピーを再構築します。

この例では、`PurchaseOrder` という名前のクラスをシリアル化し、さらに逆シリアル化します。 また、`Address` という名前のパブリック フィールドを `ShipTo` に設定する必要があるため、`Address` という名前の 2 番目のクラスも含まれています。 同様に、`OrderedItem` オブジェクトの配列を `OrderedItem` フィールドに設定する必要があるため、`OrderedItems` クラスも含まれています。 最後に、`Test` という名前のクラスには、これらのクラスをシリアル化および逆シリアル化するコードが含まれています。

`CreatePO` メソッドは、`PurchaseOrder`、`Address`、`OrderedItem` の各クラス オブジェクトを作成し、それらのパブリック フィールドの値を設定します。 このメソッドは、<xref:System.Xml.Serialization.XmlSerializer> のシリアル化および逆シリアル化に使用される `PurchaseOrder` クラスのインスタンスも生成します。 このコードは、シリアル化されるクラスの型をコンストラクターに渡します。 さらに、XML ストリームを XML ドキュメントに書き込むために使用する `FileStream` も作成します。

`ReadPo` メソッドは、もう少し単純です。 このメソッドは、逆シリアル化する対象となるオブジェクトを作成し、その値を読み取るだけです。 `CreatePo` メソッドと同様に、最初に <xref:System.Xml.Serialization.XmlSerializer>を構築し、逆シリアル化するクラスの型をコンストラクターに渡します。 また、XML ドキュメントを読み取るため、<xref:System.IO.FileStream> も必要です。 オブジェクトを逆シリアル化するには、この <xref:System.Xml.Serialization.XmlSerializer.Deserialize%2A> を引数として <xref:System.IO.FileStream> メソッドを呼び出します。 逆シリアル化されたオブジェクトは、`PurchaseOrder` 型のオブジェクト変数にキャストする必要があります。 その後で、逆シリアル化された `PurchaseOrder` の値を読み取ります。 作成された PO.xml ファイルを読み取って、実際の XML 出力を確認することもできます。

```vb
Imports System.IO
Imports System.Xml
Imports System.Xml.Serialization
Imports Microsoft.VisualBasic

' The XmlRoot attribute allows you to set an alternate name
' (PurchaseOrder) for the XML element and its namespace. By
' default, the XmlSerializer uses the class name. The attribute
' also allows you to set the XML namespace for the element. Lastly,
' the attribute sets the IsNullable property, which specifies whether
' the xsi:null attribute appears if the class instance is set to
' a null reference.
<XmlRoot("PurchaseOrder", _
 Namespace := "http://www.cpandl.com", IsNullable := False)> _
Public Class PurchaseOrder
    Public ShipTo As Address
    Public OrderDate As String
    ' The XmlArrayAttribute changes the XML element name
    ' from the default of "OrderedItems" to "Items".
    <XmlArray("Items")> _
    Public OrderedItems() As OrderedItem
    Public SubTotal As Decimal
    Public ShipCost As Decimal
    Public TotalCost As Decimal
End Class

Public Class Address
    ' The XmlAttribute attribute instructs the XmlSerializer to serialize the
    ' Name field as an XML attribute instead of an XML element (XML element is
    ' the default behavior).
    <XmlAttribute()> _
    Public Name As String
    Public Line1 As String

    ' Setting the IsNullable property to false instructs the
    ' XmlSerializer that the XML attribute will not appear if
    ' the City field is set to a null reference.
    <XmlElement(IsNullable := False)> _
    Public City As String
    Public State As String
    Public Zip As String
End Class

Public Class OrderedItem
    Public ItemName As String
    Public Description As String
    Public UnitPrice As Decimal
    Public Quantity As Integer
    Public LineTotal As Decimal

    ' Calculate is a custom method that calculates the price per item
    ' and stores the value in a field.
    Public Sub Calculate()
    LineTotal = UnitPrice * Quantity
    End Sub
End Class

Public Class Test
        Public Shared Sub Main()
    ' Read and write purchase orders.
    Dim t As New Test()
    t.CreatePO("po.xml")
    t.ReadPO("po.xml")
    End Sub

    Private Sub CreatePO(filename As String)
        ' Creates an instance of the XmlSerializer class;
        ' specifies the type of object to serialize.
        Dim serializer As New XmlSerializer(GetType(PurchaseOrder))
        Dim writer As New StreamWriter(filename)
        Dim po As New PurchaseOrder()

        ' Creates an address to ship and bill to.
        Dim billAddress As New Address()
        billAddress.Name = "Teresa Atkinson"
        billAddress.Line1 = "1 Main St."
        billAddress.City = "AnyTown"
        billAddress.State = "WA"
        billAddress.Zip = "00000"
        ' Set ShipTo and BillTo to the same addressee.
        po.ShipTo = billAddress
        po.OrderDate = System.DateTime.Now.ToLongDateString()

        ' Creates an OrderedItem.
        Dim i1 As New OrderedItem()
        i1.ItemName = "Widget S"
        i1.Description = "Small widget"
        i1.UnitPrice = CDec(5.23)
        i1.Quantity = 3
        i1.Calculate()

        ' Inserts the item into the array.
        Dim items(0) As OrderedItem
        items(0) = i1
        po.OrderedItems = items
        ' Calculates the total cost.
        Dim subTotal As New Decimal()
        Dim oi As OrderedItem
        For Each oi In  items
            subTotal += oi.LineTotal
        Next oi
        po.SubTotal = subTotal
        po.ShipCost = CDec(12.51)
        po.TotalCost = po.SubTotal + po.ShipCost
        ' Serializes the purchase order, and close the TextWriter.
        serializer.Serialize(writer, po)
        writer.Close()
    End Sub

    Protected Sub ReadPO(filename As String)
        ' Creates an instance of the XmlSerializer class;
        ' specifies the type of object to be deserialized.
        Dim serializer As New XmlSerializer(GetType(PurchaseOrder))
        ' If the XML document has been altered with unknown
        ' nodes or attributes, handles them with the
        ' UnknownNode and UnknownAttribute events.
        AddHandler serializer.UnknownNode, AddressOf serializer_UnknownNode
        AddHandler serializer.UnknownAttribute, AddressOf _
        serializer_UnknownAttribute

        ' A FileStream is needed to read the XML document.
        Dim fs As New FileStream(filename, FileMode.Open)
        ' Declare an object variable of the type to be deserialized.
        Dim po As PurchaseOrder
        ' Uses the Deserialize method to restore the object's state
        ' with data from the XML document.
        po = CType(serializer.Deserialize(fs), PurchaseOrder)
        ' Reads the order date.
        Console.WriteLine(("OrderDate: " & po.OrderDate))

        ' Reads the shipping address.
        Dim shipTo As Address = po.ShipTo
        ReadAddress(shipTo, "Ship To:")
        ' Reads the list of ordered items.
        Dim items As OrderedItem() = po.OrderedItems
        Console.WriteLine("Items to be shipped:")
        Dim oi As OrderedItem
        For Each oi In items
            Console.WriteLine((ControlChars.Tab & oi.ItemName & _
            ControlChars.Tab & _
                oi.Description & ControlChars.Tab & oi.UnitPrice & _
                ControlChars.Tab & _
                oi.Quantity & ControlChars.Tab & oi.LineTotal))
        Next oi
        ' Reads the subtotal, shipping cost, and total cost.
        Console.WriteLine((ControlChars.Cr & New String _
        (ControlChars.Tab, 5) & _
        " Subtotal" & ControlChars.Tab & po.SubTotal & ControlChars.Cr & _
        New String(ControlChars.Tab, 5) & " Shipping" & ControlChars.Tab & _
        po.ShipCost & ControlChars.Cr &  New String(ControlChars.Tab, 5) & _
        " Total" & New String(ControlChars.Tab, 2) & po.TotalCost))
    End Sub

    Protected Sub ReadAddress(a As Address, label As String)
        ' Reads the fields of the Address.
        Console.WriteLine(label)
        Console.Write((ControlChars.Tab & a.Name & ControlChars.Cr & _
        ControlChars.Tab & a.Line1 & ControlChars.Cr & ControlChars.Tab & _
        a.City & ControlChars.Tab & a.State & ControlChars.Cr & _
        ControlChars.Tab & a.Zip & ControlChars.Cr))
    End Sub

    Protected Sub serializer_UnknownNode(sender As Object, e As _
    XmlNodeEventArgs)
        Console.WriteLine(("Unknown Node:" & e.Name & _
        ControlChars.Tab & e.Text))
    End Sub

    Protected Sub serializer_UnknownAttribute(sender As Object, _
    e As XmlAttributeEventArgs)
        Dim attr As System.Xml.XmlAttribute = e.Attr
        Console.WriteLine(("Unknown attribute " & attr.Name & "='" & _
        attr.Value & "'"))
    End Sub 'serializer_UnknownAttribute
End Class 'Test
```

```csharp
using System;
using System.IO;
using System.Xml;
using System.Xml.Serialization;

// The XmlRoot attribute allows you to set an alternate name
// (PurchaseOrder) for the XML element and its namespace. By
// default, the XmlSerializer uses the class name. The attribute
// also allows you to set the XML namespace for the element. Lastly,
// the attribute sets the IsNullable property, which specifies whether
// the xsi:null attribute appears if the class instance is set to
// a null reference.
[XmlRoot("PurchaseOrder", Namespace="http://www.cpandl.com",
IsNullable = false)]
public class PurchaseOrder
{
    public Address ShipTo;
    public string OrderDate;
    // The XmlArray attribute changes the XML element name
    // from the default of "OrderedItems" to "Items".
    [XmlArray("Items")]
    public OrderedItem[] OrderedItems;
    public decimal SubTotal;
    public decimal ShipCost;
    public decimal TotalCost;
}

public class Address
{
    // The XmlAttribute attribute instructs the XmlSerializer to serialize the
    // Name field as an XML attribute instead of an XML element (XML element is
    // the default behavior).
    [XmlAttribute]
    public string Name;
    public string Line1;

    // Setting the IsNullable property to false instructs the
    // XmlSerializer that the XML attribute will not appear if
    // the City field is set to a null reference.
    [XmlElement(IsNullable = false)]
    public string City;
    public string State;
    public string Zip;
}

public class OrderedItem
{
    public string ItemName;
    public string Description;
    public decimal UnitPrice;
    public int Quantity;
    public decimal LineTotal;

    // Calculate is a custom method that calculates the price per item
    // and stores the value in a field.
    public void Calculate()
    {
        LineTotal = UnitPrice * Quantity;
    }
}

public class Test
{
    public static void Main()
    {
        // Read and write purchase orders.
        Test t = new Test();
        t.CreatePO("po.xml");
        t.ReadPO("po.xml");
    }

    private void CreatePO(string filename)
    {
        // Creates an instance of the XmlSerializer class;
        // specifies the type of object to serialize.
        XmlSerializer serializer =
        new XmlSerializer(typeof(PurchaseOrder));
        TextWriter writer = new StreamWriter(filename);
        PurchaseOrder po=new PurchaseOrder();

        // Creates an address to ship and bill to.
        Address billAddress = new Address();
        billAddress.Name = "Teresa Atkinson";
        billAddress.Line1 = "1 Main St.";
        billAddress.City = "AnyTown";
        billAddress.State = "WA";
        billAddress.Zip = "00000";
        // Sets ShipTo and BillTo to the same addressee.
        po.ShipTo = billAddress;
        po.OrderDate = System.DateTime.Now.ToLongDateString();

        // Creates an OrderedItem.
        OrderedItem i1 = new OrderedItem();
        i1.ItemName = "Widget S";
        i1.Description = "Small widget";
        i1.UnitPrice = (decimal) 5.23;
        i1.Quantity = 3;
        i1.Calculate();

        // Inserts the item into the array.
        OrderedItem [] items = {i1};
        po.OrderedItems = items;
        // Calculate the total cost.
        decimal subTotal = new decimal();
        foreach(OrderedItem oi in items)
        {
            subTotal += oi.LineTotal;
        }
        po.SubTotal = subTotal;
        po.ShipCost = (decimal) 12.51;
        po.TotalCost = po.SubTotal + po.ShipCost;
        // Serializes the purchase order, and closes the TextWriter.
        serializer.Serialize(writer, po);
        writer.Close();
    }

    protected void ReadPO(string filename)
    {
        // Creates an instance of the XmlSerializer class;
        // specifies the type of object to be deserialized.
        XmlSerializer serializer = new XmlSerializer(typeof(PurchaseOrder));
        // If the XML document has been altered with unknown
        // nodes or attributes, handles them with the
        // UnknownNode and UnknownAttribute events.
        serializer.UnknownNode+= new
        XmlNodeEventHandler(serializer_UnknownNode);
        serializer.UnknownAttribute+= new
        XmlAttributeEventHandler(serializer_UnknownAttribute);

        // A FileStream is needed to read the XML document.
        FileStream fs = new FileStream(filename, FileMode.Open);
        // Declares an object variable of the type to be deserialized.
        PurchaseOrder po;
        // Uses the Deserialize method to restore the object's state
        // with data from the XML document. */
        po = (PurchaseOrder) serializer.Deserialize(fs);
        // Reads the order date.
        Console.WriteLine ("OrderDate: " + po.OrderDate);

        // Reads the shipping address.
        Address shipTo = po.ShipTo;
        ReadAddress(shipTo, "Ship To:");
        // Reads the list of ordered items.
        OrderedItem [] items = po.OrderedItems;
        Console.WriteLine("Items to be shipped:");
        foreach(OrderedItem oi in items)
        {
            Console.WriteLine("\t"+
            oi.ItemName + "\t" +
            oi.Description + "\t" +
            oi.UnitPrice + "\t" +
            oi.Quantity + "\t" +
            oi.LineTotal);
        }
        // Reads the subtotal, shipping cost, and total cost.
        Console.WriteLine(
        "\n\t\t\t\t\t Subtotal\t" + po.SubTotal +
        "\n\t\t\t\t\t Shipping\t" + po.ShipCost +
        "\n\t\t\t\t\t Total\t\t" + po.TotalCost
        );
    }

    protected void ReadAddress(Address a, string label)
    {
        // Reads the fields of the Address.
        Console.WriteLine(label);
        Console.Write("\t"+
        a.Name +"\n\t" +
        a.Line1 +"\n\t" +
        a.City +"\t" +
        a.State +"\n\t" +
        a.Zip +"\n");
    }

    protected void serializer_UnknownNode
    (object sender, XmlNodeEventArgs e)
    {
        Console.WriteLine("Unknown Node:" +   e.Name + "\t" + e.Text);
    }

    protected void serializer_UnknownAttribute
    (object sender, XmlAttributeEventArgs e)
    {
        System.Xml.XmlAttribute attr = e.Attr;
        Console.WriteLine("Unknown attribute " +
        attr.Name + "='" + attr.Value + "'");
    }
}
```

XML 出力は、次のようになります。

```xml
<?xml version="1.0" encoding="utf-8"?>
<PurchaseOrder xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://www.cpandl.com">
    <ShipTo Name="Teresa Atkinson">
        <Line1>1 Main St.</Line1>
        <City>AnyTown</City>
        <State>WA</State>
        <Zip>00000</Zip>
    </ShipTo>
    <OrderDate>Wednesday, June 27, 2001</OrderDate>
    <Items>
        <OrderedItem>
            <ItemName>Widget S</ItemName>
            <Description>Small widget</Description>
            <UnitPrice>5.23</UnitPrice>
            <Quantity>3</Quantity>
            <LineTotal>15.69</LineTotal>
        </OrderedItem>
    </Items>
    <SubTotal>15.69</SubTotal>
    <ShipCost>12.51</ShipCost>
    <TotalCost>28.2</TotalCost>
</PurchaseOrder>
```

## <a name="see-also"></a>参照

- [XML シリアル化の概要](introducing-xml-serialization.md)
- [属性を使用した XML シリアル化の制御](controlling-xml-serialization-using-attributes.md)
- [XML シリアル化を制御する属性](attributes-that-control-xml-serialization.md)
- [XmlSerializer クラス](xref:System.Xml.Serialization.XmlSerializer)
- [方法 : オブジェクトをシリアル化する](how-to-serialize-an-object.md)
- [方法 : オブジェクトを逆シリアル化する](how-to-deserialize-an-object.md)
