---
title: Order By 句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryOrderBy
- vb.QueryAscending
- vb.QueryDescending
helpviewer_keywords:
- queries [Visual Basic], Order By
- Order By clause [Visual Basic]
- Order By statement [Visual Basic]
ms.assetid: fa911282-6b81-44c7-acfa-46b5bb93df75
ms.openlocfilehash: a7104e3dd82ff2dde2911861ce98a7367faf3b25
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350423"
---
# <a name="order-by-clause-visual-basic"></a>Order By 句 (Visual Basic)
クエリ結果の並べ替え順序を指定します。  
  
## <a name="syntax"></a>構文  
  
```vb  
Order By orderExp1 [ Ascending | Descending ] [, orderExp2 [...] ]  
```  
  
## <a name="parts"></a>指定項目  
 `orderExp1`  
 必須。 返された値の順序付け方法を示す、現在のクエリ結果の1つまたは複数のフィールド。 フィールド名は、コンマ (,) で区切る必要があります。 `Ascending` または `Descending` キーワードを使用して、昇順または降順で並べ替えられた各フィールドを識別できます。 `Ascending` または `Descending` キーワードが指定されていない場合、既定の並べ替え順序は昇順です。 並べ替え順序のフィールドは、左から右に優先されます。  
  
## <a name="remarks"></a>コメント  
 `Order By` 句を使用して、クエリの結果を並べ替えることができます。 `Order By` 句では、現在のスコープの範囲変数に基づいてのみ結果を並べ替えることができます。 たとえば、`Select` 句では、クエリ式に新しいスコープを追加し、そのスコープに新しいイテレーション変数を指定します。 クエリ内の `Select` 句の前に定義された範囲変数は、`Select` 句の後では使用できません。 したがって、`Select` 句で使用できないフィールドで結果を並べ替える場合は、`Order By` 句を `Select` 句の前に配置する必要があります。 これを行う必要がある場合の1つの例は、結果の一部として返されないフィールドによってクエリを並べ替える場合です。  
  
 フィールドの昇順と降順の順序は、フィールドのデータ型の <xref:System.IComparable> インターフェイスの実装によって決定されます。 データ型に <xref:System.IComparable> インターフェイスが実装されていない場合、並べ替え順序は無視されます。  
  
## <a name="example"></a>例  
 次のクエリ式では、`From` 句を使用して、`books` コレクションの範囲変数 `book` を宣言します。 `Order By` 句は、クエリの結果を価格で昇順に並べ替えます (既定値)。 同じ価格の書籍は、タイトルの昇順で並べ替えられます。 `Select` 句は、クエリによって返される値として `Title` と `Price` プロパティを選択します。  
  
 [!code-vb[VbSimpleQuerySamples#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#24)]  
  
## <a name="example"></a>例  
 次のクエリ式では、`Order By` 句を使用して、クエリ結果を価格で降順に並べ替えます。 同じ価格の書籍は、タイトルの昇順で並べ替えられます。  
  
 [!code-vb[VbSimpleQuerySamples#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#25)]  
  
## <a name="example"></a>例  
 次のクエリ式では、`Select` 句を使用して、書籍のタイトル、価格、発行日、および作成者を選択します。 次に、新しいスコープの範囲変数の `Title`、`Price`、`PublishDate`、および `Author` の各フィールドを設定します。 `Order By` 句は、作成者名、書籍のタイトル、価格で新しい範囲変数を並べ替えます。 各列は、既定の順序 (昇順) で並べ替えられます。  
  
 [!code-vb[VbSimpleQuerySamples#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#26)]  
  
## <a name="see-also"></a>参照

- [Visual Basic における LINQ の概要](../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [クエリ](../../../visual-basic/language-reference/queries/index.md)
- [Select 句](../../../visual-basic/language-reference/queries/select-clause.md)
- [From 句](../../../visual-basic/language-reference/queries/from-clause.md)
