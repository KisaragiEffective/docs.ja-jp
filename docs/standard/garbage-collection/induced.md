---
title: 発生したコレクション
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- garbage collection, forced
ms.assetid: 019008fe-4708-4e65-bebf-04fd9941e149
ms.openlocfilehash: 604b49ef577a46204b523ebf5a8575a30b81635e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73120927"
---
# <a name="induced-collections"></a>発生したコレクション
ほとんどの場合、コレクションの実行に最適なタイミングはガベージ コレクターが判断できるので、ガベージ コレクターに任せるのが良い方法です。 ただし、ごくまれに、強制的にコレクションを実行するとアプリケーションのパフォーマンスが向上する場合があります。 このような場合は、<xref:System.GC.Collect%2A?displayProperty=nameWithType> メソッドを使用してガベージ コレクションを強制的に実行できます。  
  
 アプリケーションのコードの特定の位置で、使用しているメモリ量が大きく減少する場合は、<xref:System.GC.Collect%2A?displayProperty=nameWithType> メソッドを使用します。 たとえば、複数のコントロールのある複雑なダイアログ ボックスを使用するアプリケーションでは、ダイアログ ボックスを閉じるときに <xref:System.GC.Collect%2A> を呼び出すと、ダイアログ ボックスの使用メモリが直ちに再利用されてパフォーマンスが向上する可能性があります。 適切でない回数でガベージ コレクターがオブジェクトの再利用を試みるとパフォーマンスが低下する場合があるので、アプリケーションではあまり頻繁にガベージ コレクションを強制しないでください。 次のセクションで説明するように、コレクションの効果がある場合にのみ、<xref:System.GCCollectionMode.Optimized?displayProperty=nameWithType> のメソッドに対して収集する <xref:System.GC.Collect%2A> の列挙値を指定できます。  
  
## <a name="gc-collection-mode"></a>GC コレクション モード  
 <xref:System.GC.Collect%2A?displayProperty=nameWithType> 値を含む <xref:System.GCCollectionMode> メソッド オーバーロードの 1 つを使用して、強制的コレクションの動作を次のように指定できます。  
  
|`GCCollectionMode` の値|[説明]|  
|------------------------------|-----------------|  
|<xref:System.GCCollectionMode.Default>|実行中のバージョンの .NET の既定のガベージ コレクション設定を使用します。|  
|<xref:System.GCCollectionMode.Forced>|直ちにガベージ コレクションを強制的に実行します。 これは、<xref:System.GC.Collect?displayProperty=nameWithType> オーバーロードを呼び出すのと同じです。 結果として、すべてのジェネレーションのフル ブロッキング コレクションになります。<br /><br /> また、直ちにフル ブロッキング ガベージ コレクションを強制的に実行する前に、<xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode%2A?displayProperty=nameWithType> プロパティを <xref:System.Runtime.GCLargeObjectHeapCompactionMode.CompactOnce?displayProperty=nameWithType> に設定して、大きなオブジェクト ヒープを圧縮することもできます。|  
|<xref:System.GCCollectionMode.Optimized>|オブジェクトを再利用するのに現在が最適なときかどうかをガベージ コレクターが判断できるようにします。<br /><br /> ガベージ コレクターは、コレクションの実行を正当化できるほど効果がないと判断して、オブジェクトを再利用せずに戻る場合があります。|  
  
## <a name="background-or-blocking-collections"></a>バックグラウンドまたはブロッキング コレクション  
 <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%29?displayProperty=nameWithType> メソッド オーバーロードを呼び出して、発生するコレクションがブロッキング コレクションであるかどうかを指定できます。 実行されるコレクションの型は、メソッドの `mode` と `blocking` のパラメーターの組み合わせによって異なります。 `mode` は <xref:System.GCCollectionMode> 列挙体のメンバーです。`blocking` は <xref:System.Boolean> 値です。 `mode` と `blocking` 引数の相互作用を次の表にまとめます。  
  
|`mode`|`blocking` = `true`|`blocking` = `false`|  
|------------|--------------------------|---------------------------|  
|<xref:System.GCCollectionMode.Forced> または <xref:System.GCCollectionMode.Default>|ブロッキング コレクションはできるだけ早く実行されます。 バックグラウンド コレクションが実行中でジェネレーションが 0 または 1 の場合、<xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%29> メソッドは直ちにブロッキング コレクションをトリガーし、コレクションが終了すると制御を戻します。 バックグラウンド コレクションが実行中で `generation` パラメーターが 2 の場合、メソッドはバックグラウンド コレクションの終了を待機し、ジェネレーション 2 のブロッキング コレクションをトリガーして、制御を戻します。|コレクションはできるだけ早く実行されます。 <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%29> メソッドはバックグラウンド コレクションを要求しますが、それは保証されず、状況によってはブロッキング コレクションが実行される場合もあります。 バックグラウンド コレクションが既に実行中の場合、メソッドはすぐに制御を返します。|  
|<xref:System.GCCollectionMode.Optimized>|ガベージ コレクターおよび `generation` パラメーターの状態によっては、ブロッキング コレクションが実行される場合があります。 ガベージ コレクターは最適なパフォーマンスを提供しようとします。|ガベージ コレクターの状態によっては、コレクションが実行される場合があります。 <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%29> メソッドはバックグラウンド コレクションを要求しますが、それは保証されず、状況によってはブロッキング コレクションが実行される場合もあります。 ガベージ コレクターは最適なパフォーマンスを提供しようとします。 バックグラウンド コレクションが既に実行中の場合、メソッドはすぐに制御を返します。|  
  
## <a name="see-also"></a>参照

- [待機モード](../../../docs/standard/garbage-collection/latency.md)
- [ガベージ コレクション](../../../docs/standard/garbage-collection/index.md)
