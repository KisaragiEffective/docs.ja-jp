---
title: LINQ to XML における遅延実行とレイジー評価
ms.date: 07/20/2015
ms.assetid: 31998eed-b95e-47fb-a865-9de1f337d1fb
ms.openlocfilehash: 8e94b9133a2d2dd287fba91600c94460a5204b2c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346407"
---
# <a name="deferred-execution-and-lazy-evaluation-in-linq-to-xml-visual-basic"></a>LINQ to XML での遅延実行とレイジー評価 (Visual Basic)
クエリと軸の操作は、多くの場合、遅延実行を使用するように実装されています。 このトピックでは、遅延実行の要件と利点、および実装に関する注意点について説明します。  
  
## <a name="deferred-execution"></a>遅延実行  
 遅延実行とは、"*具体*" 値が実際に必要となるまで、式の評価を遅らせることを意味します。 大きなデータ コレクションの操作が必要な場合、特に連結されたクエリや操作が含まれているプログラムでは、遅延実行によってパフォーマンスが大幅に向上することがあります。 最も効果的なケースでは、遅延実行を使用することによって、ソース コレクションの繰り返し処理を 1 度行うだけで済むようになります。  
  
 LINQ テクノロジでは、コア <xref:System.Linq?displayProperty=nameWithType> クラスのメンバーにおいても、<xref:System.Xml.Linq.Extensions?displayProperty=nameWithType> などのさまざまな LINQ 名前空間の拡張メソッドにおいても、遅延実行が広く利用されています。  
  
## <a name="eager-vs-lazy-evaluation"></a>集中評価とレイジー評価  
 遅延実行を実装するメソッドを記述するときは、レイジー評価と集中評価のどちらを使用してメソッドを実装するかについても決定する必要があります。  
  
- "*レイジー評価*" では、反復子を呼び出すたびに、ソース コレクションの 1 つの要素が処理されます。 これが、一般的な反復子の実装方法です。  
  
- "*集中評価*" では、反復子への最初の呼び出しの結果として、コレクション全体が処理されます。 ソース コレクションの一時的なコピーが必要になる場合もあります。 たとえば <xref:System.Linq.Enumerable.OrderBy%2A> メソッドでは、最初の要素を返す前に、コレクション全体を並べ替える必要があります。  
  
 通常は、レイジー評価を使用するとパフォーマンスが向上します。コレクション評価時のオーバーヘッド処理が均等に分散され、一時データの使用が最小限に抑えられるためです。 もちろん、操作によっては、中間結果を具体化するより他に方法がない場合もあります。  
  
## <a name="next-steps"></a>次の手順  
 このチュートリアルの次のトピックでは、遅延実行について説明します。  
  
- [遅延実行の例 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/deferred-execution-example.md)  
  
## <a name="see-also"></a>参照

- [チュートリアル: 遅延実行 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/tutorial-deferred-execution.md)
- [概念と用語 (関数型変換) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/concepts-and-terminology-functional-transformation.md)
- [集計操作 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/aggregation-operations.md)
