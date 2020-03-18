---
title: LINQ to XML における遅延実行とレイジー評価 (C#)
ms.date: 07/20/2015
ms.assetid: 8683d1b4-b7ec-407b-be12-906ebe958a09
ms.openlocfilehash: 9cf28afb5b7b8b3047c8b1b21915ffe7409eb25e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69594565"
---
# <a name="deferred-execution-and-lazy-evaluation-in-linq-to-xml-c"></a>LINQ to XML における遅延実行とレイジー評価 (C#)
クエリと軸の操作は、多くの場合、遅延実行を使用するように実装されています。 このトピックでは、遅延実行の要件と利点、および実装に関する注意点について説明します。  
  
## <a name="deferred-execution"></a>遅延実行  
 遅延実行とは、"*具体*" 値が実際に必要となるまで、式の評価を遅らせることを意味します。 大きなデータ コレクションの操作が必要な場合、特に連結されたクエリや操作が含まれているプログラムでは、遅延実行によってパフォーマンスが大幅に向上することがあります。 最も効果的なケースでは、遅延実行を使用することによって、ソース コレクションの繰り返し処理を 1 度行うだけで済むようになります。  
  
 LINQ テクノロジでは、コア <xref:System.Linq?displayProperty=nameWithType> クラスのメンバーにおいても、<xref:System.Xml.Linq.Extensions?displayProperty=nameWithType> などのさまざまな LINQ 名前空間の拡張メソッドにおいても、遅延実行が広く利用されています。  
  
 C# 言語では、反復子ブロック内で [yield](../../../language-reference/keywords/yield.md) キーワード (`yield-return` ステートメントの形式) を使用することにより、遅延実行が直接サポートされます。 このような反復子は <xref:System.Collections.IEnumerator> 型または <xref:System.Collections.Generic.IEnumerator%601> 型 (または派生型) のコレクションを返します。  
  
## <a name="eager-vs-lazy-evaluation"></a>集中評価とレイジー評価  
 遅延実行を実装するメソッドを記述するときは、レイジー評価と集中評価のどちらを使用してメソッドを実装するかについても決定する必要があります。  
  
- "*レイジー評価*" では、反復子を呼び出すたびに、ソース コレクションの 1 つの要素が処理されます。 これが、一般的な反復子の実装方法です。  
  
- "*集中評価*" では、反復子への最初の呼び出しの結果として、コレクション全体が処理されます。 ソース コレクションの一時的なコピーが必要になる場合もあります。 たとえば <xref:System.Linq.Enumerable.OrderBy%2A> メソッドでは、最初の要素を返す前に、コレクション全体を並べ替える必要があります。  
  
 通常は、レイジー評価を使用するとパフォーマンスが向上します。コレクション評価時のオーバーヘッド処理が均等に分散され、一時データの使用が最小限に抑えられるためです。 もちろん、操作によっては、中間結果を具体化するより他に方法がない場合もあります。  
  
## <a name="next-steps"></a>次の手順  
 このチュートリアルの次のトピックでは、遅延実行について説明します。  
  
- [遅延実行の例 (C#)](./deferred-execution-example.md)  
  
## <a name="see-also"></a>参照

- [チュートリアル: クエリの連結 (C#)](./deferred-execution-and-lazy-evaluation-in-linq-to-xml.md)
- [概念と用語 (関数型変換) (C#)](./concepts-and-terminology-functional-transformation.md)
- [集計操作 (C#)](./aggregation-operations.md)
- [yield](../../../language-reference/keywords/yield.md)
