---
title: 厳密でないデリゲート変換
ms.date: 07/20/2015
helpviewer_keywords:
- relaxed delegate conversion [Visual Basic]
- delegates [Visual Basic], relaxed conversion
- conversions [Visual Basic], relaxed delegate
ms.assetid: 64f371d0-5416-4f65-b23b-adcbf556e81c
ms.openlocfilehash: ffb242842553382ba26121333c38fc65eaa168a9
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345219"
---
# <a name="relaxed-delegate-conversion-visual-basic"></a>厳密でないデリゲート変換 (Visual Basic)
厳密でないデリゲート変換を使用すると、シグネチャが同一でない場合でも、デリゲートまたはハンドラーにサブルーチンおよび関数を割り当てることができます。 したがって、デリゲートへのバインドは、メソッドの呼び出しに対して既に許可されているバインディングと一致します。  
  
## <a name="parameters-and-return-type"></a>パラメーターと戻り値の型  
 厳密なシグネチャ一致の代わりに、`Option Strict` が `On`に設定されている場合は、次の条件を満たす必要があります。  
  
- 拡大変換は、各デリゲートパラメーターのデータ型から、割り当てられた関数または `Sub`の対応するパラメーターのデータ型に存在する必要があります。 次の例では、デリゲート `Del1` に1つのパラメーター (`Integer`) があります。 割り当てられたラムダ式のパラメーター `m` には、`Long` や `Double`などの `Integer`からの拡大変換があるデータ型が含まれている必要があります。  
  
     [!code-vb[VbVbalrRelaxedDelegates#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#1)]  
  
     [!code-vb[VbVbalrRelaxedDelegates#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#2)]  
  
     縮小変換は、`Option Strict` が `Off`に設定されている場合にのみ許可されます。  
  
     [!code-vb[VbVbalrRelaxedDelegates#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module2.vb#8)]  
  
- 拡大変換は、割り当てられた関数の戻り値の型からの逆方向に存在するか、デリゲートの戻り値の型に `Sub` 必要があります。 次の例では、割り当てられている各ラムダ式の本体は、`del1` の戻り値の型が `Integer`であるため、`Integer` に拡大変換されるデータ型に評価される必要があります。  
  
     [!code-vb[VbVbalrRelaxedDelegates#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#3)]  
  
 `Option Strict` が `Off`に設定されている場合、拡大制限は両方向に削除されます。  
  
 [!code-vb[VbVbalrRelaxedDelegates#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module2.vb#4)]  
  
## <a name="omitting-parameter-specifications"></a>パラメーターの指定を省略する  
 緩やかなデリゲートでは、割り当てられたメソッドでパラメーターの指定を完全に省略することもできます。  
  
 [!code-vb[VbVbalrRelaxedDelegates#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#5)]  
  
 [!code-vb[VbVbalrRelaxedDelegates#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#6)]  
  
 一部のパラメーターを指定したり、他のパラメーターを省略したりすることはできません。  
  
 [!code-vb[VbVbalrRelaxedDelegates#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#15)]  
  
 パラメーターを省略する機能は、複数の複雑なパラメーターが関係するイベントハンドラーを定義するなどの状況で役立ちます。 一部のイベントハンドラーへの引数は使用されません。 代わりに、ハンドラーは、イベントが登録されているコントロールの状態に直接アクセスし、引数を無視します。 厳密でないデリゲートを使用すると、あいまいさの結果がない場合に、このような宣言の引数を省略できます。 次の例では、完全に指定されたメソッド `OnClick` を `RelaxedOnClick`として書き直すことができます。  
  
```vb  
Sub OnClick(ByVal sender As Object, ByVal e As EventArgs) Handles b.Click  
    MessageBox.Show("Hello World from" + b.Text)  
End Sub  
  
Sub RelaxedOnClick() Handles b.Click  
    MessageBox.Show("Hello World from" + b.Text)  
End Sub  
```  
  
## <a name="addressof-examples"></a>AddressOf の例  
 前の例では、ラムダ式を使用して、型の関係を簡単に確認できます。 ただし、`AddressOf`、`Handles`、または `AddHandler`を使用するデリゲートの割り当てに対しても、同じリラクゼーションが許可されます。  
  
 次の例では、関数 `f1`、`f2`、`f3`、および `f4` をすべて `Del1`に割り当てることができます。  
  
 [!code-vb[VbVbalrRelaxedDelegates#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#1)]  
  
 [!code-vb[VbVbalrRelaxedDelegates#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#7)]  
  
 [!code-vb[VbVbalrRelaxedDelegates#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#9)]  
  
 次の例は、`Option Strict` が `Off`に設定されている場合にのみ有効です。  
  
 [!code-vb[VbVbalrRelaxedDelegates#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module2.vb#14)]  
  
## <a name="dropping-function-returns"></a>関数の戻り値の削除  
 厳密でないデリゲート変換を使用すると、関数を `Sub` デリゲートに割り当てて、関数の戻り値を効果的に無視できます。 ただし、`Sub` を関数デリゲートに割り当てることはできません。 次の例では、関数 `doubler` のアドレスが `Sub` デリゲート `Del3`に割り当てられています。  
  
 [!code-vb[VbVbalrRelaxedDelegates#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#10)]  
  
 [!code-vb[VbVbalrRelaxedDelegates#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#11)]  
  
## <a name="see-also"></a>参照

- [ラムダ式](../../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)
- [拡大変換と縮小変換](../../../../visual-basic/programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [デリゲート](../../../../visual-basic/programming-guide/language-features/delegates/index.md)
- 方法 : [Visual Basic でプロシージャを別のプロシージャに渡す](../../../../visual-basic/programming-guide/language-features/delegates/how-to-pass-procedures-to-another-procedure.md)
- [ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
- [Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)
