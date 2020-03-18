---
title: Group By 句
ms.date: 07/20/2015
f1_keywords:
- vb.QueryGroupByInto
- vb.QueryGroupBy
- vb.QueryGroupRef
- vb.QueryGroupInto
- vb.QueryGroup
helpviewer_keywords:
- queries [Visual Basic], Group By
- Group By statement [Visual Basic]
- Group By clause [Visual Basic]
ms.assetid: b1b5dcea-6654-473b-a2db-01f7e4c265d7
ms.openlocfilehash: 87080254ad5d237a593f0c35e7c3fdaef3a8ad59
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350471"
---
# <a name="group-by-clause-visual-basic"></a>Group By 句 (Visual Basic)
クエリ結果の要素をグループ化します。 これを使用して、グループごとに集計関数を適用することもできます。 グループ化操作は、1 つ以上のキーに基づきます。  
  
## <a name="syntax"></a>構文  
  
```vb  
Group [ listField1 [, listField2 [...] ] By keyExp1 [, keyExp2 [...] ]  
  Into aggregateList  
```  
  
## <a name="parts"></a>指定項目  
  
- `listField1`, `listField2`  
  
     省略可。 グループ化された結果に含めるフィールドを明示的に示す、クエリ変数の 1 つ以上のフィールドです。 フィールドを指定しない場合、グループ化された結果にはクエリ変数のすべてのフィールドが含まれます。  
  
- `keyExp1`  
  
     必須。 要素のグループを決定するために使用するキーを識別する式です。 複数のキーを指定して、複合キーを指定できます。  
  
- `keyExp2`  
  
     省略可。 `keyExp1` と結合して複合キーを作成する 1 つ以上の追加キーです。  
  
- `aggregateList`  
  
     必須。 グループの集計方法を示す 1 つ以上の式です。 グループ化された結果のメンバー名を示すには、次のいずれかの形式で、 `Group` キーワードを使用します。  
  
    ```vb  
    Into Group  
    ```  
  
     または  
  
    ```vb  
    Into <alias> = Group  
    ```  
  
     グループに適用する集計関数を含めることもできます。  
  
## <a name="remarks"></a>コメント  
 `Group By` 句を使用して、クエリの結果をグループに分割できます。 グループ化は、1 つのキー、または複数のキーで構成される複合キーに基づいて行われます。 一致するキー値と関連付けられた要素は、同じグループに入れられます。  
  
 グループの参照に使用するメンバー名を示すには、 `aggregateList` 句の `Into` パラメーターと `Group` キーワードを使用します。 `Into` 句に集計関数を含めることで、グループ化された要素の値を計算することもできます。 標準的な集計関数の一覧については、「 [Aggregate Clause](../../../visual-basic/language-reference/queries/aggregate-clause.md)」をご覧ください。  
  
## <a name="example"></a>例  
 次のコード例では、場所 (国/地域) に基づいて顧客の一覧をグループ化し、各グループ内の顧客の数を示します。 結果は、国/地域名で並べ替えられます。 グループ化した結果は、市区町村名によって並べ替えられます。  
  
 [!code-vb[VbSimpleQuerySamples#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#11)]  
  
## <a name="see-also"></a>参照

- [Visual Basic における LINQ の概要](../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [クエリ](../../../visual-basic/language-reference/queries/index.md)
- [Select 句](../../../visual-basic/language-reference/queries/select-clause.md)
- [From 句](../../../visual-basic/language-reference/queries/from-clause.md)
- [Order By 句](../../../visual-basic/language-reference/queries/order-by-clause.md)
- [Aggregate 句](../../../visual-basic/language-reference/queries/aggregate-clause.md)
- [Group Join 句](../../../visual-basic/language-reference/queries/group-join-clause.md)
