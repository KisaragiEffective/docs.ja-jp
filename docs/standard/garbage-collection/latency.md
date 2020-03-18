---
title: 待機モード
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- garbage collection, intrusiveness
- garbage collection, latency modes
ms.assetid: 96278bb7-6eab-4612-8594-ceebfc887d81
ms.openlocfilehash: a8eaf0c80aa32978eead80c51a905cbcd66a537b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74283589"
---
# <a name="latency-modes"></a>待機モード

オブジェクトを再利用するには、ガベージ コレクター (GC) で、アプリケーションで実行中のすべてのスレッドを停止する必要があります。 ガベージ コレクターがアクティブになるまでの期間を、*待機時間*と呼びます。

状況によっては、アプリケーションがデータの取得やコンテンツの表示を行うときなど、重要なときにフル ガベージ コレクションが発生し、パフォーマンスが低下することがあります。 ガベージ コレクターが作業に悪影響を与える度合いを調整するには、<xref:System.Runtime.GCSettings.LatencyMode%2A?displayProperty=nameWithType> プロパティを <xref:System.Runtime.GCLatencyMode?displayProperty=nameWithType> 値のいずれかに設定することができます。

## <a name="low-latency-settings"></a>低待機時間の設定

"低" 待機時間の設定の使用は、ガベージ コレクターによるアプリケーションへの関与が少なくなることを意味します。 ガベージ コレクションはメモリの再利用についてより控え目となります。

<xref:System.Runtime.GCLatencyMode?displayProperty=nameWithType> 列挙体には、待機時間の短い設定として次の 2 つがあります。

- [GCLatencyMode.LowLatency](xref:System.Runtime.GCLatencyMode.LowLatency) では、ジェネレーション 2 のコレクションが停止し、ジェネレーション 0 および 1 コレクションのみが実行されます。 これは、短時間の場合にのみ使用できます。 この設定を長時間にわたって使用すると、システムのメモリが不足してガベージ コレクターがガベージ コレクションをトリガーした場合に、アプリケーションが少しの間停止したり、高速性を必要とする操作が中断されたりすることがあります。 この設定は、ワークステーションのガベージ コレクションでのみ使用できます。

- [GCLatencyMode.SustainedLowLatency](xref:System.Runtime.GCLatencyMode.SustainedLowLatency) では、フォアグラウンドのジェネレーション 2 のコレクションが停止し、ジェネレーション 0、1、およびバックグラウンドのジェネレーション 2 のコレクションのみが実行されます。 これは長時間にわたって使用でき、ワークステーションとサーバーの両方のガベージ コレクションで使用できます。 この設定は、バックグラウンドのガベージ コレクションが無効の場合には使用できません。

待機時間の短い設定になっている場合でも、次の状況ではジェネレーション 2 のガベージ コレクションの停止が解除されます。

- システムがオペレーティング システムからメモリ不足の通知を受け取った場合。

- アプリケーション コードから <xref:System.GC.Collect%2A?displayProperty=nameWithType> メソッドを呼び出し、`generation` パラメーターに 2 を指定してコレクションを実行した場合。

## <a name="scenarios"></a>シナリオ

次の表に、<xref:System.Runtime.GCLatencyMode> の各値を使用するアプリケーション シナリオを示します。

|待機時間モード|アプリケーション シナリオ|
|------------------|---------------------------|
|<xref:System.Runtime.GCLatencyMode.Batch>|ユーザー インターフェイス (UI) 操作のないアプリケーションの場合、またはサーバー側の操作の場合に使用します。<br /><br />バックグラウンドのガベージ コレクションが無効にされている場合、これがワークステーションとサーバーのガベージ コレクションの既定のモードとなります。 また、<xref:System.Runtime.GCLatencyMode.Batch> モードにより [gcConcurrent](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) 設定がオーバーライドされます。つまり、バックグラウンドまたは同時実行コレクションが阻止されます。|
|<xref:System.Runtime.GCLatencyMode.Interactive>|UI を持つほとんどのアプリケーションの場合に使用します。<br /><br />これは、ワークステーションとサーバーのガベージ コレクションの既定のモードです。 ただし、アプリがホストされている場合は、ホスト プロセスのガベージ コレクター設定が優先されます。|
|<xref:System.Runtime.GCLatencyMode.LowLatency>|ガベージ コレクターからの割り込みにより重大な影響を受け、高速性を必要とする、短期間の操作を実行するアプリケーションの場合に使用します。 たとえば、アニメーションのレンダリングを行うアプリケーションやデータの取得機能などがあります。|
|<xref:System.Runtime.GCLatencyMode.SustainedLowLatency>|ガベージ コレクターからの割り込みにより重大な影響を受け、高速性を必要とする、さほど処理時間を必要としないものの長時間になる可能性のある操作を実行するアプリケーションの場合に使用します。 たとえば、取引時間中に市場データの変化に応じて迅速な応答時間を必要とするアプリケーションなどがあります。<br /><br />このモードでは、マネージド ヒープのサイズが他のモードより大きくなります。 マネージド ヒープは最適化されないため、断片化の割合が高くなる可能性があります。 十分なメモリが使用可能であることを確認してください。|

## <a name="guidelines-for-using-low-latency"></a>低待機時間の使用に関するガイドライン

[GCLatencyMode.LowLatency](xref:System.Runtime.GCLatencyMode.LowLatency) モードを使用する場合は、次のガイドラインを検討してください。

- 待機時間を短くする期間は、できるだけ短くします。

- 待機時間を短くする期間中は、大量のメモリを割り当てないようにします。 ガベージ コレクションによって再利用されるオブジェクトの数が少なくなるため、メモリ不足の通知が発生する可能性があります。

- 低待機時間モードの間は、新しい割り当ての数、特に大きなオブジェクト ヒープや固定されたオブジェクトへの割り当ての数を最小限に抑えます。

- 割り当てられる可能性のあるスレッドに注意します。 <xref:System.Runtime.GCSettings.LatencyMode%2A> プロパティの設定はプロセス全体に適用されるため、<xref:System.OutOfMemoryException> 例外は割り当てられている任意のスレッドで生成できます。

- 低待機時間コードを制約された実行領域にラップします。 詳細については、「[制約された実行領域](../../../docs/framework/performance/constrained-execution-regions.md)」を参照してください。

- 待機時間を短くする期間中でも、<xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%29?displayProperty=nameWithType> メソッドを呼び出せばジェネレーション 2 のガベージ コレクションを強制できます。

## <a name="see-also"></a>参照

- <xref:System.GC?displayProperty=nameWithType>
- [発生したコレクション](../../../docs/standard/garbage-collection/induced.md)
- [ガベージ コレクション](../../../docs/standard/garbage-collection/index.md)
