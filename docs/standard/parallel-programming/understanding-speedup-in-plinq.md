---
title: PLINQ での高速化について
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, performance tuning
ms.assetid: 53706c7e-397d-467a-98cd-c0d1fd63ba5e
ms.openlocfilehash: 07b5027d560a4caccc6c0a516c3f70c11df6be83
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139909"
---
# <a name="understanding-speedup-in-plinq"></a>PLINQ での高速化について
PLINQ の主な目的は、マルチコア コンピューターでクエリ デリゲートを並列実行することで、LINQ to Objects クエリの実行を高速化することです。 PLINQ は、ソース コレクション内の各要素の処理が独立しており、個々のデリゲート間に関連する共有状態がない場合に最適です。 このような操作は LINQ to Objects および PLINQ で一般的であり、複数のスレッドでのスケジューリングに適しているため、多くの場合、"*適切な並列操作*" と呼ばれます。 ただし、すべてのクエリが適切な並列操作のみで構成されているわけではなく、ほとんど場合、クエリには、並列化できないか、並列実行速度を低下させるいくつかの演算子が含まれます。 また、完全に適切な並列操作であるクエリの場合でも、PLINQ では引き続きデータ ソースをパーティション分割し、スレッドでの作業をスケジューリングし、通常はクエリの完了時に結果をマージする必要があります。 これらすべての操作では並列処理の計算コストが追加されます。このような並列処理の追加コストを*オーバーヘッド* と呼びます。 PLINQ クエリでの最適なパフォーマンスを実現するための目標は、適切な並列処理の部分を最大化し、オーバーヘッドを必要とする部分を最小化することです。 この記事では、引き続き正しい結果を生成しながら、できる限り効率的な PLINQ クエリを記述するのに役立つ情報を提供します。  
  
## <a name="factors-that-impact-plinq-query-performance"></a>PLINQ クエリのパフォーマンスに影響する要因  
 次のセクションでは、並列クエリのパフォーマンスに影響する最も重要な要因をいくつかリストします。 これらは一般的なものであり、あらゆる状況でクエリのパフォーマンスを予測するには十分ではありません。 当然のことながら、さまざまな代表的な構成と負荷を考慮して、コンピューターでの特定のクエリの実際のパフォーマンスを測定することが重要です。  
  
1. 作業全体の計算コスト。  
  
     高速化を実現するには、PLINQ クエリでオーバーヘッドを相殺するために十分適切な並列処理を行う必要があります。 この作業は、ソース コレクション内の要素数を掛けた各デリゲートの計算コストとして表すことができます。 操作を並列化でき、計算コストが増えるほど、高速化の機会が増えると想定します。 たとえば、関数の実行に 1 ミリ秒かかる場合、1000 要素での順次クエリの操作の実行に 1 秒かかり、一方、4 コアのコンピューターでの並列クエリには 250 ミリ秒しかかかりません。 これで、750 ミリ秒の高速化が実現します。 関数で各要素の実行に 1 秒を要した場合、750 秒の高速化となります。 デリゲートに非常に多くのコストがかかる場合、PLINQ では、ソース コレクション内のごく少数のアイテムのみで大幅な高速化を実現できることがあります。 逆に、単純なデリゲートの小さなソース コレクションは、一般的に PLINQ には適していません。  
  
     次の例では、queryA は PLINQ に適しており、Select 関数で多くの処理が行われると仮定します。 Select ステートメントで十分な処理が行われず、並列処理のオーバーヘッドでほとんどまたはすべての高速化が相殺されるため、queryB は適していません。  
  
    ```vb  
    Dim queryA = From num In numberList.AsParallel()  
                 Select ExpensiveFunction(num); 'good for PLINQ  
  
    Dim queryB = From num In numberList.AsParallel()  
                 Where num Mod 2 > 0  
                 Select num; 'not as good for PLINQ  
    ```  
  
    ```csharp  
    var queryA = from num in numberList.AsParallel()  
                 select ExpensiveFunction(num); //good for PLINQ  
  
    var queryB = from num in numberList.AsParallel()  
                 where num % 2 > 0  
                 select num; //not as good for PLINQ  
    ```  
  
2. システムでの論理コアの数 (並列処理の程度)。  
  
     このポイントは前のセクションの当然の結果です。つまり、より多くの同時実行スレッドに作業が分散されるため、より多くのコアを持つコンピューターでの適切な並列処理を行うクエリはより高速に実行されます。 高速化の全体量は、並列化可能なクエリの作業全体の割合によって異なります。 ただし、すべてのクエリについて、8 コア コンピューターでの実行速度が 4 コア コンピューターでの実行速度の 2 倍になると想定しないでください。 最適なパフォーマンスを得るためにクエリを調整する際には、さまざまなコア数のコンピューターでの実際の結果を測定することが重要です。 このポイントはポイント 1 に関連します。つまり、より多くのコンピューティング リソースを利用するには、より大きなデータセットが必要になります。  
  
3. 操作の数と種類。  
  
     PLINQ では、ソース シーケンス内の要素の順序を維持するために必要な場合に、AsOrdered 演算子を提供します。 順序付けに関連するコストがありますが、通常、これはわずかなコストです。 GroupBy および Join 操作には同様にオーバーヘッドが発生します。 PLINQ は、任意の順序でソース コレクション内の要素を処理し、準備ができしだい次の演算子に渡すことができる場合に最適です。 詳細については、「[Order Preservation in PLINQ (PLINQ における順序維持)](../../../docs/standard/parallel-programming/order-preservation-in-plinq.md)」を参照してください。  
  
4. クエリ実行の形式。  
  
     ToArray または ToList を呼び出すことでクエリの結果を格納する場合、すべての並列スレッドからの結果を単一のデータ構造にマージする必要があります。 これには、避けられない計算コストが含まれます。 同様に、foreach (Visual Basic では For Each) ループを使用して結果を反復処理する場合、ワーカー スレッドからの結果を列挙子スレッドにシリアル化する必要があります。 ただし、各スレッドからの結果に基づいて一部のアクションを実行するだけの場合は、ForAll メソッドを使用して、複数のスレッドでこの処理を実行することができます。  
  
5. マージ オプションの種類。  
  
     PLINQ は、出力をバッファリングし、分割して生成するか、結果セット全体が生成されてからすべて一度に生成するか、あるいは生成時に個々の結果をストリーミングするように構成することができます。 前者では実行時間全体が短縮され、後者では生成される要素間の待機時間が減少します。  マージ オプションが常にクエリのパフォーマンス全体に大きな影響を与えるわけではありませんが、ユーザーが結果の表示を待機する時間が制御されるため、体感パフォーマンスに影響する場合があります。 詳細については、「[Merge Options in PLINQ (PLINQ のマージ オプション)](../../../docs/standard/parallel-programming/merge-options-in-plinq.md)」を参照してください。  
  
6. パーティション分割の種類。  
  
     場合によっては、インデックス可能なソース コレクションに対する PLINQ クエリにより、作業負荷が不均等になります。 その際に、カスタム パーティショナーを作成することで、クエリのパフォーマンスを向上できる場合があります。 詳細については、「[Custom Partitioners for PLINQ and TPL (PLINQ および TPL 用のカスタム パーティショナー)](../../../docs/standard/parallel-programming/custom-partitioners-for-plinq-and-tpl.md)」を参照してください。  
  
## <a name="when-plinq-chooses-sequential-mode"></a>PLINQ で順次モードが選択されるタイミング  
 PLINQ は常に、少なくともクエリが順次実行される同じ速度でクエリを実行しようとします。 PLINQ では、ユーザー デリゲートの計算コストがどれくらい高いかや、入力ソースがどれくらい大きいかは確認されませんが、特定のクエリの "形状" は検索されます。 具体的には、通常、並列モードでのクエリの実行により時間がかかるクエリ演算子または演算子の組み合わせが検索されます。 このような形状が検出されると、PLINQ は既定でシーケンシャル モードに戻ります。  
  
 ただし、特定のクエリのパフォーマンスを測定した後に、実際には並列モードではより高速に実行されることがわかる場合があります。 このような場合は、<xref:System.Linq.ParallelExecutionMode.ForceParallelism?displayProperty=nameWithType> メソッドで <xref:System.Linq.ParallelEnumerable.WithExecutionMode%2A> フラグを使用して、クエリを並列化するように PLINQ に指示できます。 詳細については、「[How to: Specify the Execution Mode in PLINQ (方法: PLINQ の実行モードを指定する)](../../../docs/standard/parallel-programming/how-to-specify-the-execution-mode-in-plinq.md)」を参照してください。  
  
 次のリストでは、既定で PLINQ がシーケンシャル モードで実行するクエリの形状について説明します。  
  
- Select、インデックス付きの Where、インデックス付きの SelectMany、または ElementAt 句 (元のインデックスを削除または再配置した順序付けまたはフィルタリング演算子の後) を含むクエリ。  
  
- Take、TakeWhile、Skip、SkipWhile 演算子を含み、ソース シーケンス内の where インデックスが元の順序ではないクエリ。  
  
- データ ソースのいずれかに最初に順序付けされたインデックスがあり、他のデータ ソースがインデックス可能 (つまり、配列または IList(T)) である場合を除き、Zip または SequenceEquals を含むクエリ。  
  
- インデックス可能なデータ ソースに適用されている場合を除き、Concat を含むクエリ。  
  
- インデックス可能なデータ ソースに適用されている場合を除き、Reverse を含むクエリ。  
  
## <a name="see-also"></a>参照

- [Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md)
