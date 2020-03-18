---
title: Do...Loop ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Do
- vb.Loop
- vb.Until
helpviewer_keywords:
- conditional statements [Visual Basic], Do�Loop
- while statement [Visual Basic], Do...Loop
- execution [Visual Basic], conditional
- Do loops
- Until keyword [Visual Basic], Do...Loop statement
- Do...Loop statement
- instructions, repeating
- Do statement [Visual Basic]
- Exit statement [Visual Basic], in Do...Loop statements
- loop structures [Visual Basic], Do�Loop statements
- do-while statements [Visual Basic]
- loops, exiting
- Loop keyword [Visual Basic], Do...Loop statement
ms.assetid: 892f9096-b3e2-4aee-834d-83bc4e2c379d
ms.openlocfilehash: 9384cbb355189be448fa4b8d274721b4a7ca6a20
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351257"
---
# <a name="doloop-statement-visual-basic"></a>Do...Loop ステートメント (Visual Basic)
`Boolean` 条件が `True` 場合、または条件が `True`になるまで、ステートメントのブロックを繰り返します。  
  
## <a name="syntax"></a>構文  
  
```vb  
Do { While | Until } condition  
    [ statements ]  
    [ Continue Do ]  
    [ statements ]  
    [ Exit Do ]  
    [ statements ]  
Loop  
' -or-  
Do  
    [ statements ]  
    [ Continue Do ]  
    [ statements ]  
    [ Exit Do ]  
    [ statements ]  
Loop { While | Until } condition  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`Do`|必須。 `Do` ループの定義を開始します。|  
|`While`|`Until` を使用しない場合に、必ず指定します。 `condition` が `False`になるまでループを繰り返します。|  
|`Until`|`While` を使用しない場合に、必ず指定します。 `condition` が `True`になるまでループを繰り返します。|  
|`condition`|省略可。 `Boolean` 式です。 `condition` が `Nothing`場合、Visual Basic はそれを `False`として扱います。|  
|`statements`|省略可。 1つまたは複数のステートメントが繰り返されている間、または `condition` が `True`されます。|  
|`Continue Do`|省略可。 `Do` ループの次の反復処理に制御を転送します。|  
|`Exit Do`|省略可。 `Do` ループから制御を転送します。|  
|`Loop`|必須。 `Do` ループの定義を終了します。|  
  
## <a name="remarks"></a>コメント  
 条件が満たされるまで、一連のステートメントを無限に繰り返す必要がある場合は、`Do...Loop` 構造体を使用します。 ステートメントを設定された回数繰り返し実行する場合は、[For...Next ステートメント](../../../visual-basic/language-reference/statements/for-next-statement.md) は通常、より適しています。  
  
 `While` または `Until` のいずれかを使用して `condition`を指定できますが、両方を指定することはできません。  
  
 ループの開始時または終了時に、`condition` テストできるのは1回だけです。 ループの開始時に (`Do` ステートメントで) `condition` をテストした場合、ループは一度も実行されない可能性があります。 ループの最後 (`Loop` ステートメント) でテストした場合、ループは常に少なくとも1回は実行されます。  
  
 通常、この条件は2つの値の比較によって得られますが、[Boolean データ型](../../../visual-basic/language-reference/data-types/boolean-data-type.md) の値 (`True` または `False`) に評価される任意の式を指定できます。 これには、`Boolean`に変換されたその他のデータ型 (数値型など) の値が含まれます。  
  
 ループを入れ子にするには、別のループ内にループを挿入し `Do` します。 また、さまざまな種類の制御構造を相互に入れ子にすることもできます。 詳細については、「[入れ子になった制御構造](../../../visual-basic/programming-guide/language-features/control-flow/nested-control-structures.md)」を参照してください。  
  
> [!NOTE]
> `Do...Loop` 構造では、より高い柔軟性が得[While...End While ステートメント](../../../visual-basic/language-reference/statements/while-end-while-statement.md) を使用すると、`condition` が `True` 停止したとき、または最初に `True`になったときにループを終了するかどうかを決定できます。 また、ループの開始時または終了時に `condition` をテストすることもできます。  
  
## <a name="exit-do"></a>終了  
 [Exit Do](../../../visual-basic/language-reference/statements/exit-statement.md)ステートメントを使用すると、別の方法で `Do…Loop`を終了できます。 `Exit Do` は、`Loop` ステートメントの直後のステートメントに制御を直ちに転送します。  
  
 `Exit Do` は、`If...Then...Else` 構造などの条件を評価した後に頻繁に使用されます。 誤った値や終了要求など、反復処理を続行することが不要または不可能な条件を検出した場合は、ループを終了することができます。 `Exit Do` の使用方法の1つは、*無限ループ*の原因となる可能性のある条件をテストすることです。これは、大規模または無限の回数実行されるループです。 `Exit Do` を使用すると、ループをエスケープできます。  
  
 `Do…Loop`内の任意の場所に、任意の数の `Exit Do` ステートメントを含めることができます。  
  
 入れ子になった `Do` ループ内で使用された場合、`Exit Do` は最も内側のループから次の上位レベルの入れ子に制御を転送します。  
  
## <a name="example"></a>例  
 次の例では、ループ内のステートメントは、`index` 変数が10を超えるまで実行を続けます。 `Until` 句がループの最後にあります。  
  
 [!code-vb[VbVbalrStatements#131](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#131)]  
  
## <a name="example"></a>例  
 次の例では、`Until` 句ではなく `While` 句を使用します。 `condition` は最後ではなくループの開始時にテストされます。  
  
 [!code-vb[VbVbalrStatements#132](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#132)]  
  
## <a name="example"></a>例  
 次の例では、`index` 変数が100より大きい場合、`condition` はループを停止します。 ただし、ループ内の `If` ステートメントでは、インデックス変数が10より大きい場合、`Exit Do` ステートメントによってループが停止されます。  
  
 [!code-vb[VbVbalrStatements#133](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#133)]  
  
## <a name="example"></a>例  
 次の例では、テキストファイル内のすべての行を読み取ります。 <xref:System.IO.File.OpenText%2A> メソッドは、ファイルを開き、文字を読み取る <xref:System.IO.StreamReader> を返します。 `Do...Loop` 条件では、`StreamReader` の <xref:System.IO.StreamReader.Peek%2A> メソッドによって、追加の文字があるかどうかが判断されます。  
  
 [!code-vb[VbVbalrStatements#134](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#134)]  
  
## <a name="see-also"></a>参照

- [ループ構造](../../../visual-basic/programming-guide/language-features/control-flow/loop-structures.md)
- [For...Next ステートメント](../../../visual-basic/language-reference/statements/for-next-statement.md)
- [Boolean データ型](../../../visual-basic/language-reference/data-types/boolean-data-type.md)
- [入れ子になった制御構造](../../../visual-basic/programming-guide/language-features/control-flow/nested-control-structures.md)
- [Exit ステートメント](../../../visual-basic/language-reference/statements/exit-statement.md)
- [While...End While ステートメント](../../../visual-basic/language-reference/statements/while-end-while-statement.md)
