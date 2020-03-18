---
title: '方法 : 匿名型の宣言におけるプロパティ名と型を推論する'
ms.date: 07/20/2015
helpviewer_keywords:
- inferring property names [Visual Basic]
- anonymous types [Visual Basic], inferring property names and types
- inferring property types [Visual Basic]
ms.assetid: 7c748b22-913f-4d9d-b747-6b7bf296a0bc
ms.openlocfilehash: 89a39e8e9cd66b1d774da70be47c7c6824cccef2
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350036"
---
# <a name="how-to-infer-property-names-and-types-in-anonymous-type-declarations-visual-basic"></a>方法: 匿名型の宣言におけるプロパティ名と型を推論する (Visual Basic)

匿名型には、プロパティのデータ型を直接指定する機構はありません。 すべてのプロパティの型は、推論されます。 次の例では、 `Name` と `Price` の型は、それらを初期化するために使われる値から、直接推論されます。

[!code-vb[VbVbalrAnonymousTypes#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#1)]

匿名型は、プロパティの名前と型を、他のソースから推論することもできます。 この後のセクションで、推論が可能な状況の一覧と、推論が行われない状況の例を示します。

## <a name="successful-inference"></a>正常な推論

#### <a name="anonymous-types-can-infer-property-names-and-types-from-the-following-sources"></a>匿名型は、プロパティの名前と型を、次のソースから推論できます:

- 変数名から。 匿名型 `anonProduct` には、 `productName` と `productPrice`という 2 つのプロパティがあります。 それらのプロパティのデータ型は、それぞれ、元の変数のデータ型である `String` と `Double`になります。

  [!code-vb[VbVbalrAnonymousTypes#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#11)]

- 他のオブジェクトのプロパティまたはフィールドの名前から。 たとえば、 `car` プロパティと `CarClass` プロパティを含む `Name` 型の `ID` オブジェクトがあるとします。 新しい匿名型のインスタンス `car1`を作成し、 `Name` オブジェクトの値を使用して `ID` プロパティと `car` プロパティを初期化するには、次のように記述できます。

  [!code-vb[VbVbalrAnonymousTypes#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#34)]

  前の宣言は、匿名型 `car2`を定義する、次の長いコードに相当します。

  [!code-vb[VbVbalrAnonymousTypes#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#35)]

- XML メンバー名から。

  [!code-vb[VbVbalrAnonymousTypes#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#12)]

  推論後の `anon` の型は、 `Book`(Of XElement) 型の 1 つのプロパティ <xref:System.Collections.IEnumerable>を持ちます。

- 次の例に示す `SomeFunction` など、パラメーターを持たない関数から。

  ```vb
  Dim sc As New SomeClass
  Dim anon1 = New With {Key sc.SomeFunction()}
  ```

  次のコードの `anon2` 変数は、 `First`という名前の 1 文字を表すプロパティを 1 つ持つ匿名型です。 このコードでは、文字 "E" が表示されます。これは関数 <xref:System.Linq.Enumerable.First%2A>から返される文字です。

  [!code-vb[VbVbalrAnonymousTypes#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#13)]

## <a name="inference-failures"></a>推論の失敗

#### <a name="name-inference-will-fail-in-many-circumstances-including-the-following"></a>名前の推論は、次の状況を含む、さまざまな状況で失敗します:

- 推論が、引数を必要とするメソッド、コンストラクター、またはパラメーター化されたプロパティの呼び出しから派生する場合。 前の `anon1` の宣言は、 `someFunction` に 1 つ以上の引数があると失敗します。

  ```vb
  ' Not valid.
  ' Dim anon3 = New With {Key sc.someFunction(someArg)}
  ```

  この問題は、新しいプロパティ名を割り当てることによって解決できます。

  ```vb
  ' Valid.
  Dim anon4 = New With {Key .FunResult = sc.someFunction(someArg)}
  ```

- 推論が、複合式から派生する場合。

  ```vb
  Dim aString As String = "Act "
  ' Not valid.
  ' Dim label = New With {Key aString & "IV"}
  ```

  このエラーは、式の結果をプロパティ名に割り当てれば解決できます。

  [!code-vb[VbVbalrAnonymousTypes#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#14)]

- 複数のプロパティの推論で、同じ名前を持つ 2 つ以上のプロパティが生成される場合。 前の例で示した宣言で説明すると、同じ匿名型のプロパティとして `product.Name` と `car1.Name` の両方を指定することはできません。 これは、各プロパティの推論された識別子がどちらも `Name`になるためです。

  ```vb
  ' Not valid.
  ' Dim anon5 = New With {Key product.Name, Key car1.Name}
  ```

  この問題は、別個のプロパティ名に値を割り当てることで解決できます。

  [!code-vb[VbVbalrAnonymousTypes#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#36)]

  大文字と小文字の違いでは、別個の名前にならないことにご注意ください。

  ```vb
  Dim price = 0
  ' Not valid, because Price and price are the same name.
  ' Dim anon7 = New With {Key product.Price, Key price}
  ```

- 1 つのプロパティの初期の型と値が、まだ確立されていない別のプロパティに依存している場合。 たとえば、 `.IDName = .LastName` は、 `.LastName` が既に初期化されていない限り、匿名型の宣言では使えません。

  ```vb
  ' Not valid.
  ' Dim anon8 = New With {Key .IDName = .LastName, Key .LastName = "Jones"}
  ```

  この例では、プロパティを宣言する順序を逆にすることで、問題を修正できます。

  [!code-vb[VbVbalrAnonymousTypes#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#15)]

- 匿名型のプロパティの名前が、 <xref:System.Object>のメンバーの名前と同じである場合。 たとえば、次の宣言は、 `Equals` が <xref:System.Object>のメソッドなので失敗します。

  ```vb
  ' Not valid.
  ' Dim relationsLabels1 = New With {Key .Equals = "equals", Key .Greater = _
  '                       "greater than", Key .Less = "less than"}
  ```

  この問題は、プロパティ名を変更することで修正できます。

  [!code-vb[VbVbalrAnonymousTypes#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#16)]

## <a name="see-also"></a>関連項目

- [オブジェクト初期化子 : 名前付きの型と匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
- [匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)
- [Key](../../../../visual-basic/language-reference/modifiers/key.md)
