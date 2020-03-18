---
title: オーバーロードされたプロパティとメソッド
ms.date: 07/20/2015
helpviewer_keywords:
- properties [Visual Basic], overloading
- methods [Visual Basic], overloading
- shadowing
- names [Visual Basic], multiple procedures with same
- procedures [Visual Basic], multiples with same name
- classes [Visual Basic], properties
- classes [Visual Basic], methods
- method overloading
- Overloads keyword [Visual Basic], overloaded members
ms.assetid: b686fb97-e7d7-4001-afaa-6650cba08f0d
ms.openlocfilehash: a5017d371f8a01436020443b2e3466c78fc35d21
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346089"
---
# <a name="overloaded-properties-and-methods-visual-basic"></a>オーバーロードされたプロパティとメソッド (Visual Basic)

オーバーロードとは、同じ名前で引数の型が異なるクラスで、複数のプロシージャ、インスタンスコンストラクター、またはプロパティを作成することです。

## <a name="overloading-usage"></a>オーバーロードの使用

オーバーロードは、さまざまなデータ型を操作するプロシージャに対して、オブジェクトモデルで同じ名前を使用することが指示された場合に特に便利です。 たとえば、いくつかの異なるデータ型を表示できるクラスでは、次のような `Display` のプロシージャが存在する可能性があります。

[!code-vb[VbVbalrOOP#64](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#64)]

オーバーロードを使用しない場合は、次に示すように、プロシージャごとに個別の名前を作成する必要があります。

[!code-vb[VbVbalrOOP#65](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#65)]

オーバーロードを使用すると、使用可能なデータ型を選択できるため、プロパティやメソッドの使用が簡単になります。 たとえば、前に説明したオーバーロードされた `Display` メソッドは、次のいずれかのコード行を使用して呼び出すことができます。

[!code-vb[VbVbalrOOP#66](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#66)]

実行時に、Visual Basic は、指定したパラメーターのデータ型に基づいて正しいプロシージャを呼び出します。

## <a name="overloading-rules"></a>オーバーロード (規則を)

 クラスのオーバーロードされたメンバーを作成するには、同じ名前を持つ2つ以上のプロパティまたはメソッドを追加します。 オーバーロードされた派生メンバーを除き、オーバーロードされた各メンバーは異なるパラメーターリストを持つ必要があります。また、プロパティまたはプロシージャをオーバーロードするときに、次の項目を区別機能として使用することはできません。

- メンバーまたはメンバーのパラメーターに適用される、`ByVal` や `ByRef`などの修飾子。

- パラメーターの名前

- プロシージャの戻り値の型

`Overloads` キーワードはオーバーロード時には省略可能ですが、オーバーロードされたメンバーが `Overloads` キーワードを使用している場合は、同じ名前の他のすべてのオーバーロードされたメンバーもこのキーワードを指定する必要があります。

派生クラスでは、同じパラメーターとパラメーターの型を持つメンバーを持つ継承されたメンバーをオーバーロードできます。これは、*名前とシグネチャによるシャドウ*処理と呼ばれるプロセスです。 名前とシグネチャでシャドウするときに `Overloads` キーワードを使用すると、基底クラスの実装ではなく、派生クラスのメンバーの実装が使用され、そのメンバーの他のすべてのオーバーロードが派生クラスのインスタンスで使用できるようになります。

同じパラメーターとパラメーターの型を持つメンバーを持つ継承されたメンバーをオーバーロードするときに `Overloads` キーワードを省略すると、オーバーロードは*名前によるシャドウ*処理と呼ばれます。 名前によるシャドウ処理により、メンバーの継承された実装が置き換えられます。これにより、派生クラスのインスタンスとその他のすべてのオーバーロードが使用できなくなります。

`Overloads` と `Shadows` の修飾子を同じプロパティまたはメソッドと共に使用することはできません。

### <a name="example"></a>例

次の例では、金額の `String` または `Decimal` 表現を受け入れるオーバーロードされたメソッドを作成し、売上税を含む文字列を返します。

#### <a name="to-use-this-example-to-create-an-overloaded-method"></a>この例を使用して、オーバーロードされたメソッドを作成するには

1. 新しいプロジェクトを開き、`TaxClass`という名前のクラスを追加します。

2. `TaxClass` クラスに次のコードを追加します。

    [!code-vb[VbVbalrOOP#67](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#67)]

3. 次のプロシージャをフォームに追加します。

    [!code-vb[VbVbalrOOP#68](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#68)]

4. フォームにボタンを追加し、ボタンの `Button1_Click` イベントから `ShowTax` プロシージャを呼び出します。

5. プロジェクトを実行し、フォームのボタンをクリックして、オーバーロードされた `ShowTax` プロシージャをテストします。

実行時に、コンパイラは、使用されているパラメーターに一致するオーバーロードされた適切な関数を選択します。 このボタンをクリックすると、オーバーロードされたメソッドが最初に呼び出されます。このメソッドは、文字列である `Price` パラメーターと "Price は文字列です。 税 $5.12 "が表示されます。 2回目の `Decimal` 値を使用して `TaxAmount` が呼び出されます。 "Price は Decimal です。 税 $5.12 "が表示されます。

## <a name="see-also"></a>参照

- [クラスとオブジェクト](../../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
- [Visual Basic でのシャドウ処理](../../../../visual-basic/programming-guide/language-features/declared-elements/shadowing.md)
- [Sub ステートメント](../../../../visual-basic/language-reference/statements/sub-statement.md)
- [継承の基本](../../../../visual-basic/programming-guide/language-features/objects-and-classes/inheritance-basics.md)
- [Shadows](../../../../visual-basic/language-reference/modifiers/shadows.md)
- [ParamArray](../../../../visual-basic/language-reference/modifiers/byval.md)
- [ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)
- [Overloads](../../../../visual-basic/language-reference/modifiers/overloads.md)
- [Shadows](../../../../visual-basic/language-reference/modifiers/shadows.md)
