---
title: Select 句
ms.date: 07/20/2015
f1_keywords:
- vb.QuerySelect
helpviewer_keywords:
- Select statement [Visual Basic]
- Select clause [Visual Basic]
- queries [Visual Basic], Select
ms.assetid: 27a3f61c-5960-4692-9b91-4d0c4b6178fe
ms.openlocfilehash: 5ebb813229d5d517b369036c69b2d23c8ee1c9f5
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350405"
---
# <a name="select-clause-visual-basic"></a>Select 句 (Visual Basic)
クエリの結果を定義します。  
  
## <a name="syntax"></a>構文  
  
```vb  
Select [ var1 = ] fieldName1 [, [ var2 = ] fieldName2 [...] ]  
```  
  
## <a name="parts"></a>指定項目  
 `var1`  
 省略可。 列式の結果を参照するために使用できる別名。  
  
 `fieldName1`  
 必須。 クエリ結果に返されるフィールドの名前。  
  
## <a name="remarks"></a>コメント  
 `Select` 句を使用すると、クエリから返される結果を定義できます。 これにより、クエリによって作成された新しい匿名型のメンバーを定義することも、クエリによって返される名前付きの型のメンバーを対象にすることもできます。 `Select` 句は、クエリには必要ありません。 `Select` 句が指定されていない場合、クエリは、現在のスコープで特定された範囲変数のすべてのメンバーに基づいて型を返します。 詳細については、「[匿名型](../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)」を参照してください。 クエリで名前付きの型を作成すると、型 <xref:System.Collections.Generic.IEnumerable%601> の結果が返されます。 `T` は作成された型です。  
  
 `Select` 句は、現在のスコープ内の任意の変数を参照できます。 これには、`From` 句 (または `From` 句) で識別される範囲変数が含まれます。 また、`Aggregate`、`Let`、`Group By`、`Group Join` の各句、またはクエリ式の前の `Select` 句の変数によって、別名を使用して作成された新しい変数も含まれます。 `Select` 句には、静的な値を含めることもできます。 たとえば、次のコード例は、`ProductName`、`Price`、`Discount`、および `DiscountedPrice`の4つのメンバーを持つ新しい匿名型として、`Select` 句でクエリ結果を定義するクエリ式を示しています。 `ProductName` と `Price` のメンバー値は、`From` 句で定義されている製品範囲変数から取得されます。 `DiscountedPrice` メンバーの値は、`Let` 句で計算されます。 `Discount` メンバーは静的な値です。  
  
 [!code-vb[VbSimpleQuerySamples#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#27)]  
  
 `Select` 句には、後続のクエリ句の範囲変数の新しいセットが導入されています。また、以前の範囲変数はスコープに含まれなくなりました。 クエリ式の最後の `Select` 句によって、クエリの戻り値が決まります。 たとえば、次のクエリでは、合計が500を超えるすべての顧客注文の会社名と注文 ID が返されます。 最初の `Select` 句は、`Where` 句の範囲変数と2番目の `Select` 句を識別します。 2番目の `Select` 句は、クエリによって返される値を新しい匿名型として識別します。  
  
 [!code-vb[VbSimpleQuerySamples#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#28)]  
  
 返される1つの項目が `Select` 句によって識別される場合、クエリ式はその1つの項目の型のコレクションを返します。 `Select` 句によって返される複数の項目が識別される場合、クエリ式は、選択した項目に基づいて、新しい匿名型のコレクションを返します。 たとえば、次の2つのクエリは、`Select` 句に基づいて2つの異なる型のコレクションを返します。 最初のクエリでは、会社名のコレクションが文字列として返されます。 2番目のクエリは、会社名と住所情報を入力した `Customer` オブジェクトのコレクションを返します。  
  
 [!code-vb[VbSimpleQuerySamples#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#29)]  
  
## <a name="example"></a>例  
 次のクエリ式では、`From` 句を使用して、`customers` コレクションの範囲変数 `cust` を宣言します。 `Select` 句では、顧客名と ID 値を選択し、新しい範囲変数の `CompanyName` と `CustomerID` 列を設定します。 `For Each` ステートメントは、返された各オブジェクトをループし、各レコードの `CompanyName` および `CustomerID` 列を表示します。  
  
 [!code-vb[VbSimpleQuerySamples#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#30)]  
  
## <a name="see-also"></a>参照

- [Visual Basic における LINQ の概要](../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [クエリ](../../../visual-basic/language-reference/queries/index.md)
- [From 句](../../../visual-basic/language-reference/queries/from-clause.md)
- [WHERE 句](../../../visual-basic/language-reference/queries/where-clause.md)
- [Order By 句](../../../visual-basic/language-reference/queries/order-by-clause.md)
- [匿名型](../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)
