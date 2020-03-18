---
title: Where 句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryWhere
helpviewer_keywords:
- Where statement [Visual Basic]
- queries [Visual Basic], Where
- Where clause [Visual Basic]
ms.assetid: 48b5c2c5-3181-429c-8545-894296798c89
ms.openlocfilehash: 60b7ebe96ce0c4580c36675b2e4aa5f9888732c3
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349625"
---
# <a name="where-clause-visual-basic"></a>Where 句 (Visual Basic)
クエリのフィルター条件を指定します。  
  
## <a name="syntax"></a>構文  
  
```vb  
Where condition  
```  
  
## <a name="parts"></a>指定項目  
 `condition`  
 必須。 コレクション内の現在の項目の値が出力コレクションに含まれるかどうかを決定する式。 式は、`Boolean` 値または `Boolean` 値に相当する値に評価される必要があります。 条件が `True`と評価された場合、要素はクエリの結果に含まれます。それ以外の場合、要素はクエリの結果から除外されます。  
  
## <a name="remarks"></a>コメント  
 `Where` 句を使用すると、特定の条件を満たす要素のみを選択してクエリデータをフィルター処理できます。 `Where` 句が `True` と評価される値を持つ要素がクエリ結果に含まれます。その他の要素は除外されます。 `Where` 句で使用される式は、値が0の場合に `False` に評価される整数など、`Boolean` または `Boolean`に相当するものに評価される必要があります。 `And`、`Or`、`AndAlso`、`OrElse`、`Is`、`IsNot`などの論理演算子を使用して、`Where` 句で複数の式を組み合わせることができます。  
  
 既定では、クエリ式は、アクセスされるまで評価されません。たとえば、データバインドされている場合や、`For` ループ内で反復処理される場合です。 その結果、`Where` 句は、クエリがアクセスされるまで評価されません。 `Where` 句で使用されているクエリの外部の値がある場合は、クエリの実行時に `Where` 句で適切な値が使用されていることを確認してください。 クエリ実行の詳細については、「[初めての LINQ クエリの作成](../../../visual-basic/programming-guide/concepts/linq/writing-your-first-linq-query.md)」を参照してください。  
  
 `Where` 句内で関数を呼び出して、コレクション内の現在の要素の値に対して計算または操作を実行することができます。 `Where` 句で関数を呼び出すと、クエリがアクセス時ではなく定義された直後に実行される可能性があります。 クエリ実行の詳細については、「[初めての LINQ クエリの作成](../../../visual-basic/programming-guide/concepts/linq/writing-your-first-linq-query.md)」を参照してください。  
  
## <a name="example"></a>例  
 次のクエリ式では、`From` 句を使用して、`customers` コレクション内の各 `Customer` オブジェクトに対して `cust` 範囲変数を宣言します。 `Where` 句では、範囲変数を使用して、指定された地域の顧客に出力を制限します。 `For Each` ループでは、クエリ結果に各顧客の会社名が表示されます。  
  
 [!code-vb[VbSimpleQuerySamples#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#23)]  
  
## <a name="example"></a>例  
 次の例では、`Where` 句で `And` および `Or` 論理演算子を使用します。  
  
 [!code-vb[VbSimpleQuerySamples#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#31)]  
  
## <a name="see-also"></a>参照

- [Visual Basic における LINQ の概要](../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [クエリ](../../../visual-basic/language-reference/queries/index.md)
- [From 句](../../../visual-basic/language-reference/queries/from-clause.md)
- [Select 句](../../../visual-basic/language-reference/queries/select-clause.md)
- [For Each...Next ステートメント](../../../visual-basic/language-reference/statements/for-each-next-statement.md)
