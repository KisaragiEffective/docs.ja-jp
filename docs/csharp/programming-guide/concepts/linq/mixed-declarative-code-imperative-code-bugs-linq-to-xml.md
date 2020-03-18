---
title: 宣言型コードと命令型コードの混在のバグ (LINQ to XML) (C#)
ms.date: 07/20/2015
ms.assetid: fada62d0-0680-4e73-945a-2b00d7a507af
ms.openlocfilehash: 76a9bb5abf6ce2700a2a0698ebc109f65e2b7eb1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79168350"
---
# <a name="mixed-declarative-codeimperative-code-bugs-linq-to-xml-c"></a>宣言型コードと命令型コードの混在のバグ (LINQ to XML) (C#)
[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] には、XML ツリーを直接変更できるさまざまなメソッドが含まれています。 たとえば、要素の追加、要素の削除、要素の内容の変更、属性の追加などの操作を行うことができます。 このプログラミング インターフェイスについては、「[XML ツリーの変更 (LINQ to XML) (C#)](./in-memory-xml-tree-modification-vs-functional-construction-linq-to-xml.md)」を参照してください。 いずれかの軸 (<xref:System.Xml.Linq.XContainer.Elements%2A> など) を反復処理する場合に、その過程で XML ツリーを変更すると、見慣れないバグが発生することがあります。  
  
 この問題は、"ハロウィーン問題" と呼ばれることがあります。  
  
## <a name="definition-of-the-problem"></a>問題の定義  
 コレクションを反復処理するコードを LINQ を使用して記述する場合は、宣言型スタイルでコードを記述することになります。 この場合、*どのように*処理するかではなく、*何が*必要かを記述します。 たとえば、1) 最初の要素を取得する、2) この要素を何らかの条件に対してテストする、3) この要素を変更する、4) この要素をリストに戻す、というコードを記述した場合、それは命令型のコードです。 必要な処理を*どのように*行うかをコンピューターに指示しています。  
  
 この 2 つのスタイルのコードが同じ操作に混在していると、問題の原因になります。 次の点を考慮します。  
  
 3 つの項目 (a、b、および c) を含むリンク リストがあるとします。  
  
 `a -> b -> c`  
  
 このリンク リスト内を移動しながら 3 つの新しい項目 (a'、b'、および c') を追加して、 次のようなリンク リストが生成されるようにします。  
  
 `a -> a' -> b -> b' -> c -> c'`  
  
 ここで、リストを反復処理して各項目の後ろに新しいアイテムを追加するコードを記述した場合、 その結果は、コードによって最初に `a` 要素が検出され、その後ろに `a'` が追加されます。 その後、リストの次のノードに移動しますが、それは既に `a'` になっています。 そのため、新しい項目として `a''` がリストに追加されてしまいます。  
  
 現実にこのような問題が発生したら、どうすればよいでしょうか。 まず、元のリンク リストをコピーして、まったく新しいリストを作成する方法が考えられます。 また、純粋な命令型のコードを記述するのであれば、最初の項目を検出し、新しい項目を追加したら、リンク リストの 2 つ先 (追加した要素を越えた先) に移動するという方法もあります。  
  
## <a name="adding-while-iterating"></a>反復処理と追加  
 たとえば、ツリーのすべての要素に対してその複製となる要素を作成するコードを記述するとします。  
  
```csharp  
XElement root = new XElement("Root",  
    new XElement("A", "1"),  
    new XElement("B", "2"),  
    new XElement("C", "3")  
);  
foreach (XElement e in root.Elements())  
    root.Add(new XElement(e.Name, (string)e));  
```  
  
 このコードは、無限ループに入ります。 `foreach` ステートメントは、`Elements()` 軸を反復処理して `doc` 要素に新しい要素を追加しますが、 追加した要素も反復処理されることになります。 ループが繰り返されるたびに新しいオブジェクトが割り当てられるため、最後には使用可能なメモリがすべて消費されてしまいます。  
  
 この問題を修正するには、次のように、標準クエリ演算子の <xref:System.Linq.Enumerable.ToList%2A> を使用してコレクションをメモリに読み込みます。  
  
```csharp  
XElement root = new XElement("Root",  
    new XElement("A", "1"),  
    new XElement("B", "2"),  
    new XElement("C", "3")  
);  
foreach (XElement e in root.Elements().ToList())  
    root.Add(new XElement(e.Name, (string)e));  
Console.WriteLine(root);  
```  
  
 これで、コードが機能するようになります。 結果の XML ツリーは次のようになります。  
  
```xml  
<Root>  
  <A>1</A>  
  <B>2</B>  
  <C>3</C>  
  <A>1</A>  
  <B>2</B>  
  <C>3</C>  
</Root>  
```  
  
## <a name="deleting-while-iterating"></a>反復処理と削除  
 特定のレベルのノードをすべて削除する場合、次のようなコードを記述することが考えられます。  
  
```csharp  
XElement root = new XElement("Root",  
    new XElement("A", "1"),  
    new XElement("B", "2"),  
    new XElement("C", "3")  
);  
foreach (XElement e in root.Elements())  
    e.Remove();  
Console.WriteLine(root);  
```  
  
 しかし、これでは目的の操作は行われません。 最初の要素 A を削除すると、ルートに含まれる XML ツリーから A が削除されるため、反復処理を行っている Elements メソッド内のコードが次の要素を見つけられなくなります。  
  
 上のコードを実行すると、次の出力が生成されます。  
  
```xml  
<Root>  
  <B>2</B>  
  <C>3</C>  
</Root>  
```  
  
 この問題を解決するには、先ほどと同じように、<xref:System.Linq.Enumerable.ToList%2A> を呼び出してコレクションを具体化します。  
  
```csharp  
XElement root = new XElement("Root",  
    new XElement("A", "1"),  
    new XElement("B", "2"),  
    new XElement("C", "3")  
);  
foreach (XElement e in root.Elements().ToList())  
    e.Remove();  
Console.WriteLine(root);  
```  
  
 このコードを実行すると、次の出力が生成されます。  
  
```xml  
<Root />  
```  
  
 他の方法として、親要素で <xref:System.Xml.Linq.XElement.RemoveAll%2A> を呼び出して、反復処理を完全に取り除くこともできます。  
  
```csharp  
XElement root = new XElement("Root",  
    new XElement("A", "1"),  
    new XElement("B", "2"),  
    new XElement("C", "3")  
);  
root.RemoveAll();  
Console.WriteLine(root);  
```  
  
## <a name="why-cant-linq-automatically-handle-this"></a>この問題が LINQ で自動的に処理されない理由  
 まず、レイジー評価を行う代わりに常にすべてをメモリに読み込む方法が考えられます。 しかし、この方法では、パフォーマンスとメモリ使用量のコストが非常に高くなります。 実際、仮に LINQ (LINQ to XML) でこの方法を使用したとしても、現実には使いものにならないでしょう。  
  
 その他に、ある種のトランザクション構文を LINQ に追加して、コンパイラがコードを分析して特定のコレクションを具体化する必要があるかどうかを特定するようにする方法も考えられます。 しかし、副作用のあるコードをすべて特定しようとすると非常に複雑になります。 次のコードがあるとします。  
  
```csharp  
var z =  
    from e in root.Elements()  
    where TestSomeCondition(e)  
    select DoMyProjection(e);  
```  
  
 このような分析コードで副作用のあるコードの有無を特定するには、TestSomeCondition および DoMyProjection の 2 つのメソッドと、これらが呼び出すすべてのメソッドを分析する必要があります。 しかし、その分析コードでは、ただ副作用のあるコードを探すだけでなく、 この状況では、`root` の子要素に対して副作用があるコードのみを選択する必要があります。  
  
 LINQ to XML ではこのような分析は行われません。  
  
 これらの問題は、開発者が回避する必要があります。  
  
## <a name="guidance"></a>ガイダンス  
 第 1 に、宣言型のコードと命令型のコードを混在させないようにします。  
  
 仮に、コレクションのセマンティクスと、XML ツリーを変更するメソッドのセマンティクスを正確に把握していて、このカテゴリの問題を巧妙に回避するコードを記述できたとしても、将来そのコードの保守を行う別の開発者が同じようにこの問題に通じているとは限りません。 宣言型と命令型のコーディング スタイルが混在していると、コードが不安定になります。  
  
 これらの問題を回避するためにコレクションを具体化するコードを記述する場合は、コードを保守するプログラマが問題を把握できるように、必要に応じてコメントを付けるようにしてください。  
  
 第 2 に、パフォーマンスやその他の事情が許すのであれば、宣言型のコードのみを使用するようにします。 既存の XML ツリーは変更せずに、 新しいツリーを生成します。  
  
```csharp  
XElement root = new XElement("Root",  
    new XElement("A", "1"),  
    new XElement("B", "2"),  
    new XElement("C", "3")  
);  
XElement newRoot = new XElement("Root",  
    root.Elements(),  
    root.Elements()  
);  
Console.WriteLine(newRoot);  
```  
