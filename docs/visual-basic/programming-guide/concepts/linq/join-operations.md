---
title: 結合演算
ms.date: 07/20/2015
ms.assetid: 39ab4854-ac84-4738-9d0b-3cb79be84db4
ms.openlocfilehash: b09574369185be13664276c2e84697fc4969c6f5
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353296"
---
# <a name="join-operations-visual-basic"></a>結合操作 (Visual Basic)
2 つのデータ ソースの "*結合*" とは、あるデータ ソースのオブジェクトを、共通の属性を共有する別のデータ ソースのオブジェクトと関連付けることです。  
  
 相互に直接の関連がない 2 つのデータ ソースを対象とするクエリにおいて、結合は重要な操作になります。 オブジェクト指向プログラミングでは、これは一方向の関係における逆の方向など、モデル化されていないオブジェクト間の相関関係を意味する場合があります。 一方向の関係の例として、City 型のプロパティを持つ Customer クラスがあるとします。ただし、City クラスには、Customer オブジェクトのコレクションを表すプロパティはありません。 City オブジェクトのリストから各都市のすべての顧客を取得する場合は、結合演算を使用して顧客を検索できます。  
  
 LINQ framework で用意された結合メソッドは <xref:System.Linq.Enumerable.Join%2A> と <xref:System.Linq.Enumerable.GroupJoin%2A> です。 この 2 つのメソッドは、等結合 (キーが等しいかどうかに基づいて 2 つのデータ ソースを対応させる結合) を実行します。 (比較のために、Transact-sql では ' equals ' 以外の結合演算子がサポートされています (例: ' less ' 演算子)。リレーショナルデータベース用語では、<xref:System.Linq.Enumerable.Join%2A> によって内部結合が実装されます。この型の結合では、他のデータセットに一致するオブジェクトだけが返されます。 リレーショナル データベース用語で <xref:System.Linq.Enumerable.GroupJoin%2A> メソッドに直接相当するものはありませんが、このメソッドは内部結合と左外部結合のスーパーセットを実装します。 左外部結合とは、最初 (左側) のデータ ソースの各要素を返す結合です。これらの要素は、もう一方のデータ ソースの要素と相関関係がなくても返されます。  
  
 次の図は、2 つのセットと、内部結合または左外部結合としてこれらのセットに含まれている要素の概念図を示しています。  
  
 ![内側&#47;外側を示す 2 つの重なり合う円。](./media/join-operations/join-method-overlapping-circles.png)  
  
## <a name="methods"></a>メソッド  
  
|メソッド名|説明|Visual Basic クエリ式の構文|詳細|  
|-----------------|-----------------|------------------------------------------|----------------------|  
|Join|キー セレクター関数に基づいて 2 つのシーケンスを結合し、値のペアを抽出します。|`From x In …, y In … Where x.a = y.a`<br /><br /> または<br /><br /> `Join … [As …]In … On …`|<xref:System.Linq.Enumerable.Join%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Join%2A?displayProperty=nameWithType>|  
|GroupJoin|キー セレクター関数に基づいて 2 つのシーケンスを結合し、各要素について結果として得られる一致をグループ化します。|`Group Join … In … On …`|<xref:System.Linq.Enumerable.GroupJoin%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.GroupJoin%2A?displayProperty=nameWithType>|  
  
## <a name="see-also"></a>参照

- <xref:System.Linq>
- [標準クエリ演算子の概要 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/standard-query-operators-overview.md)
- [匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)
- [結合およびクロス積クエリの作成](../../../../framework/data/adonet/sql/linq/formulate-joins-and-cross-product-queries.md)
- [Join 句](../../../../visual-basic/language-reference/queries/join-clause.md)
- [方法: 異種ファイルのコンテンツを結合する (LINQ) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-join-content-from-dissimilar-files-linq.md)
- [方法: 複数のソースからオブジェクトコレクションにデータを設定する (LINQ) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-populate-object-collections-from-multiple-sources-linq.md)
