---
title: ヘッダー情報にアクセスして XML フラグメントをストリーム出力する方法 (C#)
ms.date: 07/20/2015
ms.assetid: 7f242770-b0c7-418d-894b-643215e1f8aa
ms.openlocfilehash: 5bc10bcadae0e33ee63f953608ca841d44dd6527
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712391"
---
# <a name="how-to-stream-xml-fragments-with-access-to-header-information-c"></a>ヘッダー情報にアクセスして XML フラグメントをストリーム出力する方法 (C#)
大きな XML ファイルを任意に読み取り、アプリケーションのメモリ使用量を予想できるようにアプリケーションを作成しなければならない場合があります。 大きな XML ファイルを XML ツリーに設定しようとすると、ファイルのサイズに比例してメモリが過剰に使用されます。 したがって、代わりにストリーミングの手法を使用する必要があります。  
  
これを実現する 1 つの選択肢として、<xref:System.Xml.XmlReader> を使用してアプリケーションを作成する方法があります。 ただし、場合によっては、XML ツリーに対してクエリを実行するとき、LINQ の使用が必要になることがあります。 このような場合は、カスタムの軸メソッドを独自に記述します。 詳細については、「[ LINQ to XML 軸メソッドを記述する方法 (C#)](./how-to-write-a-linq-to-xml-axis-method.md)」を参照してください。
  
 独自の軸メソッドを記述するには、<xref:System.Xml.XmlReader> を使用して、対象となるノードの 1 つに到達するまでノードを読み取る小さなメソッドを記述します。 このメソッドから <xref:System.Xml.Linq.XNode.ReadFrom%2A> が呼び出され、これにより <xref:System.Xml.XmlReader> からデータが読み取られ、XML フラグメントがインスタンス化されます。 さらに、カスタムの軸メソッドを列挙するメソッドに対しては、`yield return` を使用して各フラグメントが生成されます。 これで、カスタムの軸メソッド上に LINQ クエリを記述できます。  
  
 ストリーミングの手法は、ソース ドキュメントを 1 回だけ処理する必要がある場合に適しており、ドキュメントの順序で要素を処理できます。 <xref:System.Linq.Enumerable.OrderBy%2A> などの一部の標準クエリ演算子では、ソースが反復処理され、すべてのデータが収集され並べ替えられて、最終的にはシーケンス内の最初の項目が生成されます。 最初の項目を生成する前にソースを具体化するクエリ演算子を使用すると、メモリ使用量を低く維持することができません。  
  
## <a name="example"></a>例  

ストリーム出力は関心の高い問題となる場合があるため、例を使って説明します。 次の XML ドキュメントでは、カスタムの軸メソッドのコンシューマーが、各項目が属している顧客の名前も認識している必要があります。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<Root>  
  <Customer>  
    <Name>A. Datum Corporation</Name>  
    <Item>  
      <Key>0001</Key>  
    </Item>  
    <Item>  
      <Key>0002</Key>  
    </Item>  
    <Item>  
      <Key>0003</Key>  
    </Item>  
    <Item>  
      <Key>0004</Key>  
    </Item>  
  </Customer>  
  <Customer>  
    <Name>Fabrikam, Inc.</Name>  
    <Item>  
      <Key>0005</Key>  
    </Item>  
    <Item>  
      <Key>0006</Key>  
    </Item>  
    <Item>  
      <Key>0007</Key>  
    </Item>  
    <Item>  
      <Key>0008</Key>  
    </Item>  
  </Customer>  
  <Customer>  
    <Name>Southridge Video</Name>  
    <Item>  
      <Key>0009</Key>  
    </Item>  
    <Item>  
      <Key>0010</Key>  
    </Item>  
  </Customer>  
</Root>  
```  
  
 この例で採用している方法では、ヘッダー情報の監視と保存が行われ、その後でヘッダー情報と列挙される詳細情報の両方が含まれている小さな XML ツリーが構築されます。 次に、軸メソッドによってこの新しい小さな XML ツリーが生成されます。 これでクエリは、詳細情報だけでなくヘッダー情報にもアクセスできるようになります。  
  
 この方法で使用されるメモリは少量です。 詳細な XML フラグメントが個々に生成されるときに、前のフラグメントへの参照は保持されず、そのフラグメントはガベージ コレクションの対象になります。 この手法を使用すると、存続期間の短いオブジェクトがヒープ上に多数作成されます。  
  
 次の例では、URI で指定されたファイルから XML フラグメントをストリーム出力する、カスタムの軸メソッドを実装して使用する方法を示します。 このカスタムの軸は、`Customer`、`Name`、`Item` の各要素を含んだドキュメントを前提として記述されています。また、それらの要素は、上記の `Source.xml` ドキュメントと同じように配置されます。 これは単純な実装です。 ただし、より堅牢に実装する場合は、無効なドキュメントの解析にも対応するようにします。  
  
```csharp  
static IEnumerable<XElement> StreamCustomerItem(string uri)  
{  
    using (XmlReader reader = XmlReader.Create(uri))  
    {  
        XElement name = null;  
        XElement item = null;  
  
        reader.MoveToContent();  
  
        // Parse the file, save header information when encountered, and yield the  
        // Item XElement objects as they are created.  
  
        // loop through Customer elements  
        while (reader.Read())  
        {  
            if (reader.NodeType == XmlNodeType.Element  
                && reader.Name == "Customer")  
            {  
                // move to Name element  
                while (reader.Read())  
                {  
                    if (reader.NodeType == XmlNodeType.Element &&  
                        reader.Name == "Name")  
                    {  
                        name = XElement.ReadFrom(reader) as XElement;  
                        break;  
                    }  
                }  
  
                // loop through Item elements  
                while (reader.Read())  
                {  
                    if (reader.NodeType == XmlNodeType.EndElement)  
                        break;  
                    if (reader.NodeType == XmlNodeType.Element  
                        && reader.Name == "Item")  
                    {  
                        item = XElement.ReadFrom(reader) as XElement;  
                        if (item != null) {  
                            XElement tempRoot = new XElement("Root",  
                                new XElement(name)  
                            );  
                            tempRoot.Add(item);  
                            yield return item;  
                        }  
                    }  
                }  
            }  
        }  
    }  
}  
  
static void Main(string[] args)  
{  
    XElement xmlTree = new XElement("Root",  
        from el in StreamCustomerItem("Source.xml")  
        where (int)el.Element("Key") >= 3 && (int)el.Element("Key") <= 7  
        select new XElement("Item",  
            new XElement("Customer", (string)el.Parent.Element("Name")),  
            new XElement(el.Element("Key"))  
        )  
    );  
    Console.WriteLine(xmlTree);  
}  
```  
  
 このコードを実行すると、次の出力が生成されます。  
  
```xml  
<Root>  
  <Item>  
    <Customer>A. Datum Corporation</Customer>  
    <Key>0003</Key>  
  </Item>  
  <Item>  
    <Customer>A. Datum Corporation</Customer>  
    <Key>0004</Key>  
  </Item>  
  <Item>  
    <Customer>Fabrikam, Inc.</Customer>  
    <Key>0005</Key>  
  </Item>  
  <Item>  
    <Customer>Fabrikam, Inc.</Customer>  
    <Key>0006</Key>  
  </Item>  
  <Item>  
    <Customer>Fabrikam, Inc.</Customer>  
    <Key>0007</Key>  
  </Item>  
</Root>  
```  
