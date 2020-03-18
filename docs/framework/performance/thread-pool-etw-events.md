---
title: スレッド プール ETW イベント
ms.date: 03/30/2017
helpviewer_keywords:
- thread pool events [.NET Framework]
- ETW, thread pool events (CLR)
ms.assetid: f2a21e3a-3b6c-4433-97f3-47ff16855ecc
ms.openlocfilehash: 249d0607ddd280bcb4e9cf3ef34b28ff8ada3b04
ms.sourcegitcommit: 43d10ef65f0f1fd6c3b515e363bde11a3fcd8d6d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2020
ms.locfileid: "78240494"
---
# <a name="thread-pool-etw-events"></a>スレッド プール ETW イベント
これらのイベントは、ワーカー スレッドと I/O スレッドに関する情報を収集します。  
  
 スレッド プール イベントには 2 つのグループがあります。  
  
- [ワーカー スレッド プール イベント](#worker-thread-pool-events)は、アプリケーションがどのようにスレッド プールを使用するかに関する情報と、コンカレンシー制御におけるワークロードの効果に関する情報を提供します。  
  
- [I/O スレッド プール イベント](#io-thread-events)は、スレッド プールで作成、無効化、無効化解除、または終了した I/O スレッドに関する情報を提供します。  

## <a name="worker-thread-pool-events"></a>ワーカー スレッド プール イベント
 これらのイベントは、ランタイムのワーカー スレッドのプールに関連付けられており、スレッド イベントに関する通知 (スレッドが作成されたり停止されたりした場合など) を提供します。 ワーカー スレッド プールは、スレッドの数が計測されたスループットに基づいて計算されるアダプティブ アルゴリズムを使用して、コンカレンシー制御を実行します。 ワーカー スレッド プール イベントを使用すると、アプリケーションで使用されるスレッド プールの様子や特定のワークロードがコンカレンシー制御に与える影響などを理解することができます。  
  
### <a name="threadpoolworkerthreadstart-and-threadpoolworkerthreadstop"></a>ThreadPoolWorkerThreadStart および ThreadPoolWorkerThreadStop  
 次の表に、これらのイベントのキーワードとレベルを示します。 (詳細については、「 [CLR ETW Keywords and Levels](clr-etw-keywords-and-levels.md)」を参照してください)。  
  
|イベントを発生させるキーワード|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|情報提供 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|いつ発生するか|  
|-|-|-|  
|`ThreadPoolWorkerThreadStart`|50|ワーカー スレッドが作成された。|  
|`ThreadPoolWorkerThreadStop`|51|ワーカー スレッドが停止された。|  
|`ThreadPoolWorkerThreadRetirementStart`|52|ワーカー スレッドが無効にされた。|  
|`ThreadPoolWorkerThreadRetirementStop`|53|提供終了になったワーカー スレッドが再びアクティブになった。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|データ型|Description|  
|----------------|---------------|-----------------|  
|ActiveWorkerThreadCount|win:UInt32|作業の処理に使用可能なワーカー スレッド (既に作業の処理中のもの含む) の数。|  
|RetiredWorkerThreadCount|win:UInt32|作業の処理に使用できないものの、後にさらに多くのスレッドが必要になった場合に備えて予約されているワーカー スレッドの数。|  
|ClrInstanceID|Win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  
  
### <a name="threadpoolworkerthreadadjustment"></a>ThreadPoolWorkerThreadAdjustment  
 これらのスレッド プール イベントは、スレッドの挿入 (コンカレンシー制御) アルゴリズムの動作を理解したりデバッグしたりするための情報を提供します。 この情報は、ワーカー スレッド プールによって内部で使用されます。  
  
#### <a name="threadpoolworkerthreadadjustmentsample"></a>ThreadPoolWorkerThreadAdjustmentSample  
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|情報提供 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|Description|  
|-----------|--------------|-----------------|  
|`ThreadPoolWorkerThreadAdjustmentSample`|54|1 つのサンプルの情報のコレクションを参照します。つまり、特定のコンカレンシー レベルの特定の時刻におけるスループットの測定値です。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|データ型|Description|  
|----------------|---------------|-----------------|  
|スループット|win:Double|時間の単位あたりの入力候補の数です。|  
|ClrInstanceID|Win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  
  
#### <a name="threadpoolworkerthreadadjustmentadjustment"></a>ThreadPoolWorkerThreadAdjustmentAdjustment  
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|情報提供 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|Description|  
|-----------|--------------|-----------------|  
|`ThreadPoolWorkerThreadAdjustmentAdjustment`|55|スレッドの挿入 (山登り法) アルゴリズムが、コンカレンシー レベルに変更があったと判断した場合に、コントロールの変更を記録します。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|データ型|Description|  
|----------------|---------------|-----------------|  
|AverageThroughput|win:Double|計測のサンプルの平均のスループット。|  
|NewWorkerThreadCount|win:UInt32|新しいアクティブなワーカー スレッド数。|  
|理由|win:UInt32|調整の理由。<br /><br /> 0x00 - ウォーム アップ。<br /><br /> 0x01 - 初期化。<br /><br /> 0x02 - ランダムな移動。<br /><br /> 0x03 - 上昇移動。<br /><br /> 0x04 - 変更点。<br /><br /> 0x05 - 安定化。<br /><br /> 0x06 - 不足。<br /><br /> 0x07 - スレッドのタイムアウト。|  
|ClrInstanceID|Win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  
  
#### <a name="threadpoolworkerthreadadjustmentstats"></a>ThreadPoolWorkerThreadAdjustmentStats  
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|情報提供 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|Description|  
|-----------|--------------|-----------------|  
|`ThreadPoolWorkerThreadAdjustmentStats`|56|スレッド プールに関するデータを収集します。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|データ型|Description|  
|----------------|---------------|-----------------|  
|Duration|win:Double|これらの統計情報が収集される時間数 (秒)。|  
|スループット|win:Double|この間隔中の 1 秒あたりの入力候補の平均数。|  
|ThreadWave|win:Double|内部使用のために予約されています。|  
|ThroughputWave|win:Double|内部使用のために予約されています。|  
|ThroughputErrorEstimate|win:Double|内部使用のために予約されています。|  
|AverageThroughputErrorEstimate|win:Double|内部使用のために予約されています。|  
|ThroughputRatio|win:Double|この間隔中にアクティブなワーカー スレッドの数の変動によって引き起こされる、スループットの相対的な向上。|  
|Confidence|win:Double|ThroughputRatio フィールドの有効性の測定結果。|  
|NewcontrolSetting|win:Double|アクティブなスレッド数の将来のバリエーションのベースラインとして使用するアクティブなワーカー スレッドの数。|  
|NewThreadWaveMagnitude|Win:UInt16|アクティブなスレッド数の、将来のバリエーションの大きさを指定します。|  
|ClrInstanceID|Win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  

## <a name="io-thread-events"></a>I/O スレッド イベント  
 これらのスレッド プール イベントは、I/O スレッド プール (完了ポート) にあるスレッドで発生します。これは非同期です。  
  
### <a name="iothreadcreate_v1"></a>IOThreadCreate_V1  
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|情報提供 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|いつ発生するか|  
|-|-|-|  
|`IOThreadCreate_V1`|44|I/O スレッドがスレッド プールに作成された。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|データ型|Description|  
|----------------|---------------|-----------------|  
|Count|win:UInt64|新しく作成されたスレッドを含む、I/O のスレッドの数です。|  
|NumRetired|win:UInt64|提供終了になったワーカー スレッドの数。|  
|ClrInstanceID|Win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  
  
### <a name="iothreadretire_v1"></a>IOThreadRetire_V1  
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|情報提供 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|いつ発生するか|  
|-----------|--------------|-----------------|  
|`IOThreadRetire_V1`|46|I/O スレッドが、提供終了の候補になる。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|データ型|Description|  
|----------------|---------------|-----------------|  
|Count|win:UInt64|スレッド プールに残っている I/O スレッドの数。|  
|NumRetired|win:UInt64|提供終了になった I/O スレッドの数。|  
|ClrInstanceID|Win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  
  
### <a name="iothreadunretire_v1"></a>IOThreadUnretire_V1  
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|情報提供 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|いつ発生するか|  
|-----------|--------------|-----------------|  
|`IOThreadUnretire_V1`|47|スレッドが提供終了の候補になってから待機期間内に I/O が到着したため I/O スレッドが提供終了解除された。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|データ型|Description|  
|----------------|---------------|-----------------|  
|Count|win:UInt64|これを含む、スレッド プール内の I/O スレッドの数。|  
|NumRetired|win:UInt64|提供終了になった I/O スレッドの数。|  
|ClrInstanceID|Win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  
  
### <a name="iothreadterminate"></a>IOThreadTerminate  
 次の表に、キーワードとレベルを示します。  
  
|イベントを発生させるキーワード|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|情報提供 (4)|  
  
 次の表に、イベント情報を示します。  
  
|Event|イベント ID|いつ発生するか|  
|-----------|--------------|-----------------|  
|`IOThreadTerminate`|45|スレッドプールで i/o スレッドが終了します。|  
  
 次の表に、イベント データを示します。  
  
|フィールド名|データ型|Description|  
|----------------|---------------|-----------------|  
|Count|win:UInt64|スレッド プールに残っている I/O スレッドの数。|  
|NumRetired|win:UInt64|提供終了になった I/O スレッドの数。|  
|ClrInstanceID|Win:UInt16|CLR または CoreCLR のインスタンスの一意の ID。|  
  
## <a name="see-also"></a>参照

- [CLR ETW イベント](clr-etw-events.md)
