---
title: Partial (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- vb.Partial
- partial
helpviewer_keywords:
- structures [Visual Basic], partial
- classes [Visual Basic]
- partial, types [Visual Basic]
- partial, structures
- partial, classes [Visual Basic]
- classes [Visual Basic], partial
- Partial keyword [Visual Basic]
- type promotion
ms.assetid: 7adaef80-f435-46e1-970a-269fff63b448
ms.openlocfilehash: df85571b757fd54496677bad1195fab9690b79cc
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351360"
---
# <a name="partial-visual-basic"></a>Partial (Visual Basic)
型宣言が、型の部分定義であることを示します。  
  
 `Partial` キーワードを使用して、型の定義を複数の宣言に分割できます。 部分宣言は必要に応じていくつでも使用でき、複数のソース ファイルとして作成することもできます。 ただし、すべての宣言は同じアセンブリおよび同じ名前空間にある必要があります。  
  
> [!NOTE]
> Visual Basic では、部分クラスで通常実装される*部分メソッド*がサポートされます。 詳細については、「[部分メソッド](../../../visual-basic/programming-guide/language-features/procedures/partial-methods.md)と[Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)」を参照してください。  
  
## <a name="syntax"></a>構文  
  
```vb  
[ <attrlist> ] [ accessmodifier ] [ Shadows ] [ MustInherit | NotInheritable ] _  
Partial { Class | Structure | Interface | Module } name [ (Of typelist) ]  
    [ Inherits classname ]  
    [ Implements interfacenames ]  
    [ variabledeclarations ]  
    [ proceduredeclarations ]  
{ End Class | End Structure }  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`attrlist`|省略可。 この型に適用される属性の一覧です。 [属性リスト](../../../visual-basic/language-reference/statements/attribute-list.md)は山かっこ (`< >`) で囲む必要があります。|  
|`accessmodifier`|省略可。 どのようなコードから型にアクセスできるのかを指定します。 「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。|  
|`Shadows`|省略可。 「[Shadows](../../../visual-basic/language-reference/modifiers/shadows.md)」を参照してください。|  
|`MustInherit`|省略可。 「[MustInherit](../../../visual-basic/language-reference/modifiers/mustinherit.md)」を参照してください。|  
|`NotInheritable`|省略可。 「[NotInheritable](../../../visual-basic/language-reference/modifiers/notinheritable.md)」を参照してください。|  
|`name`|必須。 この型の名前です。 同じ型の他のすべての部分宣言で定義されている名前と一致する必要があります。|  
|`Of`|省略可。 これがジェネリック型であることを指定します。 「[Visual Basic におけるジェネリック型](../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)」を参照してください。|  
|`typelist`|[Of](../../../visual-basic/language-reference/statements/of-clause.md) を使用する場合は必須です。 [型リスト](../../../visual-basic/language-reference/statements/type-list.md)を参照してください。|  
|`Inherits`|省略可。 「[Inherits ステートメント](../../../visual-basic/language-reference/statements/inherits-statement.md)」を参照してください。|  
|`classname`|`Inherits` を使用する場合は必ず指定します。 このクラスの派生元のクラスまたはインターフェイスの名前です。|  
|`Implements`|省略可。 「[Implements ステートメント](../../../visual-basic/language-reference/statements/implements-statement.md)」を参照してください。|  
|`interfacenames`|`Implements` を使用する場合は必ず指定します。 この型が実装するインターフェイスの名前を指定します。|  
|`variabledeclarations`|省略可。 この型の追加の変数やイベントを宣言するステートメントです。|  
|`proceduredeclarations`|省略可。 この型の追加のプロシージャを宣言および定義するステートメントです。|  
|`End Class` または `End Structure`|この `Class` または `Structure` の部分定義を終了します。|  
  
## <a name="remarks"></a>コメント  
 Visual Basic では、部分クラス定義を使用して、生成されたコードとユーザーが作成したコードとを別々のソース ファイルに分離します。 たとえば、**Windows フォーム デザイナー**では、<xref:System.Windows.Forms.Form> などのコントロールに部分クラスを定義します。 これらのコントロールでは、生成されたコードを変更しないでください。  
  
 部分型を作成する際、クラス、構造体、インターフェイス、およびモジュールの作成に関するすべての規則 (修飾子の利用法や継承に関する規則など) が適用されます。  
  
## <a name="best-practices"></a>ベスト プラクティス  
  
- 通常の状況では、1 つの型の開発を複数の宣言に分割しないようにします。 したがって、ほとんどの場合は、`Partial` キーワードは必要ありません。  
  
- 読みやすくするために、型の部分宣言にはすべて、`Partial` キーワードを含めます。 コンパイラでは、最大 1 つの部分宣言でキーワードを省略できますが、複数の部分宣言で省略するとエラーになります。  
  
## <a name="behavior"></a>動作  
  
- **宣言の和集合。** コンパイラは型を、すべての部分宣言の共用体として扱います。 すべての部分定義で使用されているすべての修飾子は、型全体に適用され、すべての部分定義のすべてのメンバーは、型全体で使用できます。  
  
- **型の上位変換は、モジュールの部分型では許可されていません。** 部分定義がモジュール内にある場合、その型の上位変換は自動的に失敗します。 この場合、一連の部分定義によって、予期しない結果になったり、場合によってはコンパイラ エラーが発生することがあります。 詳細については、「[型の上位変換](../../../visual-basic/programming-guide/language-features/declared-elements/type-promotion.md)」を参照してください。  
  
     コンパイラは、完全修飾されたパスがまったく同じ場合にのみ、部分定義をマージします。  
  
 キーワード `Partial` は次のコンテキストで使用できます。  
  
 [Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md)  
  
 [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)  
  
## <a name="example"></a>例  
 次の例では、クラス `sampleClass` の定義を 2 つの宣言に分割し、それぞれ別の `Sub` プロシージャを定義します。  
  
 [!code-vb[VbVbalrKeywords#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#3)]  
  
 この例にある 2 つの部分定義は、同じソース ファイル内にあっても、別々のソース ファイル内にあってもかまいません。  
  
## <a name="see-also"></a>参照

- [Class ステートメント](../../../visual-basic/language-reference/statements/class-statement.md)
- [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)
- [型の上位変換](../../../visual-basic/programming-guide/language-features/declared-elements/type-promotion.md)
- [Shadows](../../../visual-basic/language-reference/modifiers/shadows.md)
- [Visual Basic におけるジェネリック型](../../../visual-basic/programming-guide/language-features/data-types/generic-types.md)
- [部分メソッド](../../../visual-basic/programming-guide/language-features/procedures/partial-methods.md)
