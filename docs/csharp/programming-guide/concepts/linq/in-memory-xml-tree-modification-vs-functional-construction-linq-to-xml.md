---
title: メモリ内の XML ツリーの変更と関数型構築の比較 (LINQ to XML) (C#)
ms.date: 07/20/2015
ms.assetid: b5afc31d-a325-4ec6-bf17-0ff90a20ffca
ms.openlocfilehash: 55eb95585bdd5d2c52175cacae2e6d049bd06f69
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "66484558"
---
# <a name="in-memory-xml-tree-modification-vs-functional-construction-linq-to-xml-c"></a>メモリ内の XML ツリーの変更と関数型構築の比較 (LINQ to XML) (C#)
XML ドキュメントの構造を変更する場合は、XML ツリーを直接変更するのが従来の方法です。 一般的なアプリケーションでは、ドキュメントを DOM や [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] などのデータ ストアに読み込み、プログラミング インターフェイスを使用してノードの挿入、削除、または内容変更を行い、その後に XML をファイルに保存するか、またはネットワーク上に送信します。  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、多くのシナリオで役立つもう 1 つの方法として、*関数型構築*を使用できます。 関数型構築では、データの変更が、データ ストアの詳細な操作としてではなく変換の問題として扱われます。 データの表現をある形式から別の形式に効率よく変換できれば、データ ストアを何らかの方法で操作して別の構造にする場合と同じ結果を得られます。 関数型構築の方法で重要なのは、クエリの結果を <xref:System.Xml.Linq.XDocument> コンストラクターと <xref:System.Xml.Linq.XElement> コンストラクターに渡す処理です。  
  
 多くの場合、変換コードは、データ ストアを操作する場合に比べてはるかに短時間で作成でき、堅牢性と保守性にも優れています。 この場合、変換コードによる方法では、より大きな処理能力を必要とする可能性はありますが、効果的にデータを変更できます。 開発者が関数型の方法に精通していれば、ほとんどの場合、作成されるコードもよりわかりやすくなります。 ツリーの各部分を変更するコードが簡単に見つかります。  
  
 DOM プログラマの多くは、XML ツリーを直接変更する方法に慣れています。関数型の方法をまだ理解していない開発者は、この方法で記述されたコードに慣れていない場合があります。 大きな XML ツリーに小さな変更を加えるだけであれば、ツリーを直接変更する方法の方が使用する CPU 時間が少ない場合がほとんどです。  
  
 このトピックでは、両方の方法での実装例を紹介します。  
  
## <a name="transforming-attributes-into-elements"></a>属性から要素への変換  
 この例では、属性が要素となるように、次の単純な XML ドキュメントを変更するケースを想定します。 まず、直接変更する従来の方法を示します。 その後に関数型構築の方法を示します。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<Root Data1="123" Data2="456">  
  <Child1>Content</Child1>  
</Root>  
```  
  
### <a name="modifying-the-xml-tree"></a>XML ツリーの変更  
 属性から要素を作成した後に属性を削除する、次のようなプロシージャ コードを記述します。  
  
```csharp  
XElement root = XElement.Load("Data.xml");  
foreach (XAttribute att in root.Attributes()) {  
    root.Add(new XElement(att.Name, (string)att));  
}  
root.Attributes().Remove();  
Console.WriteLine(root);  
```  
  
 このコードを実行すると、次の出力が生成されます。  
  
```xml  
<Root>  
  <Child1>Content</Child1>  
  <Data1>123</Data1>  
  <Data2>456</Data2>  
</Root>  
```  
  
### <a name="functional-construction-approach"></a>関数型構築の方法  
 上のコードとは対照的に、関数型の方法を構成するコードでは、新しいツリーを作成し、ソース ツリーから要素と属性を選択し、その要素と属性を新しいツリーに追加するときに適宜変換します。 関数型の方法は次のようになります。  
  
```csharp  
XElement root = XElement.Load("Data.xml");  
XElement newTree = new XElement("Root",  
    root.Element("Child1"),  
    from att in root.Attributes()  
    select new XElement(att.Name, (string)att)  
);  
Console.WriteLine(newTree);  
```  
  
 この例では、最初の例と同じ XML が出力されます。 ただし、関数型の方法では、新しい XML の構造が実際に作成されます。 `Root` 要素、ソース ツリーから `Child1` 要素を取り出すコード、およびソース ツリーから取得した属性を新しいツリーの要素に変換するコードが作成されることがわかります。  
  
 ここで示した関数型の例は、最初の例と比べて短くはなく、単純でもありません。 しかし XML ツリーに多数の変更を加える場合、関数型でない方法では、処理がかなり複雑になり、効率的とはいえません。 一方、関数型の方法では、クエリと式を適宜組み込んだ必要な XML を作成するだけで、必要な内容を取り出せます。 関数型の方法で生成されたコードの方が、保守も簡単です。  
  
 この例では、ツリーを操作する方法に比べて関数型の方法のパフォーマンスが低くなる可能性があるので注意してください。 主な問題は、関数型の方法では存続期間の短いオブジェクトが多数作成されることです。 その代わりに、関数型の方法の方がプログラマの生産性が高いという効果があります。  
  
 ここで示したのはごく単純な例ですが、2 つの方法に関する考え方の違いをよく表しています。 大きな XML ドキュメントを変換する場合は、関数型の方法を使用した方が生産性が高くなります。  
  