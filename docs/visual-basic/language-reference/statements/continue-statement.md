---
title: Continue ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.continue
helpviewer_keywords:
- Continue statement [Visual Basic]
- loops, transferring to next iteration
ms.assetid: 3ad00103-358b-4af3-a3a8-1b9ea0e995d3
ms.openlocfilehash: 20140cafb68c7e5518bf3d5fa80e56ca1c1de2c6
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354110"
---
# <a name="continue-statement-visual-basic"></a>Continue ステートメント (Visual Basic)
ループの次の反復処理に制御を直ちに転送します。  
  
## <a name="syntax"></a>構文  
  
```vb  
Continue { Do | For | While }  
```  
  
## <a name="remarks"></a>コメント  
 `Do`、`For`、または `While` ループ内から、ループの次の反復処理に転送できます。 制御はすぐにループ条件テストに渡されます。これは、`For` または `While` ステートメントに転送するのと同じです。また、`Until` または `While` 句が含まれている `Do` または `Loop` ステートメントに渡すこともできます。  
  
 `Continue` は、転送を許可するループ内の任意の場所で使用できます。 制御の転送を許可する規則は、 [GoTo ステートメント](../../../visual-basic/language-reference/statements/goto-statement.md)と同じです。  
  
 たとえば、ループが `Try` ブロック、`Catch` ブロック、または `Finally` ブロック内に完全に含まれている場合は、`Continue` を使用してループから転送できます。 一方、`Try`...`End Try` 構造がループ内に含まれている場合、`Continue` を使用して `Finally` ブロックから制御を移すことはできません。また、`Try` 構造から完全に転送した場合にのみ、`Catch` または `Try`ブロックから転送することもできます。`End Try`  
  
 同じ種類のループが入れ子になっている場合 (たとえば、別の `Do` ループ内の `Do` ループ)、`Continue Do` ステートメントは、それを含む最も内側の `Do` ループの次の反復処理にスキップします。 `Continue` を使用して、同じ型の含まれているループの次の反復処理にスキップすることはできません。  
  
 さまざまな種類のループが入れ子になっている場合 (`For` ループ内の `Do` ループなど)、`Continue Do` または `Continue For`を使用して、いずれかのループの次の反復処理に進むことができます。  
  
## <a name="example"></a>例  
 次のコード例では、除数が0の場合に、`Continue While` ステートメントを使用して、配列の次の列にスキップします。 `Continue While` が `For` ループ内にあります。 `While col < lastcol` ステートメントに転送されます。これは、`For` ループを含む最も内側の `While` ループの次の反復です。  
  
 [!code-vb[VbVbalrStatements#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#14)]  
  
## <a name="see-also"></a>参照

- [Do...Loop ステートメント](../../../visual-basic/language-reference/statements/do-loop-statement.md)
- [For...Next ステートメント](../../../visual-basic/language-reference/statements/for-next-statement.md)
- [While...End While ステートメント](../../../visual-basic/language-reference/statements/while-end-while-statement.md)
- [Try...Catch...Finally ステートメント](../../../visual-basic/language-reference/statements/try-catch-finally-statement.md)
