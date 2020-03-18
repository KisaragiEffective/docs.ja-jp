---
title: DataRow の比較 (LINQ to DataSet)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8fe0eadf-297b-487c-8d4b-7816753c2883
ms.openlocfilehash: fbd642fb3da6d664df9076b8d7576865d516727e
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634899"
---
# <a name="comparing-datarows-linq-to-dataset"></a>DataRow の比較 (LINQ to DataSet)
統合言語クエリ (LINQ) では、ソース要素を比較して等しいかどうかを確認するためのさまざまな集合演算子が定義されています。 LINQ には、次の集合演算子が用意されています。  
  
- <xref:System.Linq.Enumerable.Distinct%2A>  
  
- <xref:System.Linq.Enumerable.Union%2A>  
  
- <xref:System.Linq.Enumerable.Intersect%2A>  
  
- <xref:System.Linq.Enumerable.Except%2A>  
  
 これらの演算子は、要素のコレクションに対して <xref:System.Collections.Generic.IEqualityComparer%601.GetHashCode%2A> メソッドおよび <xref:System.Collections.Generic.IEqualityComparer%601.Equals%2A> メソッドを呼び出すことによってソース要素を比較します。 <xref:System.Data.DataRow> の場合、これらの演算子によって実行されるのは参照の比較です。表形式のデータに対する集合演算においては適切な動作とは言えません。 通常、集合演算で重要なのは要素の値が等しいかどうかであって、要素の参照が等しいかどうかではありません。 したがって、<xref:System.Data.DataRowComparer> クラスが LINQ to DataSet に追加されています。 このクラスを使用することで行の値を比較できます。  
  
 <xref:System.Data.DataRowComparer> クラスには、<xref:System.Data.DataRow> の値を比較するための機能が実装されています。このクラスを <xref:System.Linq.Enumerable.Distinct%2A> などの集合演算に使用できます。 このクラスを直接インスタンス化することはできません。代わりに、<xref:System.Data.DataRowComparer.Default%2A> プロパティを使用して、<xref:System.Data.DataRowComparer%601> のインスタンスを取得する必要があります。 その後、<xref:System.Data.DataRowComparer%601.Equals%2A> メソッドを呼び出す際、比較する 2 つの <xref:System.Data.DataRow> オブジェクトを入力パラメーターとして渡します。 <xref:System.Data.DataRowComparer%601.Equals%2A> メソッドは、2 つの `true` オブジェクトに含まれる一連の列値が等しければ <xref:System.Data.DataRow> を、それ以外の場合は `false` を返します。  
  
## <a name="example"></a>使用例  
 この例では、両方のテーブルに存在する連絡先を `Intersect` を使用して取得します。  
  
 [!code-csharp[DP LINQ to DataSet Examples#Intersect2](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#intersect2)]
 [!code-vb[DP LINQ to DataSet Examples#Intersect2](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#intersect2)]  
  
### <a name="example"></a>使用例  
 次の例では、2 つの行を比較して、そのハッシュ コードを取得します。  
  
 [!code-vb[DP LINQ to DataSet Examples#CompareDifferentRows](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#comparedifferentrows)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Data.DataRowComparer>
- [DataSet へのデータの読み込み](loading-data-into-a-dataset.md)
- [LINQ to DataSet の例](linq-to-dataset-examples.md)
