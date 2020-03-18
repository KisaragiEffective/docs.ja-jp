---
title: オブジェクトの型の決定
ms.date: 07/20/2015
helpviewer_keywords:
- classes [Visual Basic], discovering which an object belongs to
- types [Visual Basic], determining Visual Basic object types
- object variables [Visual Basic], testing values
- TypeOf...Is expression, object type at run time
- TypeName function
- objects [Visual Basic], type determining
ms.assetid: d95e7ad1-cd63-41d6-9a28-d7a1380d49c1
ms.openlocfilehash: a77cc0603a0b61f58a4aa703c4b1e6ef4c26729c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345199"
---
# <a name="determining-object-type-visual-basic"></a>オブジェクトの型の決定 (Visual Basic)
ジェネリックオブジェクト変数 (つまり、`Object`として宣言する変数) は、任意のクラスのオブジェクトを保持できます。 `Object`型の変数を使用する場合、オブジェクトのクラスに基づいて異なるアクションを実行することが必要になる場合があります。たとえば、一部のオブジェクトでは、特定のプロパティまたはメソッドがサポートされていない場合があります。 Visual Basic には、オブジェクト変数に格納されているオブジェクトの種類を判別する2つの方法があります。 `TypeName` 関数と `TypeOf...Is` 演算子です。  
  
## <a name="typename-and-typeofis"></a>TypeName と TypeOf...Is  
 `TypeName` 関数は文字列を返します。次のコードに示すように、オブジェクトのクラス名を格納または表示する必要がある場合に最適な選択肢です。  
  
 [!code-vb[VbVbalrOOP#92](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#92)]  
  
 `TypeOf...Is` 演算子は、`TypeName`を使用した同等の文字列比較よりもはるかに高速であるため、オブジェクトの型をテストするのに最適な選択肢です。 次のコード片では、`If...Then...Else` ステートメント内で `TypeOf...Is` を使用します。  
  
 [!code-vb[VbVbalrOOP#93](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#93)]  
  
 ここでは注意が必要です。 `TypeOf...Is` 演算子は、オブジェクトが特定の型であるか、特定の型から派生している場合に `True` を返します。 Visual Basic で行うほぼすべての要素にはオブジェクトが含まれます。これには、文字列や整数など、通常はオブジェクトとして見なされない一部の要素が含まれます。 これらのオブジェクトは、から派生し、<xref:System.Object>からメソッドを継承します。 `Integer` が渡され、`Object`で評価されると、`TypeOf...Is` 演算子は `True`を返します。 次の例では、パラメーター `InParam` が `Object` と `Integer`の両方であることを報告しています。  
  
 [!code-vb[VbVbalrOOP#94](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#94)]  
  
 次の例では、`TypeOf...Is` と `TypeName` の両方を使用して、`Ctrl` 引数で渡されたオブジェクトの型を確認します。 `TestObject` プロシージャは、3種類のコントロールを使用して `ShowType` を呼び出します。  
  
#### <a name="to-run-the-example"></a>例を実行するには  
  
1. 新しい Windows アプリケーションプロジェクトを作成し、<xref:System.Windows.Forms.Button> コントロール、<xref:System.Windows.Forms.CheckBox> コントロール、および <xref:System.Windows.Forms.RadioButton> コントロールをフォームに追加します。  
  
2. フォームのボタンから `TestObject` プロシージャを呼び出します。  
  
3. 次のコードをフォームに追加します。  
  
     [!code-vb[VbVbalrOOP#95](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#95)]  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Information.TypeName%2A>
- [文字列名によるプロパティまたはメソッドの呼び出し](../../../../visual-basic/programming-guide/language-features/early-late-binding/calling-a-property-or-method-using-a-string-name.md)
- [Object Data Type](../../../../visual-basic/language-reference/data-types/object-data-type.md)
- [If...Then...Else ステートメント](../../../../visual-basic/language-reference/statements/if-then-else-statement.md)
- [String データ型](../../../../visual-basic/language-reference/data-types/string-data-type.md)
- [Integer データ型](../../../../visual-basic/language-reference/data-types/integer-data-type.md)
