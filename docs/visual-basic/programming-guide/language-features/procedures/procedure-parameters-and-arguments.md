---
title: プロシージャのパラメーターと引数
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedures [Visual Basic], argument lists
- values [Visual Basic], passing to procedures
- arguments [Visual Basic], passing
- procedures [Visual Basic], parameters
- Visual Basic code, argument lists
- Visual Basic code, procedures
- parameters [Visual Basic], Visual Basic procedures
- parameters [Visual Basic], lists
- arguments [Visual Basic], Visual Basic procedures
- arguments [Visual Basic], procedures
- parameter lists [Visual Basic]
- Visual Basic code, parameter lists
- argument lists [Visual Basic]
- procedures [Visual Basic], parameter lists
ms.assetid: ff275aff-aa13-40df-bd4c-63486db8c1e9
ms.openlocfilehash: 7dfbbcb39cf7bb05c8a62a7a252e425f287c9a09
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352580"
---
# <a name="procedure-parameters-and-arguments-visual-basic"></a>プロシージャのパラメーターと引数 (Visual Basic)
ほとんどの場合、プロシージャは、呼び出された状況に関する情報を必要とします。 繰り返しまたは共有タスクを実行するプロシージャは、呼び出しごとに異なる情報を使用します。 この情報は、呼び出し時にプロシージャに渡す変数、定数、および式で構成されます。  
  
 *パラメーター*は、プロシージャが呼び出し時に指定する必要がある値を表します。 プロシージャの宣言では、パラメーターを定義します。  
  
 パラメーターのないプロシージャ、1つのパラメーター、または複数のプロシージャを定義できます。 パラメーターを指定するプロシージャ定義の一部は、*パラメーターリスト*と呼ばれます。  
  
 *引数*は、プロシージャを呼び出すときにプロシージャパラメーターに指定する値を表します。 呼び出し元のコードは、プロシージャを呼び出すときに引数を指定します。 引数を指定するプロシージャ呼び出しの部分は、*引数リスト*と呼ばれます。  
  
 次の図は、2つの異なる場所から `safeSquareRoot` プロシージャを呼び出すコードを示しています。 最初の呼び出しでは、変数 `x` (4.0) の値が `number`パラメーターに渡され、`root` (2.0) の戻り値が変数 `y`に代入されます。 2番目の呼び出しは、リテラル値9.0 を `number`に渡し、戻り値 (3.0) を変数 `z`に代入します。  
  
 ![引数をパラメーターに渡す方法を示す図](./media/procedure-parameters-and-arguments/pass-argument-parameter.gif)  
  
 詳細については、「[パラメーターと引数の違い](./differences-between-parameters-and-arguments.md)」を参照してください。  
  
## <a name="parameter-data-type"></a>パラメーターのデータ型  
 パラメーターのデータ型を定義するには、その宣言で `As` 句を使用します。 たとえば、次の関数は、文字列と整数を受け取ります。  
  
 [!code-vb[VbVbcnProcedures#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#32)]  
  
 型チェックスイッチ ([Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)) が `Off,` 場合、`As` 句は省略可能です。ただし、いずれか1つのパラメーターで使用する場合は、すべてのパラメーターで使用する必要があります。 型チェックが `On`場合は、すべてのプロシージャパラメーターに `As` 句が必要です。  
  
 呼び出し元のコードが、対応するパラメーターとは異なるデータ型の引数を指定することを想定している場合 (`String` パラメーターへの `Byte` など)、次のいずれかの操作を行う必要があります。  
  
- パラメーターのデータ型に拡大変換されるデータ型の引数のみを指定します。  
  
- `Option Strict Off` を設定して、暗黙的な縮小変換を許可します。もしくは  
  
- 変換キーワードを使用して、データ型を明示的に変換します。  
  
### <a name="type-parameters"></a>型パラメーター  
 *ジェネリックプロシージャ*は、通常のパラメーターに加えて、1 つまたは複数の*型パラメーター*も定義します。 ジェネリックプロシージャを使用すると、呼び出し元のコードはプロシージャを呼び出すたびに異なるデータ型を渡すことができるので、データ型を個々の呼び出しの要件に合わせて調整できます。「[Visual Basic におけるジェネリック プロシージャ](../../../../visual-basic/programming-guide/language-features/data-types/generic-procedures.md)」を参照してください。 

  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [Sub プロシージャ](./sub-procedures.md)
- [Function プロシージャ](./function-procedures.md)
- [Property プロシージャ](./property-procedures.md)
- [演算子プロシージャ](./operator-procedures.md)
- [方法 : プロシージャにパラメーターを定義する](./how-to-define-a-parameter-for-a-procedure.md)
- [方法: プロシージャに引数を渡す](./how-to-pass-arguments-to-a-procedure.md)
- [引数の値渡しと参照渡し](./passing-arguments-by-value-and-by-reference.md)
- [プロシージャのオーバーロード](./procedure-overloading.md)
- [Visual Basic での型変換](../../../../visual-basic/programming-guide/language-features/data-types/type-conversions.md)
