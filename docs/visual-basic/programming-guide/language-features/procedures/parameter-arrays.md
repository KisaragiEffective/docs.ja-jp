---
title: パラメーター配列
ms.date: 07/20/2015
helpviewer_keywords:
- parameter arrays [Visual Basic], about parameter arrays
- ParamArray keyword [Visual Basic], parameter arrays
- Visual Basic code, procedures
- parameters [Visual Basic], parameter arrays
- arguments [Visual Basic], parameter arrays
- procedures [Visual Basic], indefinite number of argument values
- arrays [Visual Basic], parameter arrays
ms.assetid: c43edfae-9114-4096-9ebc-8c5c957a1067
ms.openlocfilehash: ffb532fbac70b9aa8ab210450e4d9207f5e0291f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351124"
---
# <a name="parameter-arrays-visual-basic"></a>パラメーター配列 (Visual Basic)
通常、プロシージャの宣言よりも多くの引数を指定してプロシージャを呼び出すことはできません。 不特定数の引数が必要な場合は、パラメーター*配列*を宣言して、プロシージャがパラメーターの値の配列を受け取ることができるようにすることができます。 プロシージャを定義するときに、パラメーター配列内の要素の数を知る必要はありません。 配列のサイズは、プロシージャの呼び出しごとに個別に決定されます。  
  
## <a name="declaring-a-paramarray"></a>ParamArray の宣言  
 パラメーターリストのパラメーター配列を示すには、 [ParamArray](../../../../visual-basic/language-reference/modifiers/paramarray.md)キーワードを使用します。 次の規則が適用されます。  
  
- プロシージャは、パラメーター配列を1つだけ定義できます。また、プロシージャ定義の最後のパラメーターである必要があります。  
  
- パラメーター配列は、値で渡す必要があります。 プロシージャの定義に[ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)キーワードを明示的に含めることをお勧めします。  
  
- パラメーター配列は、自動的に省略可能です。 既定値は、パラメーター配列の要素型の空の1次元配列です。  
  
- パラメーター配列の前にあるすべてのパラメーターが必要です。 パラメーター配列は、唯一の省略可能なパラメーターである必要があります。  
  
## <a name="calling-a-paramarray"></a>ParamArray の呼び出し  
 パラメーター配列を定義するプロシージャを呼び出す場合は、次のいずれかの方法で引数を指定できます。  
  
- Nothing: つまり、 [ParamArray](../../../../visual-basic/language-reference/modifiers/paramarray.md)引数を省略できます。 この場合、空の配列がプロシージャに渡されます。 [Nothing](../../../../visual-basic/language-reference/nothing.md)キーワードを明示的に渡すと、null 配列がプロシージャに渡され、呼び出されたプロシージャがこの条件をチェックしない場合、NullReferenceException が発生する可能性があります。
  
- コンマで区切られた任意の数の引数のリスト。 各引数のデータ型は、`ParamArray` 要素型に暗黙的に変換できる必要があります。  
  
- パラメーター配列の要素型と同じ要素型を持つ配列。  
  
 どのような場合でも、プロシージャ内のコードは、パラメーター配列を `ParamArray` データ型と同じデータ型の要素を持つ1次元配列として扱います。  
  
> [!IMPORTANT]
> 無限に大きくなる可能性がある配列を処理する場合、アプリケーションの内部容量がオーバーランするリスクがあります。 パラメーター配列を受け入れる場合は、呼び出し元のコードが配列のサイズを渡すかどうかをテストする必要があります。 アプリケーションに対して大きすぎる場合は、適切な手順を実行してください。 詳細については、「[配列](../../../../visual-basic/programming-guide/language-features/arrays/index.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、関数 `calcSum`を定義して呼び出します。 パラメーター `args` の `ParamArray` 修飾子は、関数が可変個の引数を受け取ることができるようにします。  
  
 [!code-vb[VbVbalrStatements#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#26)]  
  
 次の例では、パラメーター配列を使用してプロシージャを定義し、パラメーター配列に渡されたすべての配列要素の値を出力します。  
  
 [!code-vb[VbVbcnProcedures#48](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#48)]  
  
 [!code-vb[VbVbcnProcedures#49](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#49)]  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Information.UBound%2A>
- [Visual Basic におけるプロシージャ](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [引数の値渡しと参照渡し](./passing-arguments-by-value-and-by-reference.md)
- [位置と名前による引数渡し](./passing-arguments-by-position-and-by-name.md)
- [省略可能なパラメーター](./optional-parameters.md)
- [プロシージャのオーバーロード](./procedure-overloading.md)
- [配列](../../../../visual-basic/programming-guide/language-features/arrays/index.md)
- [Optional](../../../../visual-basic/language-reference/modifiers/optional.md)
