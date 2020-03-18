---
title: REM ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.'
- vb.Rem
- Rem
- "'"
helpviewer_keywords:
- REM statement [Visual Basic]
- comments, Visual Basic code
- code comments, Visual Basic
- Visual Basic code, comments
- "' comment marker character [Visual Basic]"
ms.assetid: 34126d7f-e0f9-476d-91e6-b31b398615dc
ms.openlocfilehash: bdde4beae242c3175b02cd2af252babb850416f6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346730"
---
# <a name="rem-statement-visual-basic"></a>REM ステートメント (Visual Basic)
プログラムのソースコードに説明の解説を含めるために使用されます。  
  
## <a name="syntax"></a>構文  
  
```vb  
REM comment  
' comment  
```  
  
## <a name="parts"></a>指定項目  
 `comment`  
 省略可。 追加するコメントのテキスト。 `REM` キーワードと `comment`の間にはスペースが必要です。  
  
## <a name="remarks"></a>コメント  
 `REM` ステートメントを1行に単独で配置することも、別のステートメントの後の行に配置することもできます。 `REM` ステートメントは、行の最後のステートメントである必要があります。 別のステートメントの後に続く場合、`REM` は、そのステートメントからスペースで区切る必要があります。  
  
 `REM`ではなく、単一引用符 (`'`) を使用できます。 これは、コメントが同じ行で別のステートメントに続くか、または行に単独で存在するかにかかわらず当てはまります。  
  
> [!NOTE]
> 行連結シーケンス (`_`) を使用して、`REM` ステートメントを続行することはできません。 コメントが開始されると、コンパイラは文字が特別な意味を持つかどうかを検査しません。 複数行のコメントの場合は、各行に別の `REM` ステートメントまたはコメントシンボル (`'`) を使用します。  
  
## <a name="example"></a>例  
 次の例は、プログラムに説明の解説を含めるために使用される `REM` ステートメントを示しています。 また、`REM`の代わりに単一引用符文字 (`'`) を使用する方法についても説明します。  
  
 [!code-vb[VbVbalrStatements#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>参照

- [コード内のコメント](../../../visual-basic/programming-guide/program-structure/comments-in-code.md)
- [方法 : コード内でステートメントを分割および連結する](../../../visual-basic/programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)
