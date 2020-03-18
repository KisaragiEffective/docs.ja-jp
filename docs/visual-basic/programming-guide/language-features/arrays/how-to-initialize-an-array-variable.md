---
title: '方法: 配列変数を初期化する'
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], initializing
- arrays [Visual Basic], variables
- arrays [Visual Basic], initializing
- arrays [Visual Basic], declaring
ms.assetid: aadd7a60-7ca4-4608-b986-091f19e7fc10
ms.openlocfilehash: 509859cbec41ca31b3abaa1c739e2975ec93bc0e
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351878"
---
# <a name="how-to-initialize-an-array-variable-in-visual-basic"></a>方法: Visual Basic で配列変数を初期化する
配列リテラルを `New` 句に含めること、および配列の初期値を指定することで、配列変数を初期化します。 型を指定するか、配列リテラル内の値から推論することを許可できます。 型の推論方法の詳細については、 [「配列」](../../../../visual-basic/programming-guide/language-features/arrays/index.md)の「配列に初期値を設定する」を参照してください。  
  
### <a name="to-initialize-an-array-variable-by-using-an-array-literal"></a>配列リテラルを使用して配列変数を初期化するには  
  
- `New` 句で、または配列値を割り当てるときに、中かっこ (`{}`) の中に要素の値を指定します。 変数を宣言、作成、および初期化して `Char` 型の要素を持つ配列を含めるいくつかの方法を次の例に示します。  
  
     [!code-vb[VbVbalrArrays#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrArrays/VB/Class1.vb#16)]  
  
     各ステートメントを実行すると、長さが 3 の配列が作成され、インデックス 0 からインデックス 2 の各要素に初期値が格納されます。 上限および値の両方を指定した場合、インデックス 0 から上限までの、すべての要素の値を含める必要があります。  
  
     配列リテラルで要素の値を指定した場合は、インデックスの上限を指定する必要はありません。 上限が指定されていない場合、配列リテラル内の値の数に基づいて配列のサイズが推論されます。  
  
### <a name="to-initialize-a-multidimensional-array-variable-by-using-array-literals"></a>配列リテラルを使用して多次元配列変数を初期化するには  
  
- 中かっこ (`{}`) で囲んだ値を入れ子にして中かっこの中に含めます。 入れ子になったすべての配列リテラルが、型と長さが同じ配列として推論されるようにします。 多次元配列を初期化するいくつかの例を次に示します。  
  
     [!code-vb[VbVbalrArrays#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrArrays/VB/Class1.vb#17)]  
  
- 配列の範囲は明示的に指定することも省略することもできます。省略した場合、配列リテラル内の値に基づいてコンパイラによって配列の範囲が推論されます。 上限と値の両方を指定する場合は、すべての次元について、インデックス 0 ～上限までの値を指定する必要があります。 変数を宣言、作成、および初期化して `Short` 型の要素を持つ 2 次元配列を含めるいくつかの方法を次の例に示します。  
  
     [!code-vb[VbVbalrArrays#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrArrays/VB/Class1.vb#18)]  
  
     各ステートメントを実行すると、初期化された 6 つの要素 (インデックスは `(0,0)`、`(0,1)`、`(0,2)`、`(1,0)`、`(1,1)`、および `(1,2)`) が格納された配列が作成されます。 配列のそれぞれの位置には値 `10` が格納されます。  
  
- 次の例では、多次元配列を反復処理します。 Visual Basic で記述された Windows コンソール アプリケーションでは、コードを `Sub Main()` メソッド内に貼り付けます。 最終コメントは出力を示しています。  
  
     [!code-vb[VbVbalrArrays#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrArrays/VB/Class1.vb#31)]  
  
### <a name="to-initialize-a-jagged-array-variable-by-using-array-literals"></a>配列リテラルを使用してジャグ配列変数を初期化するには  
  
- 中かっこ (`{}`) で囲んだオブジェクトの値を入れ子にします。 さまざまな長さの配列を指定する配列リテラルを入れ子にすることもできますが、ジャグ配列の場合は、入れ子になった配列リテラルがかっこ (`()`) で囲まれていることを確認してください。 かっこで囲むことで、入れ子になった配列リテラルが強制的に評価され、結果の配列がジャグ配列の初期値として使用されるようになります。 ジャグ配列を初期化する 2 つの例を次に示します。  
  
     [!code-vb[VbVbalrArrays#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrArrays/VB/Class1.vb#19)]  
  
- 次の例では、ジャグ配列を反復処理します。 Visual Basic で記述された Windows コンソール アプリケーションでは、コードを `Sub Main()` メソッド内に貼り付けます。  コードの最終コメントは出力を示しています。  
  
     [!code-vb[VbVbalrArrays#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrArrays/VB/Class1.vb#32)]  
  
## <a name="see-also"></a>関連項目

- [配列](../../../../visual-basic/programming-guide/language-features/arrays/index.md)
- [配列のトラブルシューティング](../../../../visual-basic/programming-guide/language-features/arrays/troubleshooting-arrays.md)
