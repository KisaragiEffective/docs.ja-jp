---
title: Resume ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Resume
- vb.ResumeNext
helpviewer_keywords:
- Next statement [Visual Basic], Resume
- Resume Next statement [Visual Basic]
- execution [Visual Basic], resuming
- run-time errors [Visual Basic], resuming after
- Resume statement [Visual Basic], syntax
- errors [Visual Basic], resuming after
- Error statement [Visual Basic], and Resume statement
- execution
- Resume statement [Visual Basic]
ms.assetid: e24d058b-1a5c-4274-acb9-7d295d3ea537
ms.openlocfilehash: 95137a9f6a4a4a18655b51b95300bfaf93cca193
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74333029"
---
# <a name="resume-statement"></a>Resume ステートメント
エラー処理ルーチンが終了した後、実行を再開します。  
  
 構造化例外処理は、構造化されていない例外処理、`On Error` および `Resume` ステートメントを使用するのではなく、可能な限りコードで使用することをお勧めします。 詳しくは、「[Try...Catch...Finally ステートメント](../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)」をご覧ください。  
  
## <a name="syntax"></a>構文  
  
```vb  
Resume [ Next | line ]  
```  
  
## <a name="parts"></a>指定項目  
 `Resume`  
 必須。 エラーがエラーハンドラーと同じ手順で発生した場合は、エラーの原因となったステートメントを使用して実行が再開されます。 呼び出されたプロシージャでエラーが発生した場合、エラー処理ルーチンを含むプロシージャから最後に呼び出されたステートメントで実行が再開されます。  
  
 `Next`  
 省略可。 エラーハンドラーと同じプロシージャでエラーが発生した場合は、エラーの原因となったステートメントの直後のステートメントを使用して実行が再開されます。 呼び出されたプロシージャでエラーが発生した場合、エラー処理ルーチン (または `On Error Resume Next` ステートメント) を含むプロシージャから最後に呼び出されたステートメントの直後にあるステートメントを使用して、実行が再開されます。  
  
 `line`  
 省略可。 実行は、必要な `line` 引数で指定した行で再開されます。 `line` 引数は、行ラベルまたは行番号であり、エラーハンドラーと同じプロシージャ内にある必要があります。  
  
## <a name="remarks"></a>コメント  
  
> [!NOTE]
> 構造化例外処理は、構造化されていない例外処理、`On Error` および `Resume` ステートメントを使用するのではなく、可能な限りコードで使用することをお勧めします。 詳しくは、「[Try...Catch...Finally ステートメント](../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)」をご覧ください。  
  
 エラー処理ルーチン以外の場所で `Resume` ステートメントを使用すると、エラーが発生します。  
  
 `Resume` ステートメントは、`Try...Catch...Finally` ステートメントを含むプロシージャ内では使用できません。  
  
## <a name="example"></a>例  
 この例では、`Resume` ステートメントを使用してプロシージャのエラー処理を終了し、エラーの原因となったステートメントを使用して実行を再開します。 `Resume` ステートメントの使用方法を示すために、エラー番号55が生成されます。  
  
 [!code-vb[VbVbalrErrorHandling#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrErrorHandling/VB/Class1.vb#16)]  
  
## <a name="requirements"></a>要件  
 **名前空間:** [Microsoft. visual basic](../../../visual-basic/language-reference/runtime-library-members.md)  
  
 **アセンブリ:** Visual Basic ランタイムライブラリ (Microsoft... .dll)  
  
## <a name="see-also"></a>参照

- [Try...Catch...Finally ステートメント](../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)
- [Error ステートメント](../../../visual-basic/language-reference/statements/error-statement.md)
- [On Error ステートメント](../../../visual-basic/language-reference/statements/on-error-statement.md)
