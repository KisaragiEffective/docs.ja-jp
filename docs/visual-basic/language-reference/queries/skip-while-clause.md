---
title: Skip While 句
ms.date: 07/20/2015
f1_keywords:
- vb.QuerySkipWhile
helpviewer_keywords:
- Skip While statement [Visual Basic]
- Skip While clause [Visual Basic]
- queries [Visual Basic], Skip While
ms.assetid: 5dee8350-7520-4f1a-b00d-590cacd572d6
ms.openlocfilehash: 47703e445865435f5bf5312c3fe41833ac21aa3f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74333138"
---
# <a name="skip-while-clause-visual-basic"></a>Skip While 句 (Visual Basic)
指定された条件が `true` である限り、コレクションの要素をバイパスし、残りの要素を返します。  
  
## <a name="syntax"></a>構文  
  
```vb  
Skip While expression  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`expression`|必須。 テスト要素の条件を表す式。 式は、`Boolean` 値または同等の機能 (`Boolean`として評価される `Integer` など) を返す必要があります。|  
  
## <a name="remarks"></a>コメント  
 `Skip While` 句は、指定された `expression` が `false`を返すまで、クエリ結果の先頭からの要素をバイパスします。 `expression` が `false`を返した後、クエリは残りのすべての要素を返します。 残りの結果については、`expression` は無視されます。  
  
 `Skip While` 句は、特定の条件を満たしていないクエリからすべての要素を除外するために `Where` 句を使用できるという点で、`Where` 句とは異なります。 `Skip While` 句では、最初に条件が満たされない限り、要素は除外されます。 `Skip While` 句は、順序付けられたクエリ結果を操作する場合に最も役立ちます。  
  
 `Skip` 句を使用すると、クエリ結果の先頭から特定の数の結果をバイパスできます。  
  
## <a name="example"></a>例  
 次のコード例では、`Skip While` 句を使用して、米国の最初の顧客が見つかるまで結果をバイパスします。  
  
 [!code-vb[VbSimpleQuerySamples#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#3)]  
  
## <a name="see-also"></a>参照

- [Visual Basic における LINQ の概要](../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [クエリ](../../../visual-basic/language-reference/queries/index.md)
- [Select 句](../../../visual-basic/language-reference/queries/select-clause.md)
- [From 句](../../../visual-basic/language-reference/queries/from-clause.md)
- [Skip 句](../../../visual-basic/language-reference/queries/skip-clause.md)
- [Take While 句](../../../visual-basic/language-reference/queries/take-while-clause.md)
- [WHERE 句](../../../visual-basic/language-reference/queries/where-clause.md)
