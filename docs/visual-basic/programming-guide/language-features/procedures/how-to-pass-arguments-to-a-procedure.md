---
title: '方法: プロシージャに引数を渡す'
ms.date: 07/20/2015
helpviewer_keywords:
- arguments [Visual Basic], passing to procedures
- procedures [Visual Basic], arguments
- procedures [Visual Basic], parameters
- procedure arguments
- Visual Basic code, procedures
- procedure parameters
- procedures [Visual Basic], calling
- argument passing [Visual Basic], procedures
ms.assetid: 08723588-3890-4ddc-8249-79e049e0f241
ms.openlocfilehash: 0267eed7c145988d61de715fd661bd4906d8d57d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344844"
---
# <a name="how-to-pass-arguments-to-a-procedure-visual-basic"></a>方法: プロシージャに引数を渡す (Visual Basic)
プロシージャを呼び出すときは、かっこで囲んだ引数リストを使用してプロシージャ名を指定します。 プロシージャで定義されているすべての必須パラメーターに対応する引数を指定します。また、必要に応じて `Optional` パラメーターに引数を指定することもできます。 呼び出しに `Optional` パラメーターを指定しない場合は、後続の引数を指定する場合に、引数リスト内でその位置をマークするためにコンマを含める必要があります。  
  
 `Byte` など、対応するパラメーターとは異なるデータ型の引数を `String`に渡す場合は、型チェックスイッチ ([Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)) を `Off`に設定できます。 `Option Strict` が `On`場合は、拡大変換または明示的な変換キーワードのいずれかを使用する必要があります。 詳細については、「[拡大および縮小変換](../../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)」と「[型変換関数](../../../../visual-basic/language-reference/functions/type-conversion-functions.md)」を参照してください。  
  
 詳細については、「[プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)」を参照してください。  
  
### <a name="to-pass-one-or-more-arguments-to-a-procedure"></a>プロシージャに1つ以上の引数を渡すには  
  
1. 呼び出し元のステートメントで、プロシージャ名の後にかっこを付けます。  
  
2. かっこ内に引数リストを入力します。 プロシージャが定義する必須パラメーターごとに引数を含め、引数をコンマで区切ります。  
  
3. 各引数が、対応するパラメーターに対して定義されている型に変換できるデータ型に評価される有効な式であることを確認します。  
  
4. パラメーターが [Optional](../../../../visual-basic/language-reference/modifiers/optional.md) として定義されている場合は、引数リストに含めるか、または省略することができます。 省略した場合、プロシージャは、そのパラメーターに対して定義されている既定値を使用します。  
  
5. `Optional` パラメーターの引数を省略していて、パラメーターリスト内に別のパラメーターがある場合は、引数リストで省略された引数の代わりにコンマを追加してマークできます。  
  
     次の例では、Visual Basic <xref:Microsoft.VisualBasic.Interaction.MsgBox%2A> 関数を呼び出します。  
  
     [!code-vb[VbVbcnProcedures#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#34)]  
  
     前の例では、必要な最初の引数が指定されています。これは、表示されるメッセージ文字列です。 省略可能な2番目のパラメーターの引数を省略して、メッセージボックスに表示するボタンを指定します。 呼び出しで値が指定されていないため、`MsgBox` は既定値の `MsgBoxStyle.OKOnly`を使用し、 **[OK** ] ボタンのみを表示します。  
  
     引数リストの2番目のコンマは省略された2番目の引数の位置をマークし、最後の文字列は `MsgBox`の3番目のパラメーター (タイトルバーに表示されるテキスト) に渡されます。  
  
## <a name="see-also"></a>参照

- [Sub プロシージャ](./sub-procedures.md)
- [Function プロシージャ](./function-procedures.md)
- [Property プロシージャ](./property-procedures.md)
- [演算子プロシージャ](./operator-procedures.md)
- [方法 : プロシージャにパラメーターを定義する](./how-to-define-a-parameter-for-a-procedure.md)
- [引数の値渡しと参照渡し](./passing-arguments-by-value-and-by-reference.md)
- [再帰プロシージャ](./recursive-procedures.md)
- [プロシージャのオーバーロード](./procedure-overloading.md)
- [クラスとオブジェクト](../../../../visual-basic/programming-guide/language-features/objects-and-classes/index.md)
- [オブジェクト指向プログラミング (Visual Basic)](../../concepts/object-oriented-programming.md)
