---
title: Get ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Get
helpviewer_keywords:
- Get statement [Visual Basic], syntax
- Get statement [Visual Basic]
- properties [Visual Basic], read-only
- read-only properties
- Get keyword [Visual Basic]
- property procedures [Visual Basic], Get statements
ms.assetid: 56b05cdc-bd64-4dfd-bb12-824eacec6f94
ms.openlocfilehash: 9560faf90d531c32f104dbd053a7c1f5584cfb1b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351173"
---
# <a name="get-statement"></a>Get ステートメント
プロパティの値を取得するために使用される `Get` プロパティプロシージャを宣言します。  
  
## <a name="syntax"></a>構文  
  
```vb  
[ <attributelist> ] [ accessmodifier ] Get()  
    [ statements ]  
End Get  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`attributelist`|省略可。 「[属性リスト](../../../visual-basic/language-reference/statements/attribute-list.md)」を参照してください。|  
|`accessmodifier`|このプロパティの `Get` および `Set` ステートメントのうちの 1 つで、省略可能です。 次のいずれかになります。<br /><br /> -     [Protected](../../../visual-basic/language-reference/modifiers/protected.md)<br />-   [Friend](../../../visual-basic/language-reference/modifiers/friend.md)<br />-   [Private](../../../visual-basic/language-reference/modifiers/private.md)<br />-   `Protected Friend`<br /><br /> 「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。|  
|`statements`|省略可。 `Get` プロパティプロシージャが呼び出されたときに実行される1つ以上のステートメント。|  
|`End Get`|必須。 `Get` property プロシージャの定義を終了します。|  
  
## <a name="remarks"></a>コメント  
 プロパティが `WriteOnly`としてマークされている場合を除き、すべてのプロパティには `Get` プロパティプロシージャが必要です。 `Get` プロシージャは、プロパティの現在の値を返すために使用されます。  
  
 Visual Basic は、プロパティの値を式が要求すると、プロパティの `Get` プロシージャを自動的に呼び出します。  
  
 プロパティ宣言の本体には、プロパティの `Get` と、[Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)と `End Property` ステートメントの間の `Set` プロシージャのみを含めることができます。 これらのプロシージャ以外を格納することはできません。 特に、プロパティの現在の値を格納することはできません。 この値はプロパティの外部に格納する必要があります。これは、プロパティプロシージャの内部に格納する場合、他のプロパティプロシージャがアクセスできないためです。 通常の方法では、プロパティと同じレベルで宣言された [Private](../../../visual-basic/language-reference/modifiers/private.md)変数に値を格納します。 `Get` プロシージャは、適用先のプロパティ内で定義する必要があります。  
  
 `Get` ステートメントで `accessmodifier` を使用しない限り、`Get` プロシージャの既定値は、それを含むプロパティのアクセスレベルです。  
  
## <a name="rules"></a>規則  
  
- **アクセス レベルの混在。** 読み取り/書き込みプロパティを定義する場合は、必要に応じて、`Get` または `Set` のいずれかのプロシージャに対して異なるアクセスレベルを指定できますが、両方を指定することはできません。 この場合、プロシージャのアクセスレベルは、プロパティのアクセスレベルよりも制限されている必要があります。 たとえば、プロパティが `Friend`として宣言されている場合は、`Get` プロシージャ `Private`を宣言できますが、`Public`は宣言できません。  
  
     `ReadOnly` プロパティを定義する場合、`Get` プロシージャはプロパティ全体を表します。 `Get`に対して異なるアクセスレベルを宣言することはできません。これは、プロパティに2つのアクセスレベルが設定されるためです。  
  
- **戻り値の型。** [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)では、返される値のデータ型を宣言できます。 `Get` プロシージャは、そのデータ型を自動的に返します。 任意のデータ型を指定することも、列挙型、構造体、クラス、またはインターフェイスの名前を指定することもできます。  
  
     `Property` ステートメントで `returntype`が指定されていない場合、プロシージャは `Object`を返します。  
  
## <a name="behavior"></a>動作  
  
- **プロシージャから戻る。** `Get` プロシージャが呼び出し元のコードに戻ると、プロパティ値を要求したステートメント内で実行が続行されます。  
  
     `Get` のプロパティプロシージャは、 [Return ステートメント](../../../visual-basic/language-reference/statements/return-statement.md)を使用するか、プロパティ名に戻り値を割り当てることによって、値を返すことができます。 詳細については、「[Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)」の「戻り値」を参照してください。  
  
     `Exit Property` ステートメントおよび `Return` ステートメントでは、プロパティプロシージャがすぐに終了します。 プロシージャ内の任意の場所で任意の数の `Exit Property` および `Return` ステートメントを使用できます。また、`Exit Property` と `Return` のステートメントを混在させることができます。  
  
- **戻り値。** `Get` プロシージャから値を返すには、プロパティ名に値を割り当てるか、 [Return ステートメント](../../../visual-basic/language-reference/statements/return-statement.md)に含めることができます。 `Return` ステートメントでは、`Get` プロシージャの戻り値が同時に割り当てられ、プロシージャが終了します。  
  
     プロパティ名に値を割り当てずに `Exit Property` を使用する場合、`Get` プロシージャは、プロパティのデータ型の既定値を返します。 詳細については、「[Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)」の「戻り値」を参照してください。  
  
     次の例は、読み取り専用プロパティ `quoteForTheDay` が、プライベート変数 `quoteValue`に保持されている値を返す方法を示しています。  
  
     [!code-vb[VbVbalrStatements#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#27)]  
  
     [!code-vb[VbVbalrStatements#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#28)]  
  
     [!code-vb[VbVbalrStatements#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#29)]  
  
## <a name="example"></a>例  
 次の例では、`Get` ステートメントを使用して、プロパティの値を返します。  
  
 [!code-vb[VbVbalrStatements#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#30)]  
  
## <a name="see-also"></a>参照

- [Set ステートメント](../../../visual-basic/language-reference/statements/set-statement.md)
- [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)
- [Exit ステートメント](../../../visual-basic/language-reference/statements/exit-statement.md)
- [クラスとオブジェクト](../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
- [チュートリアル : クラスの定義](../../../visual-basic/programming-guide/language-features/objects-and-classes/walkthrough-defining-classes.md)
