---
title: From 句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryFrom
- vb.QueryFromIn
- vb.QueryFromLet
helpviewer_keywords:
- queries [Visual Basic], From
- From clause [Visual Basic]
- From statement [Visual Basic]
ms.assetid: 83e3665e-68a0-4540-a3a3-3d777a0f95d5
ms.openlocfilehash: a63fb41fc2b0ad885acf76ad5d56e446922f5b90
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343778"
---
# <a name="from-clause-visual-basic"></a>From 句 (Visual Basic)
1つ以上の範囲変数と、クエリを実行するコレクションを指定します。  
  
## <a name="syntax"></a>構文  
  
```vb  
From element [ As type ] In collection [ _ ]  
  [, element2 [ As type2 ] In collection2 [, ... ] ]  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`element`|必須。 コレクションの要素を反復処理するために使用する*範囲変数*。 範囲変数は、クエリが `collection`を反復処理するときに、`collection` の各メンバーを参照するために使用されます。 列挙可能な型である必要があります。|  
|`type`|省略可。 `element` の型。 `type` が指定されていない場合、`element` の種類は `collection`から推論されます。|  
|`collection`|必須。 クエリ対象のコレクションを参照します。 列挙可能な型である必要があります。|  
  
## <a name="remarks"></a>コメント  
 `From` 句は、クエリのソースデータと、ソースコレクションの要素を参照するために使用される変数を識別するために使用されます。 これらの変数は*範囲変数*と呼ばれます。 `Aggregate` 句を使用して集計結果のみを返すクエリを識別する場合を除き、クエリには `From` 句が必要です。 詳細については、「[Aggregate 句](../../../visual-basic/language-reference/queries/aggregate-clause.md)」を参照してください。  
  
 クエリで複数の `From` 句を指定すると、結合する複数のコレクションを識別できます。 複数のコレクションが指定されている場合は、個別に反復処理されるか、または関連付けられている場合は結合できます。 `Select` 句を使用して暗黙的にコレクションを結合することも、`Join` または `Group Join` 句を使用して明示的に結合することもできます。 別の方法として、1つの `From` 句で複数の範囲変数とコレクションを指定し、関連する範囲変数とコレクションをコンマで区切って指定することもできます。 次のコード例は、`From` 句の両方の構文オプションを示しています。  
  
 [!code-vb[VbSimpleQuerySamples#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#21)]  
  
 `From` 句は、クエリのスコープを定義します。これは、`For` ループのスコープに似ています。 したがって、クエリのスコープ内の各 `element` 範囲変数には、一意の名前を付ける必要があります。 クエリに対して複数の `From` 句を指定できるので、後続の `From` 句は `From` 句の範囲変数を参照できます。また、前の `From` 句の範囲変数を参照することもできます。 たとえば、次の例は入れ子になった `From` 句を示しています。2番目の句のコレクションは、最初の句の範囲変数のプロパティに基づいています。  
  
 [!code-vb[VbSimpleQuerySamples#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#22)]  
  
 各 `From` 句の後に、追加のクエリ句の任意の組み合わせを使用して、クエリを絞り込むことができます。 クエリは、次の方法で絞り込むことができます。  
  
- `From` と `Select` の句を使用して、または明示的に `Join` または `Group Join` 句を使用して、複数のコレクションを暗黙的に結合します。  
  
- クエリ結果をフィルター処理するには、`Where` 句を使用します。  
  
- `Order By` 句を使用して結果を並べ替えます。  
  
- `Group By` 句を使用して、類似した結果をまとめてグループ化します。  
  
- `Aggregate` 句を使用して、クエリ結果全体に対して評価する集計関数を識別します。  
  
- `Let` 句を使用して、コレクションではなく式によって値が決定される反復変数を導入します。  
  
- `Distinct` 句を使用して、重複するクエリ結果を無視します。  
  
- `Skip`、`Take`、`Skip While`、および `Take While` 句を使用して返される結果の部分を識別します。  
  
## <a name="example"></a>例  
 次のクエリ式では、`From` 句を使用して、`customers` コレクション内の各 `Customer` オブジェクトに対して `cust` 範囲変数を宣言します。 `Where` 句では、範囲変数を使用して、指定された地域の顧客に出力を制限します。 `For Each` ループでは、クエリ結果に各顧客の会社名が表示されます。  
  
 [!code-vb[VbSimpleQuerySamples#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#23)]  
  
## <a name="see-also"></a>参照

- [クエリ](../../../visual-basic/language-reference/queries/index.md)
- [Visual Basic における LINQ の概要](../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [For Each...Next ステートメント](../../../visual-basic/language-reference/statements/for-each-next-statement.md)
- [For...Next ステートメント](../../../visual-basic/language-reference/statements/for-next-statement.md)
- [Select 句](../../../visual-basic/language-reference/queries/select-clause.md)
- [Where 句](../../../visual-basic/language-reference/queries/where-clause.md)
- [Aggregate 句](../../../visual-basic/language-reference/queries/aggregate-clause.md)
- [Distinct 句](../../../visual-basic/language-reference/queries/distinct-clause.md)
- [Join 句](../../../visual-basic/language-reference/queries/join-clause.md)
- [Group Join 句](../../../visual-basic/language-reference/queries/group-join-clause.md)
- [Order By 句](../../../visual-basic/language-reference/queries/order-by-clause.md)
- [Let 句](../../../visual-basic/language-reference/queries/let-clause.md)
- [Skip 句](../../../visual-basic/language-reference/queries/skip-clause.md)
- [Take 句](../../../visual-basic/language-reference/queries/take-clause.md)
- [Skip While 句](../../../visual-basic/language-reference/queries/skip-while-clause.md)
- [Take While 句](../../../visual-basic/language-reference/queries/take-while-clause.md)
