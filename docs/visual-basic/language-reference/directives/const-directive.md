---
title: '#Const ディレクティブ'
ms.date: 07/20/2015
f1_keywords:
- vb.#Const
- '#vb.Const'
- '#Const'
helpviewer_keywords:
- '#Const directive'
- conditional compilation [Visual Basic], directives
- Const directive (#Const)
- Visual Basic compiler, compiler directives
- constants [Visual Basic], Const directive
- constants [Visual Basic], declaring
- Const statement [Visual Basic], directive (#Const)
- 'declaring constants [Visual Basic], #const directive'
ms.assetid: 707669e5-23f9-4f17-8622-a0d534429386
ms.openlocfilehash: 278219edb1bb5d1c0bb015611d69cbe4ae70014b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343845"
---
# <a name="const-directive"></a>#Const ディレクティブ

Visual Basic の条件付きコンパイラ定数を定義します。  
  
## <a name="syntax"></a>構文  
  
```vb  
#Const constname = expression  
```  
  
## <a name="parts"></a>指定項目  

 `constname`  
 必須。 定義されている定数の名前。  
  
 `expression`  
 必須。 リテラル、その他の条件付きコンパイラ定数、または `Is`を除く任意またはすべての算術演算子または論理演算子を含む任意の組み合わせ。  
  
## <a name="remarks"></a>コメント  

 条件付きコンパイラ定数は、それらが表示されるファイルに対して常にプライベートです。 `#Const` ディレクティブを使用して、パブリックコンパイラ定数を作成することはできません。これらは、ユーザーインターフェイスまたは `/define` コンパイラオプションでのみ作成できます。  
  
 `expression`では、条件付きコンパイラ定数とリテラルのみを使用できます。 `Const` で定義された標準定数を使用すると、エラーが発生します。 逆に、`#Const` キーワードで定義された定数は、条件付きコンパイルに対してのみ使用できます。 定数を未定義にすることもできます。その場合、値は `Nothing`になります。  
  
## <a name="example"></a>例  

 `#Const` ディレクティブの使用例を次に示します。  
  
 [!code-vb[VbVbalrConditionalComp#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConditionalComp/VB/Class1.vb#3)]  
  
## <a name="see-also"></a>関連項目

- [-define (Visual Basic)](../../../visual-basic/reference/command-line-compiler/define.md)
- [#If...Then...#Else ディレクティブ](../../../visual-basic/language-reference/directives/if-then-else-directives.md)
- [Const ステートメント](../../../visual-basic/language-reference/statements/const-statement.md)
- [条件付きコンパイル](../../../visual-basic/programming-guide/program-structure/conditional-compilation.md)
- [If...Then...Else ステートメント](../../../visual-basic/language-reference/statements/if-then-else-statement.md)
