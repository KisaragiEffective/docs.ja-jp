---
title: リレーションシップを介したクエリの実行
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 297878d0-685b-4c01-b2e0-9d731b7322bc
ms.openlocfilehash: 1675e4c6a610373e1c981b383ae0229739d96dd0
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "72003319"
---
# <a name="querying-across-relationships"></a>リレーションシップを介したクエリの実行
他のオブジェクトまたは他のオブジェクトのコレクションをクラス定義内で参照することは、データベース内の外部キー リレーションシップに直接対応します。 クエリを実行するときにこのリレーションシップを使用するには、ドット表記を使ってリレーションシップ プロパティにアクセスし、オブジェクト間を移動します。 これらのアクセス操作は、SQL で同等の複雑な結合または相関サブクエリとして変換されます。  
  
 たとえば、次のクエリでは、ロンドン在住の顧客からの注文のみが結果として得られるように注文から顧客へと移動します。  
  
 [!code-csharp[DLinqQueryConcepts#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryConcepts/cs/Program.cs#3)]
 [!code-vb[DLinqQueryConcepts#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryConcepts/vb/Module1.vb#3)]  
  
 リレーションシップのプロパティが存在しない場合は、次のコードのように、SQL クエリの場合と同様に、*結合*として手動で作成する必要があります。  
  
 [!code-csharp[DLinqQueryConcepts#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryConcepts/cs/Program.cs#4)]
 [!code-vb[DLinqQueryConcepts#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryConcepts/vb/Module1.vb#4)]  
  
 *リレーションシップ*プロパティを使用して、この特定のリレーションシップを1回だけ定義できます。 こうすると、使いやすいドット構文を使用できるようになります。 しかし、それだけがリレーションシップ プロパティの利点ではありません。なぜなら、通常はドメイン固有のオブジェクト モデルは階層構造またはグラフとして定義されるからです。 プログラムで使用するオブジェクトには他のオブジェクトへの参照が含まれます。 このオブジェクト間リレーションシップがデータベースの外部キー型リレーションシップに相当するのは、単なる幸運な偶然です。 プロパティへのアクセスは、結合を記述するための便利な方法となります。  
  
 この点では、リレーションシップ プロパティは、クエリそのものよりもクエリの結果でより重要です。 クエリによって特定の顧客に関するデータが取得された後で、クラス定義ではその顧客の注文が存在することが示されます。 つまり、特定の顧客の `Orders` プロパティにその顧客のすべての注文が含まれると見なすことができます。 実際には、これはクラスをこのように定義することにより宣言したコントラクトです。 クエリによって注文が要求されなかった場合でも、ここに注文が含まれていると見なすことができます。 オブジェクト モデルに対して期待されるのは、それがデータベースがメモリ内に拡張されたものであり、関連オブジェクトがすぐに使用できる状態にある、という錯覚を維持することです。  
  
 リレーションシップが得られたので、クラスに定義されたリレーションシップ プロパティを参照してクエリを作成できます。 これらのリレーションシップ参照は、データベース内の外部キー リレーションシップに対応します。 これらのリレーションシップを使用する操作は、SQL で同等の複雑な結合として変換されます。 <xref:System.Data.Linq.Mapping.AssociationAttribute> 属性を使用してリレーションシップを定義している限りは、明示的な結合を [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] で記述する必要はありません。  
  
 この錯覚を維持するために、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は*遅延読み込み*と呼ばれる手法を実装しています。 詳細については、「[遅延読み込みと即時読み込み](deferred-versus-immediate-loading.md)」を参照してください。  
  
 次の SQL クエリを検討して、`CustomerID`-`OrderID` のペアの一覧を射影します。  
  
```sql
SELECT t0.CustomerID, t1.OrderID  
FROM   Customers AS t0 INNER JOIN  
          Orders AS t1 ON t0.CustomerID = t1.CustomerID  
WHERE  (t0.City = @p0)  
```  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] を使用して同じ結果を得るには、`Orders` クラスに既に存在する `Customer` プロパティ参照を使用します。 `Orders` リファレンスは、次のコードのように、クエリを実行し、`OrderID` のペア -`CustomerID`を射影するために必要な情報を提供します。  
  
 [!code-csharp[DLinqQueryConcepts#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryConcepts/cs/Program.cs#5)]
 [!code-vb[DLinqQueryConcepts#5](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryConcepts/vb/Module1.vb#5)]  
  
 逆の操作も可能です。 つまり、`Orders` にクエリを実行し、その `Customer` リレーションシップ参照を使用して、関連付けられた `Customer` オブジェクトに関する情報にアクセスできます。 次のコードでは、前と同じように `OrderID` ペア -同じ `CustomerID`を射影しますが、今回は `Customers`ではなく `Orders` に対してクエリを実行します。  
  
 [!code-csharp[DLinqQueryConcepts#6](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryConcepts/cs/Program.cs#6)]
 [!code-vb[DLinqQueryConcepts#6](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryConcepts/vb/Module1.vb#6)]  
  
## <a name="see-also"></a>参照

- [クエリの概念](query-concepts.md)
