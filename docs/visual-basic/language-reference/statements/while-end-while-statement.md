---
title: While...End While ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.While
- vb.While...EndWhile
helpviewer_keywords:
- While statement [Visual Basic], While...End While
- While statement [Visual Basic]
- While...End While statements [Visual Basic]
ms.assetid: b931d1ce-e8ed-44d8-a13d-92a4f5458a1e
ms.openlocfilehash: 87f6fbd6147b6dbfbe08c93e862d58b9868f9201
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352753"
---
# <a name="whileend-while-statement-visual-basic"></a>While...End While ステートメント (Visual Basic)
指定された条件が `True`場合に限り、一連のステートメントを実行します。  
  
## <a name="syntax"></a>構文  
  
```vb  
While condition  
    [ statements ]  
    [ Continue While ]  
    [ statements ]  
    [ Exit While ]  
    [ statements ]  
End While  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`condition`|必須。 `Boolean` 式です。 `condition` が `Nothing`場合、Visual Basic はそれを `False`として扱います。|  
|`statements`|省略可。 `While`の後に1つ以上のステートメントがあり、`condition` が `True`たびに実行されます。|  
|`Continue While`|省略可。 `While` ブロックの次の反復処理に制御を転送します。|  
|`Exit While`|省略可。 `While` ブロックの外に制御を転送します。|  
|`End While`|必須。 `While` ブロックの定義を終了します。|  
  
## <a name="remarks"></a>コメント  
 条件が `True` のままである限り、一連のステートメントを無期限に繰り返す場合は、`While...End While` 構造体を使用します。 条件またはテスト対象の結果をテストする場所を柔軟に指定できるようにするには、[Do...Loop ステートメント](../../../visual-basic/language-reference/statements/do-loop-statement.md)を選択します。 ステートメントを設定された回数繰り返し実行する場合、通常は[For...Next ステートメント](../../../visual-basic/language-reference/statements/for-next-statement.md)がより適しています。  
  
> [!NOTE]
> `While` キーワードは、 [Do...Loop ステートメント](../../../visual-basic/language-reference/statements/do-loop-statement.md)、 [Skip while 句](../../../visual-basic/language-reference/queries/skip-while-clause.md)、 [Take while 句](../../../visual-basic/language-reference/queries/take-while-clause.md)。  
  
 `condition` が `True`場合、`End While` ステートメントが検出されるまで、すべての `statements` が実行されます。 制御が `While` ステートメントに戻り、`condition` が再度オンになります。 `condition` がまだ `True`場合は、プロセスが繰り返されます。 `False`の場合、制御は、`End While` ステートメントの後のステートメントに渡されます。  
  
 `While` ステートメントは、ループを開始する前に常に条件をチェックします。 ループは、条件が `True`のまま続行されます。 最初にループを入力したときに `condition` が `False` 場合は、一度も実行されません。  
  
 通常、`condition` は2つの値を比較した結果になりますが、[Boolean データ型](../../../visual-basic/language-reference/data-types/boolean-data-type.md) の値 (`True` または `False`) に評価される任意の式を指定できます。 この式には、`Boolean`に変換された別のデータ型 (数値型など) の値を含めることができます。  
  
 ループを `While` 入れ子にするには、別のループ内に1つのループを配置します。 また、さまざまな種類の制御構造を相互に入れ子にすることもできます。 詳細については、「[入れ子になった制御構造](../../../visual-basic/programming-guide/language-features/control-flow/nested-control-structures.md)」を参照してください。  
  
## <a name="exit-while"></a>Exit While  
 [Exit While](../../../visual-basic/language-reference/statements/exit-statement.md)ステートメントを使用すると、`While` ループを終了する別の方法を指定できます。 `Exit While` は、`End While` ステートメントの後にあるステートメントに制御を直ちに転送します。  
  
 通常は、何らかの条件が評価された後 (たとえば、`If...Then...Else` 構造) に `Exit While` を使用します。 誤った値や終了要求など、反復処理を続行することが不要または不可能な条件を検出した場合は、ループを終了することができます。 *無限ループ*の原因となる可能性のある条件をテストするときに、`Exit While` を使用できます。これは、非常に大きいまたは無限の回数実行されるループです。 その後、`Exit While` を使用してループをエスケープできます。  
  
 `While` ループ内の任意の場所に、任意の数の `Exit While` ステートメントを配置できます。  
  
 入れ子になった `While` ループ内で使用された場合、`Exit While` は最も内側のループから次の上位レベルの入れ子に制御を転送します。  
  
 `Continue While` ステートメントは、ループの次の反復処理に制御を直ちに転送します。 詳細については、「[Continue ステートメント](../../../visual-basic/language-reference/statements/continue-statement.md)」を参照してください。  
  
## <a name="example"></a>例  
 次の例では、ループ内のステートメントは、`index` 変数が10を超えるまで実行を続けます。  
  
 [!code-vb[VbVbalrStatements#171](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class14.vb#171)]  
  
## <a name="example"></a>例  
 次の例は、`Continue While` と `Exit While` ステートメントの使用方法を示しています。  
  
 [!code-vb[VbVbalrStatements#172](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class14.vb#172)]  
  
## <a name="example"></a>例  
 次の例では、テキストファイル内のすべての行を読み取ります。 <xref:System.IO.File.OpenText%2A> メソッドは、ファイルを開き、文字を読み取る <xref:System.IO.StreamReader> を返します。 `While` 条件では、`StreamReader` の <xref:System.IO.StreamReader.Peek%2A> メソッドによって、ファイルに追加の文字が含まれているかどうかが判断されます。  
  
 [!code-vb[VbVbalrStatements#173](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class14.vb#173)]  
  
## <a name="see-also"></a>参照

- [ループ構造](../../../visual-basic/programming-guide/language-features/control-flow/loop-structures.md)
- [Do...Loop ステートメント](../../../visual-basic/language-reference/statements/do-loop-statement.md)
- [For...Next ステートメント](../../../visual-basic/language-reference/statements/for-next-statement.md)
- [Boolean データ型](../../../visual-basic/language-reference/data-types/boolean-data-type.md)
- [入れ子になった制御構造](../../../visual-basic/programming-guide/language-features/control-flow/nested-control-structures.md)
- [Exit ステートメント](../../../visual-basic/language-reference/statements/exit-statement.md)
- [Continue ステートメント](../../../visual-basic/language-reference/statements/continue-statement.md)
