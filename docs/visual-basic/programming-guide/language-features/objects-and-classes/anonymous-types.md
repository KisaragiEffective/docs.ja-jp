---
title: 匿名型
ms.date: 07/20/2015
f1_keywords:
- vb.AnonymousType
helpviewer_keywords:
- anonymous types [Visual Basic], about anonymous types
- anonymous types [Visual Basic]
- types [Visual Basic], anonymous
ms.assetid: 7b87532c-4b3e-4398-8503-6ea9d67574a4
ms.openlocfilehash: 064c43274069be3951f816eaafafac0bbece7651
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347157"
---
# <a name="anonymous-types-visual-basic"></a>匿名型 (Visual Basic)
Visual Basic は匿名型をサポートしているため、データ型のクラス定義を記述せずにオブジェクトを作成できます。 クラスは、コンパイラによって生成されます。 クラスには使用可能な名前がなく、<xref:System.Object>から直接継承し、オブジェクトの宣言で指定したプロパティが含まれています。 データ型の名前は指定されていないため、*匿名型*と呼ばれます。  
  
 次の例では、`Name` と `Price`の2つのプロパティを持つ匿名型のインスタンスとして、変数 `product` を宣言して作成します。  
  
 [!code-vb[VbVbalrAnonymousTypes#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#1)]  
  
 *クエリ式*では、クエリによって選択されたデータの列を結合するために、匿名型を使用します。 特定のクエリによって選択される列を予測できないため、結果の型を事前に定義することはできません。 匿名型を使用すると、任意の数の列を任意の順序で選択するクエリを記述できます。 コンパイラは、指定されたプロパティと指定された順序に一致するデータ型を作成します。  
  
 次の例では、`products` が製品オブジェクトの一覧であり、それぞれに多くのプロパティがあります。 変数 `namePriceQuery` には、クエリの定義が保持されます。クエリを実行すると、`Name` と `Price`の2つのプロパティを持つ匿名型のインスタンスのコレクションが返されます。  
  
 [!code-vb[VbVbalrAnonymousTypes#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#2)]  
  
 変数 `nameQuantityQuery` には、クエリの定義が保持されます。クエリを実行すると、`Name` と `OnHand`の2つのプロパティを持つ匿名型のインスタンスのコレクションが返されます。  
  
 [!code-vb[VbVbalrAnonymousTypes#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#3)]  
  
 匿名型のコンパイラによって作成されたコードの詳細については、「[匿名型の定義](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-type-definition.md)」を参照してください。  
  
> [!CAUTION]
> 匿名型の名前はコンパイラによって生成されるため、コンパイルごとに異なる場合があります。 プロジェクトを再コンパイルするときに名前が変更される可能性があるため、コードで匿名型の名前を使用したり、使用したりしないでください。  
  
## <a name="declaring-an-anonymous-type"></a>匿名型の宣言  
 匿名型のインスタンスの宣言では、初期化子リストを使用して、型のプロパティを指定します。 メソッドやイベントなどの他のクラス要素ではなく、匿名型を宣言する場合は、プロパティのみを指定できます。 次の例では、`product1` は、`Name` と `Price`の2つのプロパティを持つ匿名型のインスタンスです。  
  
 [!code-vb[VbVbalrAnonymousTypes#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#4)]  
  
 プロパティをキープロパティとして指定した場合は、これらのプロパティを使用して、2つの匿名型インスタンスが等しいかどうかを比較できます。 ただし、キープロパティの値は変更できません。 詳細については、このトピックで後述する「キーのプロパティ」を参照してください。  
  
 匿名型のインスタンスを宣言することは、オブジェクト初期化子を使用して名前付きの型のインスタンスを宣言するのと似ています。  
  
 [!code-vb[VbVbalrAnonymousTypes#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#5)]  
  
 匿名型のプロパティを指定するその他の方法の詳細については、「[方法: 匿名型の宣言でプロパティの名前と型を推論](../../../../visual-basic/programming-guide/language-features/objects-and-classes/how-to-infer-property-names-and-types-in-anonymous-type-declarations.md)する」を参照してください。  
  
## <a name="key-properties"></a>キー プロパティ  
 キープロパティは、次のいくつかの基本的な方法で、キー以外のプロパティとは異なります。  
  
- 2つのインスタンスが等しいかどうかを判断するために、キープロパティの値のみが比較されます。  
  
- キープロパティの値は読み取り専用で、変更することはできません。  
  
- コンパイラによって生成された匿名型のハッシュコードアルゴリズムには、キープロパティ値のみが含まれます。  
  
### <a name="equality"></a>等価比較  
 匿名型のインスタンスは、同じ匿名型のインスタンスである場合にのみ、同じにすることができます。 コンパイラは、次の条件を満たす場合、2つのインスタンスを同じ型のインスタンスとして扱います。  
  
- これらは同じアセンブリ内で宣言されています。  
  
- これらのプロパティは同じ名前を持ち、推論される型は同じであり、同じ順序で宣言されています。 名前比較では、大文字と小文字は区別されません。  
  
- 各の同じプロパティは、キープロパティとしてマークされます。  
  
- 各宣言の少なくとも1つのプロパティがキープロパティです。  
  
 キープロパティを持たない匿名型のインスタンスは、それ自体と同じです。  
  
 [!code-vb[VbVbalrAnonymousTypes#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#6)]  
  
 キープロパティの値が等しい場合、同じ匿名型の2つのインスタンスは等しいと見なされます。 次の例は、等値のテスト方法を示しています。  
  
 [!code-vb[VbVbalrAnonymousTypes#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#7)]  
  
### <a name="read-only-values"></a>読み取り専用の値  
 キープロパティの値は変更できません。 たとえば、前の例の `prod8` では、`Name` フィールドと `Price` フィールドが `read-only`ますが、`OnHand` は変更できます。  
  
 [!code-vb[VbVbalrAnonymousTypes#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#8)]  
  
## <a name="anonymous-types-from-query-expressions"></a>クエリ式からの匿名型  
 クエリ式では、必ずしも匿名型を作成する必要はありません。 可能な場合は、既存の型を使用して列データを保持します。 このエラーは、クエリがデータソースからレコード全体を返すか、または各レコードのフィールドを1つだけ返す場合に発生します。 次のコード例では、`customers` は `Customer` クラスのオブジェクトのコレクションです。 クラスには多くのプロパティがあり、クエリの結果には、任意の順序で1つ以上のプロパティを含めることができます。 最初の2つの例では、クエリは名前付きの型の要素を選択するため、匿名型は必要ありません。  
  
- `cust.Name` が文字列であるため、`custs1` には文字列のコレクションが含まれています。  
  
     [!code-vb[VbVbalrAnonymousTypes#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#30)]  
  
- `custs2` には、`Customer` オブジェクトのコレクションが含まれています。 `customers` の各要素は `Customer` オブジェクトであり、要素全体がクエリによって選択されているためです。  
  
     [!code-vb[VbVbalrAnonymousTypes#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#31)]  
  
 ただし、適切な名前付きの型は常に使用できるとは限りません。 お客様は、1つの目的で顧客の名前と住所、別の顧客の ID 番号と場所、顧客名、住所、および注文履歴を選択することができます。 匿名型を使用すると、プロパティの任意の組み合わせを任意の順序で選択できます。最初に、結果を保持するために新しい名前付きの型を宣言する必要はありません。 代わりに、コンパイラは、プロパティのコンパイルごとに匿名型を作成します。 次のクエリでは、`customers`の各 `Customer` オブジェクトから、顧客の名前と ID 番号のみを選択します。 したがって、コンパイラは、これら2つのプロパティのみを含む匿名型を作成します。  
  
 [!code-vb[VbVbalrAnonymousTYpes#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#32)]  
  
 匿名型のプロパティの名前とデータ型は両方とも、`Select`、`cust.Name`、および `cust.ID`の引数から取得されます。 クエリによって作成される匿名型のプロパティは、常にキープロパティです。 次の `For Each` ループで `custs3` を実行すると、`Name` と `ID`の2つのキープロパティを持つ匿名型のインスタンスのコレクションが生成されます。  
  
 [!code-vb[VbVbalrAnonymousTypes#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#33)]  
  
 `custs3` によって表されるコレクション内の要素は厳密に型指定されます。 IntelliSense を使用して、使用可能なプロパティ間を移動したり、その型を検証したりできます。  
  
 詳細については、「 [Visual Basic での LINQ の概要](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)」を参照してください。  
  
## <a name="deciding-whether-to-use-anonymous-types"></a>匿名型を使用するかどうかの決定  
 オブジェクトを匿名クラスのインスタンスとして作成する前に、それが最適なオプションであるかどうかを検討してください。 たとえば、関連データを格納する一時オブジェクトを作成する場合に、完全なクラスに含まれる可能性のある他のフィールドやメソッドが不要な場合は、匿名型を使用することをお勧めします。 匿名型は、各宣言に対して異なるプロパティを選択する場合や、プロパティの順序を変更する場合にも便利です。 ただし、同じプロパティを持つ複数のオブジェクトがプロジェクトに含まれている場合は、クラスコンストラクターを持つ名前付きの型を使用して、より簡単に宣言することができます。 たとえば、適切なコンストラクターを使用すると、匿名型の複数のインスタンスを宣言するよりも、`Product` クラスの複数のインスタンスを宣言する方が簡単になります。  
  
 [!code-vb[VbVbalrAnonymousTypes#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#9)]  
  
 名前付きの型のもう1つの利点は、コンパイラがプロパティ名の誤入力をキャッチできることです。 前の例では、`firstProd2`、`secondProd2`、および `thirdProd2` は、同じ匿名型のインスタンスとして意図されています。 ただし、次のいずれかの方法で誤って `thirdProd2` を宣言すると、その型が `firstProd2` と `secondProd2`とは異なることになります。  
  
 [!code-vb[VbVbalrAnonymousTypes#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#10)]  
  
 さらに重要な点として、名前付きの型のインスタンスには適用されない匿名型の使用に関する制限があります。 `firstProd2`、`secondProd2`、および `thirdProd2` は、同じ匿名型のインスタンスです。 ただし、共有されている匿名型の名前は使用できません。また、コードで型名を指定する場所を指定することはできません。 たとえば、匿名型を使用して、メソッドシグネチャを定義したり、別の変数やフィールドを宣言したり、任意の型宣言で宣言したりすることはできません。 そのため、メソッド間で情報を共有する必要がある場合、匿名型は適切ではありません。  
  
## <a name="an-anonymous-type-definition"></a>匿名型の定義  
 匿名型のインスタンスの宣言に応答して、コンパイラは、指定されたプロパティを含む新しいクラス定義を作成します。  
  
 匿名型に少なくとも1つのキープロパティが含まれている場合、<xref:System.Object>: <xref:System.Object.Equals%2A>、<xref:System.Object.GetHashCode%2A>、および <xref:System.Object.ToString%2A>から継承された3つのメンバーは、定義によってオーバーライドされます。 等価性をテストし、ハッシュコード値を決定するために生成されたコードは、キープロパティのみを考慮します。 匿名型にキープロパティが含まれていない場合は、<xref:System.Object.ToString%2A> のみがオーバーライドされます。 匿名型の明示的に名前が付けられたプロパティは、これらの生成されたメソッドと競合しません。 つまり、`.Equals`、`.GetHashCode`、または `.ToString` を使用してプロパティの名前を指定することはできません。  
  
 少なくとも1つのキープロパティを持つ匿名型定義は、<xref:System.IEquatable%601?displayProperty=nameWithType> インターフェイスも実装します。 `T` は匿名型の型です。  
  
 コンパイラによって作成されたコードと、オーバーライドされたメソッドの機能の詳細については、「[匿名型の定義](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-type-definition.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [オブジェクト初期化子 : 名前付きの型と匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
- [Visual Basic における LINQ の概要](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [方法 : 匿名型の宣言におけるプロパティ名と型を推論する](../../../../visual-basic/programming-guide/language-features/objects-and-classes/how-to-infer-property-names-and-types-in-anonymous-type-declarations.md)
- [匿名型の定義](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-type-definition.md)
- [Key](../../../../visual-basic/language-reference/modifiers/key.md)
