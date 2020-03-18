---
title: PLINQ および TPL 用のカスタム パーティショナー
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, partitioners
ms.assetid: 96153688-9a01-47c4-8430-909cee9a2887
ms.openlocfilehash: 8caea6d8a97b8c0daf7c59718479ea2e12a52d78
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73141569"
---
# <a name="custom-partitioners-for-plinq-and-tpl"></a>PLINQ および TPL 用のカスタム パーティショナー

データ ソース上で操作を並列化する場合の必須の手順の 1 つは、ソースを複数のスレッドによって同時にアクセスできる複数のセクションに*パーティション分割*することです。 PLINQ およびタスク並列ライブラリ (TPL: Task Parallel Library) には、並列クエリまたは <xref:System.Threading.Tasks.Parallel.ForEach%2A> ループを記述するときに透過的に機能する既定のパーティショナーが用意されています。 より高度なシナリオでは、独自のパーティショナーをプラグインできます。

## <a name="kinds-of-partitioning"></a>パーティション分割の種類

データ ソースは、さまざまな方法でパーティション分割できます。 最も効率的な方法は、ソースを複数のサブシーケンスに物理的に分離するのではなく、複数のスレッドが協調して元のソース シーケンスを処理するというものです。 配列や、長さが事前にわかっている <xref:System.Collections.IList> コレクションなどの他のインデックス付きソースの場合は、"*範囲パーティション分割*" が最も簡単なパーティション分割です。 各スレッドは、一意の開始インデックスおよび終了インデックスを受け取ります。そのため、他のスレッドで上書きしたり、上書きされたりすることなく、ソースの範囲を処理できます。 範囲パーティション分割に必要なオーバーヘッドは、最初に行われる範囲を作成する作業のみです。その後は、追加の同期は不要です。 したがって、ワークロードが均等に分割されている限り、優れたパフォーマンスを実現できます。 範囲パーティション分割の欠点は、あるスレッドが早く終了した場合、他のスレッドが作業を終了するのを支援できないことです。

リンク リストまたは長さがわからない他のコレクションの場合は、"*チャンク パーティション分割*" を使用できます。 チャンク パーティション分割では、並列ループまたは並列クエリ内のすべてのスレッドまたはタスクが、1 つのチャンク内のソース要素をいくつか使用し、それらのソース要素を処理し、その後追加の要素を取得します。 パーティショナーは、すべての要素が配布され、重複する要素が存在しないことを保証します。 チャンクは任意のサイズにすることができます。 たとえば、「[方法: 動的パーティションを実装する](../../../docs/standard/parallel-programming/how-to-implement-dynamic-partitions.md)」で示されているパーティショナーは、1 つの要素のみを含むチャンクを作成します。 チャンクが大きすぎない限り、この種類のパーティション分割は、本質的に負荷分散を実行します。これは、スレッドへの要素の割り当てが事前に決定されないからです。 ただし、スレッドが別のチャンクを取得する必要があるたびに、パーティショナーが同期のオーバーヘッドを発生させます。 これらのケースで発生する同期の量は、チャンクのサイズに反比例します。

一般に、範囲パーティション分割の方が高速なのは、デリゲートの実行時間が短時間から中程度までの長さであり、ソースに多数の要素があり、かつ各パーティションの総作業量がほぼ等価である場合のみです。 したがってチャンク パーティション分割の方が、ほとんどのケースで高速です。 要素が少ないか、またはデリゲートの実行時間が長いソースでは、チャンク パーティション分割と範囲パーティション分割のパフォーマンスがほぼ等しくなります。

TPL パーティショナーは、動的な数のパーティションもサポートします。 つまり、たとえば <xref:System.Threading.Tasks.Parallel.ForEach%2A> ループが新しいタスクを作成するときに、パーティションをその場で作成できます。 この機能により、パーティショナーをループ自体と共に拡大縮小できます。 動的パーティショナーも本質的に負荷分散を実行します。 カスタム パーティショナーを作成するときは、<xref:System.Threading.Tasks.Parallel.ForEach%2A> ループから使用できるようにするために、動的パーティション分割をサポートする必要があります。

### <a name="configuring-load-balancing-partitioners-for-plinq"></a>PLINQ 用の負荷分散パーティショナーの構成

<xref:System.Collections.Concurrent.Partitioner.Create%2A?displayProperty=nameWithType> メソッドの一部のオーバーロードを使用すると、配列または <xref:System.Collections.IList> ソース用のパーティショナーを作成し、スレッド間でワークロードの分散を試みるかどうかを指定できます。 負荷分散を実行するようにパーティショナーを構成した場合、チャンク パーティション分割が使用され、要素は要求時に小さいチャンクで各パーティションに渡されます。 この方法は、ループまたはクエリの全体が完了するまで、すべてのパーティションに処理する要素があることを保証するのに役立ちます。 追加のオーバーロードを使用すると、任意の <xref:System.Collections.IEnumerable> ソースを負荷分散パーティション分割できます。

一般に、負荷分散では、パーティションで比較的頻繁にパーティショナーに要素を要求する必要があります。 これに対し、静的パーティション分割を実行するパーティショナーでは、範囲パーティション分割またはチャンク パーティション分割を使用して、各パーティショナーに要素を一度に割り当てることができます。 この方法では、負荷分散よりもオーバーヘッドが少なくて済みますが、あるスレッドが他のスレッドよりもはるかに多くの作業を行う場合は、実行時間が長くなることがあります。 既定では、IList または配列が渡された場合、PLINQ は常に負荷分散なしの範囲パーティション分割を使用します。 PLINQ で負荷分散を有効にするには、次の例に示すように、`Partitioner.Create` メソッドを使用します。

[!code-csharp[TPL_Partitioners#02](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_partitioners/cs/partitioners.cs#02)]
[!code-vb[TPL_Partitioners#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_partitioners/vb/partitionsnippets_vb.vb#02)]

特定のシナリオで負荷分散を使用するかどうかを判断する最適な方法は、典型的な負荷およびコンピューター構成の下で操作が完了するまでにどのくらいの時間がかかるかを実験し、計測することです。 たとえば、静的パーティション分割は、少数のコアしか持たないマルチコア コンピューターでは速度が飛躍的に向上することがありますが、比較的多くのコアを持つコンピューターでは速度が低下することがあります。

次の表に、<xref:System.Collections.Concurrent.Partitioner.Create%2A> メソッドで使用できるオーバーロードを示します。 これらのパーティショナーは、PLINQ または <xref:System.Threading.Tasks.Task> での使用に限定されるわけではありません。 これらのパーティショナーは、任意のカスタム parallel コンストラクトでも使用できます。

|オーバーロード|負荷分散を使用する|
|--------------|-------------------------|
|<xref:System.Collections.Concurrent.Partitioner.Create%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%29>|Always (常に)|
|<xref:System.Collections.Concurrent.Partitioner.Create%60%601%28%60%600%5B%5D%2CSystem.Boolean%29>|ブール型の引数を true と指定した場合|
|<xref:System.Collections.Concurrent.Partitioner.Create%60%601%28System.Collections.Generic.IList%7B%60%600%7D%2CSystem.Boolean%29>|ブール型の引数を true と指定した場合|
|<xref:System.Collections.Concurrent.Partitioner.Create%28System.Int32%2CSystem.Int32%29>|Never|
|<xref:System.Collections.Concurrent.Partitioner.Create%28System.Int32%2CSystem.Int32%2CSystem.Int32%29>|Never|
|<xref:System.Collections.Concurrent.Partitioner.Create%28System.Int64%2CSystem.Int64%29>|Never|
|<xref:System.Collections.Concurrent.Partitioner.Create%28System.Int64%2CSystem.Int64%2CSystem.Int64%29>|Never|

### <a name="configuring-static-range-partitioners-for-parallelforeach"></a>Parallel.ForEach 用の静的範囲パーティショナーの構成

<xref:System.Threading.Tasks.Parallel.For%2A> ループでは、ループの本体がデリゲートとしてメソッドに提供されます。 このデリゲートを呼び出すコストは、仮想メソッドの呼び出しとほぼ同じです。 シナリオによっては、並列ループの本体が小さく、各ループ反復でデリゲートを呼び出すコストが膨大になることがあります。 そのような状況では、いずれかの <xref:System.Collections.Concurrent.Partitioner.Create%2A> オーバーロードを使用して、ソース要素に対する範囲パーティション分割の <xref:System.Collections.Generic.IEnumerable%601> を作成できます。 その後、この範囲のコレクションを、本体が通常の <xref:System.Threading.Tasks.Parallel.ForEach%2A> ループで構成される `for` メソッドに渡すことができます。 この方法の利点は、デリゲートを呼び出すコストが、要素ごとに 1 回ではなく、範囲ごとに 1 回しか発生しないことです。 基本的なパターンを次の例に示します。

[!code-csharp[TPL_Partitioners#01](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_partitioners/cs/partitioner01.cs#01)]
[!code-vb[TPL_Partitioners#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_partitioners/vb/partitionercreate01.vb#01)]

ループ内のすべてのスレッドは、指定されたサブ範囲の開始インデックス値と終了インデックス値を含む独自の <xref:System.Tuple%602> を受け取ります。 内側の `for` ループでは、`fromInclusive` 値および `toExclusive` 値を使用して、配列または <xref:System.Collections.IList> を直接ループ処理します。

<xref:System.Collections.Concurrent.Partitioner.Create%2A> オーバーロードのいずれかを使用すると、パーティションのサイズと、パーティションの数を指定できます。 このオーバーロードは、要素ごとの作業がきわめて少なく、要素ごとに 1 回の仮想メソッド呼び出しでもパフォーマンスに大きな影響が及ぶシナリオで使用できます。

## <a name="custom-partitioners"></a>カスタム パーティショナー

シナリオによっては、独自のパーティショナーを実装するのが適切か、または必須である場合があります。 たとえば、クラスの内部構造に関する知識に基づいて、カスタム コレクション クラスを既定のパーティショナーよりも効率的にパーティション分割できる場合があります。 または、ソース コレクションの異なる場所にある要素を処理するのにかかる時間についての知識に基づいて、可変サイズの範囲パーティションを作成する必要がある場合があります。

基本的なカスタム パーティショナーを作成するには、<xref:System.Collections.Concurrent.Partitioner%601?displayProperty=nameWithType> からクラスを派生させ、次の表に示すように仮想メソッドをオーバーライドします。

|||
|-|-|
|<xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A>|このメソッドは、メイン スレッドによって 1 回呼び出され、IList(IEnumerator(TSource)) を返します。 ループまたはクエリ内の各ワーカー スレッドでは、リスト上で `GetEnumerator` を呼び出して、個別のパーティションに対する <xref:System.Collections.Generic.IEnumerator%601> を取得できます。|
|<xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A>|`true` を実装した場合は <xref:System.Collections.Concurrent.Partitioner%601.GetDynamicPartitions%2A> を返し、それ以外の場合は `false` を返します。|
|<xref:System.Collections.Concurrent.Partitioner%601.GetDynamicPartitions%2A>|<xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A> が `true` の場合、このメソッドを必要に応じて <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A> の代わりに呼び出すことができます。|

結果が並べ替え可能である必要がある場合、または要素へのインデックス付きアクセスが必要な場合、<xref:System.Collections.Concurrent.OrderablePartitioner%601?displayProperty=nameWithType> から派生させて、次の表に示すように仮想メソッドをオーバーライドします。

|||
|-|-|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetPartitions%2A>|このメソッドは、メイン スレッドによって 1 回呼び出され、`IList(IEnumerator(TSource))` を返します。 ループまたはクエリ内の各ワーカー スレッドでは、リスト上で `GetEnumerator` を呼び出して、個別のパーティションに対する <xref:System.Collections.Generic.IEnumerator%601> を取得できます。|
|<xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A>|`true` を実装した場合は <xref:System.Collections.Concurrent.OrderablePartitioner%601.GetDynamicPartitions%2A> を返し、それ以外の場合は false を返します。|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetDynamicPartitions%2A>|通常は <xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderableDynamicPartitions%2A> を単純に呼び出します。|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderableDynamicPartitions%2A>|<xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A> が `true` の場合、このメソッドを必要に応じて <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A> の代わりに呼び出すことができます。|

次の表に、3 種類の負荷分散パーティショナーで <xref:System.Collections.Concurrent.OrderablePartitioner%601> クラスを実装する方法の詳細を示します。

|メソッド/プロパティ|負荷分散なしの IList/配列|負荷分散ありの IList/配列|IEnumerable|
|----------------------|-------------------------------------------|----------------------------------------|-----------------|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderablePartitions%2A>|範囲パーティション分割を使用|指定された partitionCount のリストに最適化されたチャンク パーティション分割を使用|静的な数のパーティションを作成することにより、チャンク パーティション分割を使用|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderableDynamicPartitions%2A?displayProperty=nameWithType>|サポートしていない機能にアクセスしたときの例外をスロー|リストおよび動的なパーティションに最適化されたチャンク パーティション分割を使用|動的な数のパーティションを作成することにより、チャンク パーティション分割を使用|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.KeysOrderedInEachPartition%2A>|`true` を返します。|`true` を返します。|`true` を返します。|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.KeysOrderedAcrossPartitions%2A>|`true` を返します。|`false` を返します。|`false` を返します。|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.KeysNormalized%2A>|`true` を返します。|`true` を返します。|`true` を返します。|
|<xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A>|`false` を返します。|`true` を返します。|`true` を返します。|

### <a name="dynamic-partitions"></a>動的パーティション

パーティショナーを <xref:System.Threading.Tasks.Parallel.ForEach%2A> メソッドで使用する場合、動的な数のパーティションを返すことができる必要があります。 これは、パーティショナーがループの実行中の任意の時点で、新しいパーティションの列挙子をオンデマンドで供給できることを意味します。 基本的に、ループで新しい並列タスクを追加するたびに、そのタスク用の新しいパーティションが要求されます。 データが順序付け可能である必要がある場合は、<xref:System.Collections.Concurrent.OrderablePartitioner%601?displayProperty=nameWithType> から派生させて、各パーティション内の各項目に一意のインデックスが割り当てられるようにします。

詳細および使用例については、[「方法: 動的パーティションを実装する」](../../../docs/standard/parallel-programming/how-to-implement-dynamic-partitions.md)を参照してください。

### <a name="contract-for-partitioners"></a>パーティショナーのコントラクト

カスタム パーティショナーを実装するときは、次のガイドラインに従って、PLINQ および TPL 内の <xref:System.Threading.Tasks.Parallel.ForEach%2A> との適切な相互作用を保証します。

- <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A> が `partitionsCount` に 0 以下の引数を指定して呼び出された場合は、<xref:System.ArgumentOutOfRangeException> をスローします。 PLINQ および TPL が 0 と等しい `partitionCount` を渡すことはありませんが、このような場合に備えることをお勧めします。

- <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A> および <xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderablePartitions%2A> では、常に `partitionsCount` 個のパーティションを返す必要があります。 パーティショナーがデータを使い果たし、要求された数のパーティションを作成できない場合、このメソッドでは残りのパーティションのそれぞれについて、空の列挙子を返す必要があります。 それ以外の場合、PLINQ と TPL はいずれも <xref:System.InvalidOperationException> をスローします。

- <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A>、<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderablePartitions%2A>、<xref:System.Collections.Concurrent.Partitioner%601.GetDynamicPartitions%2A>、<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderableDynamicPartitions%2A> では、`null` (Visual Basic では `Nothing`) を返さないようにします。 返した場合、PLINQ または TPL は <xref:System.InvalidOperationException> をスローします。

- パーティションを返すメソッドでは、データ ソースを完全かつ一意に列挙できるパーティションを常に返す必要があります。 パーティショナーの設計により特別に必要な場合を除いて、データ ソース内の項目が重複したり、スキップされたりすることがないようにします。 この規則に従わないと、出力順序が乱れる場合があります。

- 次のブール型の getter では、常に以下の値を正確に返して、出力順序が乱れないようにする必要があります。

  - `KeysOrderedInEachPartition`: 各パーティションは、昇順のキー インデックスを持つ要素を返します。

  - `KeysOrderedAcrossPartitions`: 返されるすべてのパーティションについて、パーティション *i* のキー インデックスは、パーティション *i*-1 のキー インデックスよりも大きくなります。

  - `KeysNormalized`: すべてのキー インデックスは、0 から始まりギャップなしで単調に増加します。

- どのインデックスも一意である必要があります。 インデックスを重複させることはできません。 この規則に従わないと、出力順序が乱れる場合があります。

- どのインデックスも負数以外である必要があります。 この規則に従わないと、PLINQ または TPL が例外をスローする場合があります。

## <a name="see-also"></a>参照

- [並列プログラミング](../../../docs/standard/parallel-programming/index.md)
- [方法: 動的パーティションを実装する](../../../docs/standard/parallel-programming/how-to-implement-dynamic-partitions.md)
- [方法: 静的パーティション分割用にパーティショナーを実装する](../../../docs/standard/parallel-programming/how-to-implement-a-partitioner-for-static-partitioning.md)
