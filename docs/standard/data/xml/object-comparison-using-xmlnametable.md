---
title: XmlNameTable によるオブジェクトの比較
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 8d94e041-d340-4ddf-9a2c-d7319e3f4f86
ms.openlocfilehash: 63278f1aa1fe47377d2dae322a9d12338bbe45dd
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710532"
---
# <a name="object-comparison-using-xmlnametable"></a>XmlNameTable によるオブジェクトの比較
**XmlDocuments** の作成時には、そのドキュメント用の名前テーブルが作成されます。 XML がドキュメントに読み込まれるか、新しい要素または属性が作成されると、その属性名と要素名が **XmlNameTable** に格納されます。 別のドキュメントからの既存の **NameTable** を使用して **XmlDocument** を作成することもできます。 **XmlNameTable** パラメーターをとるコンストラクターを使用して **XmlDocuments** を作成すると、ドキュメントは、その **XmlNameTable** に既に保存されているノード名、名前空間、プレフィックスにアクセスするようになります。 どのような方法で名前テーブルと名前が読み込まれたとしても、いったんテーブルに名前が格納されると、文字列比較によってではなく、オブジェクト比較を利用して高速に名前を比較できます。 <xref:System.Xml.NameTable.Add%2A> を使用して、名前テーブルに文字列を追加することもできます。 名前テーブルを作成し、そのテーブルに **MyString** という文字列を追加するコード サンプルを次に示します。 その後、そのテーブルを使用して **XmlDocument** を作成し、**Myfile.xml** の要素名と属性名を既存の名前テーブルに追加します。  
  
```vb  
Dim nt As New NameTable()  
nt.Add("MyString")  
Dim doc As New XmlDocument(nt)  
doc.Load("Myfile.xml")  
```  
  
```csharp  
NameTable nt = new NameTable();  
nt.Add("MyString");  
XmlDocument doc = new XmlDocument(nt);  
doc.Load("Myfile.xml");  
```  
  
 ドキュメントを作成し、ドキュメントに 2 つの新しい要素を追加し、それらの要素をドキュメントの名前テーブルにも追加して、名前のオブジェクト比較を実行するコード サンプルを次に示します。  
  
```vb  
Dim doc1 As XmlDocument = imp.CreateDocument()  
Dim node1 As XmlElement = doc.CreateElement("node1")  
Dim doc2 As XmlDocument = imp.CreateDocument()  
Dim node2 As XmlElement = doc.CreateElement("node2")  
if (CType(node1.Name, object) = CType(node2.Name, object))  
```  
  
```csharp  
XmlDocument doc1 = imp.CreateDocument();  
node1 = doc1.CreateElement ("node1");  
XmlDocument doc2 = imp.CreateDocument();  
node2 = doc2.CreateElement ("node1");  
if (((object)node1.Name) == ((object)node2.Name))  
{ ...  
```  
  
 上記のように 2 つのドキュメント間で名前テーブルが渡される状況は、同じタイプのドキュメントを繰り返し処理する場合によく発生します。たとえば、XML スキーマ定義言語 (XSD) スキーマまたはドキュメント型定義 (DTD) に準拠し、同じ文字列が繰り返し使われる発注ドキュメントを e コマース サイトで処理する場合などです。 このような場合は、同じ要素名が複数のドキュメントに現れるため、同じ名前テーブルを使用することによってパフォーマンスが向上します。  
  
## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
