---
title: XML ドキュメントでの名前空間の管理
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 682643fc-b848-4e42-8c0d-50deeaeb5f2a
ms.openlocfilehash: 7b219788895ab2f89fa285c2e1b7de62639bfcf9
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78160041"
---
# <a name="managing-namespaces-in-an-xml-document"></a>XML ドキュメントでの名前空間の管理
XML 名前空間は、XML ドキュメントの要素名と属性名をカスタムの定義済み URI に関連付けます。 この関係を作成するには、名前空間 URI のプレフィックスを定義し、そのプレフィックスを使用して XML データ内の要素名と属性名を修飾します。 名前空間は要素名や属性名の競合を防ぎ、同じ名前の要素や属性を個別に処理および評価できるようにします。  
  
<a name="declare"></a>
## <a name="declaring-namespaces"></a>名前空間の宣言  
 要素で名前空間を宣言するには、`xmlns:` 属性を使用します。  
  
 `xmlns:<name>=<"uri">`  
  
 ここで、`<name>` は名前空間プレフィックスで、`<"uri">` は名前空間を識別する URI です。 プレフィックスを宣言したら、そのプレフィックスを使用して XML ドキュメント内の要素と属性を修飾し、名前空間 URI と関連付けることができます。 名前空間プレフィックスはドキュメント全体を通じて使用されるため、短くする必要があります。  
  
 この例では、2 つの `BOOK` 要素を定義しています。 1 つ目の要素はプレフィックス `mybook` で修飾され、2 つ目の要素はプレフィックス `bb` で修飾されています。 プレフィックスはそれぞれ、異なる名前空間 URI に関連付けられています。  
  
```xml  
<mybook:BOOK xmlns:mybook="http://www.contoso.com/books.dtd">  
<bb:BOOK xmlns:bb="urn:blueyonderairlines">  
```  
  
 ある要素が特定の名前空間の一部であることを指定するには、名前空間プレフィックスをその要素に追加します。 たとえば、`Author` 要素が `mybook` 名前空間に属する場合は、`<mybook:Author>` として宣言されます。  
  
<a name="scope"></a>
## <a name="declaration-scope"></a>宣言のスコープ  
 名前空間の有効な範囲は、宣言された位置から、宣言された要素の最後までです。 この例では、`BOOK` 要素で定義されている名前空間は、`BOOK` 要素など、`Publisher` 要素の外側にある要素には適用されません。  
  
```xml  
<Author>Joe Smith</Author>  
<BOOK xmlns:book="http://www.contoso.com">  
    <title>My Wonderful Day</title>  
      <price>$3.95</price>  
</BOOK>  
<Publisher>  
    <Name>MSPress</Name>  
</Publisher>  
```  
  
 名前空間は使用する前に宣言されている必要がありますが、必ずしも XML ドキュメントの先頭に示されている必要はありません。  
  
 1 つの XML ドキュメントで複数の名前空間を使用する場合は、そのうちの 1 つを既定の名前空間として定義することで、ドキュメントの外観を見やすくできます。 既定の名前空間はルート要素で宣言され、ドキュメント内の修飾されていないすべての要素に適用されます。 既定の名前空間は要素にのみ適用されます。属性には適用されません。  
  
 既定の名前空間を使用するには、要素の宣言でプレフィックスとコロンを省略します。  
  
```xml  
<BOOK xmlns="http://www.contoso.com/books.dtd">  
```  
  
## <a name="managing-namespaces"></a>名前空間の管理  
 <xref:System.Xml.XmlNamespaceManager> クラスには、名前空間 URI とそのプレフィックスのコレクションが格納されます。このクラスを使用すると、コレクションで名前空間を検索、追加、および削除できます。 このクラスは、特定のコンテキストで、XML の処理のパフォーマンスを向上させるために必要です。 たとえば、XPath をサポートするには、<xref:System.Xml.Xsl.XsltContext> クラスで <xref:System.Xml.XmlNamespaceManager> を使用します。  
  
 名前空間マネージャーでは名前空間の検証は一切実行されません。このマネージャーでは、プレフィックスと名前空間が既に確認され、[W3C 名前空間](https://www.w3.org/TR/REC-xml-names/)仕様に準拠していることが前提となっています。  
  
> [!NOTE]
> [C#](../../../csharp/programming-guide/concepts/linq/linq-to-xml-overview.md) および [Visual Basic](../../../visual-basic/programming-guide/concepts/linq/linq-to-xml.md) の LINQ to XML では、名前空間を管理するために <xref:System.Xml.XmlNamespaceManager> を使用しません。 LINQ to XML を使用している場合の名前空間の管理については、LINQ のドキュメントにある「[XML 名前空間の使用 (C#)](../../../csharp/programming-guide/concepts/linq/namespaces-overview-linq-to-xml.md)」および [XML 名前空間の使用 (Visual Basic)](../../../visual-basic/programming-guide/concepts/linq/working-with-xml-namespaces.md) に関するページを参照してください。  
  
 <xref:System.Xml.XmlNamespaceManager> クラスを使用して実行できる管理タスクと検索タスクをいくつか次に示します。 使用例を含む詳細については、各メソッドまたはプロパティのリファレンス ページへのリンクをクリックしてください。  
  
|ターゲット|用途|  
|--------|---------|  
|名前空間を追加する|<xref:System.Xml.XmlNamespaceManager.AddNamespace%2A> メソッド|  
|名前空間を削除する|<xref:System.Xml.XmlNamespaceManager.RemoveNamespace%2A> メソッド|  
|既定の名前空間の URI を検索する|<xref:System.Xml.XmlNamespaceManager.DefaultNamespace%2A> プロパティ|  
|名前空間プレフィックスの URI を検索する|<xref:System.Xml.XmlNamespaceManager.LookupNamespace%2A> メソッド|  
|名前空間 URI のプレフィックスを検索する|<xref:System.Xml.XmlNamespaceManager.LookupPrefix%2A> メソッド|  
|現在のノードの名前空間の一覧を取得する|<xref:System.Xml.XmlNamespaceManager.GetNamespacesInScope%2A> メソッド|  
|名前空間のスコープを指定する|<xref:System.Xml.XmlNamespaceManager.PushScope%2A> メソッドおよび <xref:System.Xml.XmlNamespaceManager.PopScope%2A> メソッド|  
|現在のスコープ内にプレフィックスが定義されているかどうかを確認する|<xref:System.Xml.XmlNamespaceManager.HasNamespace%2A> メソッド|  
|プレフィックスおよび URI を検索するときに使用する名前テーブルを取得する|<xref:System.Xml.XmlNamespaceManager.NameTable%2A> プロパティ|  
  
## <a name="see-also"></a>参照

- <xref:System.Xml.XmlNamespaceManager>
- [XML ドキュメントと XML データ](../../../../docs/standard/data/xml/index.md)
