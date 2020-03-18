---
title: Exit ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Exit
helpviewer_keywords:
- execution [Visual Basic], ending
- files [Visual Basic], closing
- programs [Visual Basic], quitting
- code, exiting
- Exit statement [Visual Basic]
- program termination
- execution [Visual Basic], stopping
ms.assetid: 760bfb32-5c3f-4bdb-a432-9a6001c92db7
ms.openlocfilehash: 1bfe81428fd3c50663fd8978e05c6a945cd47df8
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345937"
---
# <a name="exit-statement-visual-basic"></a>Exit ステートメント (Visual Basic)

プロシージャまたはブロックを終了し、プロシージャ呼び出しまたはブロック定義に続くステートメントに制御を直ちに転送します。

## <a name="syntax"></a>構文

```vb
Exit { Do | For | Function | Property | Select | Sub | Try | While }
```

## <a name="statements"></a>ステートメント

 `Exit Do`  
 が表示されている `Do` ループを直ちに終了します。 実行は、`Loop` ステートメントに続くステートメントを使用して続行されます。 `Exit Do` は、`Do` ループ内でのみ使用できます。 入れ子になった `Do` ループ内で使用すると、`Exit Do` 最も内側のループを終了し、次に高い入れ子レベルに制御を転送します。

 `Exit For`  
 が表示されている `For` ループを直ちに終了します。 実行は、`Next` ステートメントに続くステートメントを使用して続行されます。 `Exit For` は、`For`...`Next` または `For Each`...`Next` ループ内でのみ使用できます。 入れ子になった `For` ループ内で使用すると、`Exit For` 最も内側のループを終了し、次に高い入れ子レベルに制御を転送します。

 `Exit Function`  
 が表示されている `Function` プロシージャをすぐに終了します。 実行は、`Function` プロシージャを呼び出したステートメントの後に続くステートメントから続行されます。 `Exit Function` は、`Function` プロシージャ内でのみ使用できます。

 戻り値を指定するには、`Exit Function` ステートメントの前にある行の関数名に値を代入します。 戻り値を割り当てて、1つのステートメントで関数を終了するには、代わりに[Return ステートメント](return-statement.md)を使用できます。

 `Exit Property`  
 が表示されている `Property` プロシージャをすぐに終了します。 実行は、`Property` プロシージャを呼び出したステートメント、つまり、プロパティの値を要求または設定するステートメントによって続行されます。 `Exit Property` は、プロパティの `Get` または `Set` プロシージャ内でのみ使用できます。

 `Get` プロシージャで戻り値を指定するには、`Exit Property` ステートメントの前にある行の関数名に値を割り当てることができます。 1つのステートメントで戻り値を割り当てて `Get` プロシージャを終了するには、代わりに `Return` ステートメントを使用できます。

 `Set` のプロシージャでは、`Exit Property` ステートメントは `Return` ステートメントと同じです。

 `Exit Select`  
 が表示されている `Select Case` ブロックを直ちに終了します。 実行は、`End Select` ステートメントに続くステートメントを使用して続行されます。 `Exit Select` は、`Select Case` ステートメント内でのみ使用できます。

 `Exit Sub`  
 が表示されている `Sub` プロシージャをすぐに終了します。 実行は、`Sub` プロシージャを呼び出したステートメントの後に続くステートメントから続行されます。 `Exit Sub` は、`Sub` プロシージャ内でのみ使用できます。

 `Sub` のプロシージャでは、`Exit Sub` ステートメントは `Return` ステートメントと同じです。

 `Exit Try`  
 が表示されている `Try` または `Catch` ブロックを直ちに終了します。 実行は、存在する場合は `Finally` ブロックで続行されます。それ以外の場合は `End Try` ステートメントの後のステートメントで続行されます。 `Exit Try` は、`Finally` ブロック内ではなく、`Try` または `Catch` ブロック内でのみ使用できます。

 `Exit While`  
 が表示されている `While` ループを直ちに終了します。 実行は、`End While` ステートメントに続くステートメントを使用して続行されます。 `Exit While` は、`While` ループ内でのみ使用できます。 入れ子になった `While` ループ内で使用される場合、`Exit While` は `Exit While` が発生するループの上に入れ子になった1つのレベルであるループに制御を転送します。

## <a name="remarks"></a>コメント

`Exit` ステートメントを `End` ステートメントと混同しないでください。 `Exit` では、ステートメントの末尾が定義されていません。

## <a name="example"></a>例

次の例では、`index` 変数が100より大きい場合、ループ条件によってループが停止します。 ただし、ループ内の `If` ステートメントでは、インデックス変数が10より大きい場合、`Exit Do` ステートメントによってループが停止されます。

[!code-vb[VbVbalrStatements#133](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#133)]

## <a name="example"></a>例

次の例では、関数名 `myFunction`に戻り値を代入し、`Exit Function` を使用して関数からを返します。

[!code-vb[VbVbalrStatements#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#23)]

## <a name="example"></a>例

次の例では、 [Return ステートメント](return-statement.md)を使用して戻り値を割り当て、関数を終了します。

[!code-vb[VbVbalrStatements#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#24)]

## <a name="see-also"></a>関連項目

- [Continue ステートメント](continue-statement.md)
- [Do...Loop ステートメント](do-loop-statement.md)
- [End ステートメント](end-statement.md)
- [For Each...Next ステートメント](for-each-next-statement.md)
- [For...Next ステートメント](for-next-statement.md)
- [Function ステートメント](function-statement.md)
- [Return ステートメント](return-statement.md)
- [Stop ステートメント](stop-statement.md)
- [Sub ステートメント](sub-statement.md)
- [Try...Catch...Finally ステートメント](try-catch-finally-statement.md)
