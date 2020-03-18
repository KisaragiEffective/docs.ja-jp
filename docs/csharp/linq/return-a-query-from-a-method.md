---
title: メソッドからクエリを返す
description: クエリを返す方法。
ms.date: 11/30/2016
ms.assetid: db220f79-c35b-41f2-886c-cd068672d42d
ms.openlocfilehash: 1df533770f76301432b104d6f8398f1687750cce
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73972521"
---
# <a name="how-to-return-a-query-from-a-method-c-programming-guide"></a>メソッドからクエリを返す方法 (C# プログラミング ガイド)
この例では、メソッドから、クエリを戻り値として返す方法と `out` パラメーターとして返す方法を示します。  
  
 クエリ オブジェクトはコンポーザブルです。つまり、メソッドからクエリを返すことができます。 クエリを表すオブジェクトには、結果のコレクションではなく、必要に応じて結果を生成する手順が格納されます。 メソッドからクエリ オブジェクトを返すメリットは、そのオブジェクトをさらに構成したり変更したりできることです。 そのため、クエリを返すメソッドの戻り値または `out` パラメーターも同じ型である必要があります。 メソッドがクエリを具象型 <xref:System.Collections.Generic.List%601> または <xref:System.Array> に実体化する場合は、そのメソッドは、クエリ自体ではなくクエリ結果を返すと見なされます。 メソッドから返されたクエリ変数は、引き続き構成または変更できます。  
  
## <a name="example"></a>例  
 次の例は、最初のメソッドはクエリを戻り値として返し、2 番目のメソッドはクエリを `out` パラメーターとして返しています。 どちらの場合も返されるのはクエリであり、クエリ結果ではないことに注意してください。  
  
 [!code-csharp[csProgGuideLINQ#80](~/samples/snippets/csharp/concepts/linq/how-to-return-a-query-from-a-method_1.cs)]  

## <a name="see-also"></a>参照

- [統合言語クエリ (LINQ)](index.md)
