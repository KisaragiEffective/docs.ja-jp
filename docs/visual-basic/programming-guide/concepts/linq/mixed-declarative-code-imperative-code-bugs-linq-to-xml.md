---
title: 宣言型コードと命令型コードの混在のバグ (LINQ to XML)
ms.date: 07/20/2015
ms.assetid: f12b1ab4-bb92-4b92-a648-0525e45b3ce7
ms.openlocfilehash: 369fae59516df785ac686645d47e74e69a8f1457
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74331646"
---
# <a name="mixed-declarative-codeimperative-code-bugs-linq-to-xml-visual-basic"></a>宣言型コードと命令型コードの混在のバグ (LINQ to XML) (Visual Basic)
[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] には、XML ツリーを直接変更できるさまざまなメソッドが含まれています。 たとえば、要素の追加、要素の削除、要素の内容の変更、属性の追加などの操作を行うことができます。 このプログラミングインターフェイスについては、「 [XML ツリーの変更 (LINQ to XML)」 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/modifying-xml-trees-linq-to-xml.md)を参照してください。 いずれかの軸 (<xref:System.Xml.Linq.XContainer.Elements%2A> など) を反復処理する場合に、その過程で XML ツリーを変更すると、見慣れないバグが発生することがあります。  
  
 この問題は、"ハロウィーン問題" と呼ばれることがあります。  
  
## <a name="definition-of-the-problem"></a>問題の定義  
 コレクションを反復処理するコードを LINQ を使用して記述する場合は、宣言型スタイルでコードを記述することになります。 この場合、*どのように*処理するかではなく、*何が*必要かを記述します。 たとえば、1) 最初の要素を取得する、2) この要素を何らかの条件に対してテストする、3) この要素を変更する、4) この要素をリストに戻す、というコードを記述した場合、それは命令型のコードです。 必要な処理を*どのように*行うかをコンピューターに指示しています。  
  
 この 2 つのスタイルのコードが同じ操作に混在していると、問題の原因になります。 次のような項目を考慮します。  
  
 3 つの項目 (a、b、および c) を含むリンク リストがあるとします。  
  
 `a -> b -> c`  
  
 このリンク リスト内を移動しながら 3 つの新しい項目 (a'、b'、および c') を追加して、 次のようなリンク リストが生成されるようにします。  
  
 `a -> a' -> b -> b' -> c -> c'`  
  
 ここで、リストを反復処理して各項目の後ろに新しいアイテムを追加するコードを記述した場合、 その結果は、コードによって最初に `a` 要素が検出され、その後ろに `a'` が追加されます。 その後、リストの次のノードに移動しますが、それは既に `a'` になっています。 そのため、新しい項目として `a''` がリストに追加されてしまいます。  
  
 現実にこのような問題が発生したら、どうすればよいでしょうか。 まず、元のリンク リストをコピーして、まったく新しいリストを作成する方法が考えられます。 また、純粋な命令型のコードを記述するのであれば、最初の項目を検出し、新しい項目を追加したら、リンク リストの 2 つ先 (追加した要素を越えた先) に移動するという方法もあります。  
  
## <a name="adding-while-iterating"></a>反復処理と追加  
 たとえば、ツリーのすべての要素に対してその複製となる要素を作成するコードを記述するとします。  
  
```vb  
Dim root As XElement = _  
    <Root>  
        <A>1</A>  
        <B>2</B>  
        <C>3</C>  
    </Root>  
For Each e As XElement In root.Elements()  
    root.Add(New XElement(e.Name, e.Value))  
Next  
```  
  
 このコードは、無限ループに入ります。 `foreach` ステートメントは、`Elements()` 軸を反復処理して `doc` 要素に新しい要素を追加しますが、 追加した要素も反復処理されることになります。 ループが繰り返されるたびに新しいオブジェクトが割り当てられるため、最後には使用可能なメモリがすべて消費されてしまいます。  
  
 この問題を修正するには、次のように、標準クエリ演算子の <xref:System.Linq.Enumerable.ToList%2A> を使用してコレクションをメモリに読み込みます。  
  
```vb  
Dim root As XElement = _  
    <Root>  
        <A>1</A>  
        <B>2</B>  
        <C>3</C>  
    </Root>  
For Each e As XElement In root.Elements().ToList()  
    root.Add(New XElement(e.Name, e.Value))  
Next  
Console.WriteLine(root)  
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
  
```vb  
Dim root As XElement = _  
    <Root>  
        <A>1</A>  
        <B>2</B>  
        <C>3</C>  
    </Root>  
For Each e As XElement In root.Elements()  
    e.Remove()  
Next  
Console.WriteLine(root)  
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
  
```vb  
Dim root As XElement = _  
    <Root>  
        <A>1</A>  
        <B>2</B>  
        <C>3</C>  
    </Root>  
For Each e As XElement In root.Elements().ToList()  
    e.Remove()  
Next  
Console.WriteLine(root)  
```  
  
 このコードを実行すると、次の出力が生成されます。  
  
```xml  
<Root />  
```  
  
 他の方法として、親要素で <xref:System.Xml.Linq.XElement.RemoveAll%2A> を呼び出して、反復処理を完全に取り除くこともできます。  
  
```vb  
Dim root As XElement = _  
    <Root>  
        <A>1</A>  
        <B>2</B>  
        <C>3</C>  
    </Root>  
root.RemoveAll()  
Console.WriteLine(root)  
```  
  
## <a name="why-cant-linq-automatically-handle-this"></a>この問題が LINQ で自動的に処理されない理由  
 まず、レイジー評価を行う代わりに常にすべてをメモリに読み込む方法が考えられます。 しかし、この方法では、パフォーマンスとメモリ使用量のコストが非常に高くなります。 実際、仮に LINQ (LINQ to XML) でこの方法を使用したとしても、現実には使いものにならないでしょう。  
  
 その他に、ある種のトランザクション構文を LINQ に追加して、コンパイラがコードを分析して特定のコレクションを具体化する必要があるかどうかを特定するようにする方法も考えられます。 しかし、副作用のあるコードをすべて特定しようとすると非常に複雑になります。 次のコードがあるとします。  
  
```vb  
Dim z = _  
    From e In root.Elements() _  
    Where (TestSomeCondition(e)) _  
    Select DoMyProjection(e)  
```  
  
 このような分析コードで副作用のあるコードの有無を特定するには、TestSomeCondition および DoMyProjection の 2 つのメソッドと、これらが呼び出すすべてのメソッドを分析する必要があります。 しかし、その分析コードでは、ただ副作用のあるコードを探すだけでなく、 この状況では、`root` の子要素に対して副作用があるコードのみを選択する必要があります。  
  
 LINQ to XML ではこのような分析は行われません。  
  
 これらの問題は、開発者が回避する必要があります。  
  
## <a name="guidance"></a>ガイダンス  
 第 1 に、宣言型のコードと命令型のコードを混在させないようにします。  
  
 仮に、コレクションのセマンティクスと、XML ツリーを変更するメソッドのセマンティクスを正確に把握していて、このカテゴリの問題を巧妙に回避するコードを記述できたとしても、将来そのコードの保守を行う別の開発者が同じようにこの問題に通じているとは限りません。 宣言型と命令型のコーディング スタイルが混在していると、コードが不安定になります。  
  
 これらの問題を回避するためにコレクションを具体化するコードを記述する場合は、コードを保守するプログラマが問題を把握できるように、必要に応じてコメントを付けるようにしてください。  
  
 第 2 に、パフォーマンスやその他の事情が許すのであれば、宣言型のコードのみを使用するようにします。 既存の XML ツリーは変更せずに、 新しいツリーを生成します。  
  
```vb  
Dim root As XElement = _  
    <Root>  
        <A>1</A>  
        <B>2</B>  
        <C>3</C>  
    </Root>  
Dim newRoot As XElement = New XElement("Root", _  
    root.Elements(), root.Elements())  
Console.WriteLine(newRoot)  
```  
  
## <a name="see-also"></a>参照

- [高度な LINQ to XML プログラミング (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/advanced-linq-to-xml-programming.md)
