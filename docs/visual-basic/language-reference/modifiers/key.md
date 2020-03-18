---
title: Key
ms.date: 07/20/2015
f1_keywords:
- vb.AnonymousKey
helpviewer_keywords:
- anonymous types [Visual Basic], key
- Key [Visual Basic]
- Key keyword [Visual Basic]
ms.assetid: 7697a928-7d14-4430-a72a-c9e96e8d6c11
ms.openlocfilehash: 92c8809779d6cab524f67ee47f355b72ab152403
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351518"
---
# <a name="key-visual-basic"></a>Key (Visual Basic)
`Key` キーワードを使用すると、匿名型のプロパティの動作を指定できます。 キープロパティとして指定したプロパティのみが、匿名型インスタンス間の等価性のテスト、またはハッシュコード値の計算に参加します。 キープロパティの値は変更できません。  
  
 匿名型のプロパティをキープロパティとして指定するには、初期化リストの宣言の前にキーワード `Key` を配置します。 次の例では、`Airline` と `FlightNo` はキープロパティですが、`Gate` はありません。  
  
 [!code-vb[VbVbalrAnonymousTypes#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#26)]  
  
 新しい匿名型を作成すると、<xref:System.Object>から直接継承されます。 コンパイラは、継承された3つのメンバー (<xref:System.Object.Equals%2A>、<xref:System.Object.GetHashCode%2A>、および <xref:System.Object.ToString%2A>) をオーバーライドします。 <xref:System.Object.Equals%2A> と <xref:System.Object.GetHashCode%2A> に対して生成されるオーバーライドコードは、キープロパティに基づいています。 型にキープロパティがない場合、<xref:System.Object.GetHashCode%2A> と <xref:System.Object.Equals%2A> はオーバーライドされません。  
  
## <a name="equality"></a>等価比較  
 2つの匿名型のインスタンスは、同じ型のインスタンスであり、キープロパティの値が等しい場合に等しくなります。 次の例では、`flight2` は、同じ匿名型のインスタンスであり、キープロパティの値が一致しているため、前の例の `flight1` と同じです。 ただし、キープロパティの値が異なるため、`flight3` は `flight1` と同じではありません。 `FlightNo`。 インスタンス `flight4` が `flight1` と同じ型ではありません。キープロパティとして異なるプロパティが指定されているためです。  
  
 [!code-vb[VbVbalrAnonymousTypes#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#27)]  
  
 2つのインスタンスが、名前、型、順序、および値のキー以外のプロパティのみで宣言されている場合、2つのインスタンスが等しくないことを示します。 キープロパティのないインスタンスは、それ自体と同じです。  
  
 2つの匿名型インスタンスが同じ匿名型のインスタンスである場合の条件の詳細については、「[匿名型](../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)」を参照してください。  
  
## <a name="hash-code-calculation"></a>ハッシュコードの計算  
 <xref:System.Object.Equals%2A>と同様に、匿名型の <xref:System.Object.GetHashCode%2A> で定義されているハッシュ関数は、型のキープロパティに基づいています。 次の例は、キープロパティとハッシュコード値の間の相互作用を示しています。  
  
 すべてのキープロパティに同じ値を持つ匿名型のインスタンスのハッシュコード値は、キー以外のプロパティに一致する値がない場合でも同じです。 次のステートメントは `True`を返します。  
  
 [!code-vb[VbVbalrAnonymousTypes#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#37)]  
  
 1つまたは複数のキープロパティの値が異なる匿名型のインスタンスには、異なるハッシュコード値があります。 次のステートメントは `False`を返します。  
  
 [!code-vb[VbVbalrAnonymousTypes#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#38)]  
  
 キープロパティとして異なるプロパティを指定する匿名型のインスタンスは、同じ型のインスタンスではありません。 すべてのプロパティの名前と値が同じであっても、ハッシュコードの値は異なります。 次のステートメントは `False`を返します。  
  
 [!code-vb[VbVbalrAnonymousTypes#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#39)]  
  
## <a name="read-only-values"></a>読み取り専用の値  
 キープロパティの値は変更できません。 たとえば、前の例の `flight1` では、`Airline` フィールドと `FlightNo` フィールドは読み取り専用ですが、`Gate` は変更できます。  
  
 [!code-vb[VbVbalrAnonymousTypes#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#28)]  
  
## <a name="see-also"></a>参照

- [匿名型の定義](../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-type-definition.md)
- [方法 : 匿名型の宣言におけるプロパティ名と型を推論する](../../../visual-basic/programming-guide/language-features/objects-and-classes/how-to-infer-property-names-and-types-in-anonymous-type-declarations.md)
- [匿名型](../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)
