---
title: Is 演算子
ms.date: 07/20/2015
f1_keywords:
- vb.is
helpviewer_keywords:
- comparison operators [Visual Basic]
- equivalent objects
- TypeOf...Is expression
- Is operator [Visual Basic]
ms.assetid: 8045a6c8-2a83-45b6-ad47-d09a704c656d
ms.openlocfilehash: 52fbb39ab0a36c8b947b78f464fad26be05ce204
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349542"
---
# <a name="is-operator-visual-basic"></a>Is 演算子 (Visual Basic)
2つのオブジェクト参照変数を比較します。  
  
## <a name="syntax"></a>構文  
  
```vb  
result = object1 Is object2  
```  
  
## <a name="parts"></a>指定項目  
 `result`  
 必須。 任意の `Boolean` 値。  
  
 `object1`  
 必須。 任意の `Object` 名。  
  
 `object2`  
 必須。 任意の `Object` 名。  
  
## <a name="remarks"></a>コメント  
 `Is` 演算子は、2つのオブジェクト参照が同じオブジェクトを参照するかどうかを判断します。 ただし、値の比較は実行されません。 `object1` と `object2` 両方がまったく同じオブジェクトインスタンスを参照している場合、`result` は `True`です。そうでない場合は、`result` が `False`ます。  
  
 また、`Is` を `TypeOf` キーワードと共に使用して `TypeOf`...`Is` 式を作成することもできます。これにより、オブジェクト変数がデータ型と互換性があるかどうかがテストされます。  
  
> [!NOTE]
> `Is` キーワードは、 [Select...Case ステートメント](../../../visual-basic/language-reference/statements/select-case-statement.md)。  
  
## <a name="example"></a>例  
 次の例では、`Is` 演算子を使用して、オブジェクト参照のペアを比較します。 結果は、2つのオブジェクトが同一かどうかを表す `Boolean` 値に割り当てられます。  
  
 [!code-vb[VbVbalrOperators#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#27)]  
  
 前の例で示したように、`Is` 演算子を使用して、事前バインディングオブジェクトと遅延バインディングオブジェクトの両方をテストできます。  
  
## <a name="see-also"></a>参照

- [TypeOf 演算子](../../../visual-basic/language-reference/operators/typeof-operator.md)
- [IsNot 演算子](../../../visual-basic/language-reference/operators/isnot-operator.md)
- [Visual Basic の比較演算子](../../../visual-basic/programming-guide/language-features/operators-and-expressions/comparison-operators.md)
- [Visual Basic における演算子の優先順位](../../../visual-basic/language-reference/operators/operator-precedence.md)
- [機能別の演算子一覧](../../../visual-basic/language-reference/operators/operators-listed-by-functionality.md)
- [演算子および式](../../../visual-basic/programming-guide/language-features/operators-and-expressions/index.md)
