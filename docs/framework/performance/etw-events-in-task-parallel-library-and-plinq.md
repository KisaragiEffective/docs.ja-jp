---
title: タスク並列ライブラリおよび PLINQ での ETW イベント
ms.date: 03/30/2017
helpviewer_keywords:
- tasks, ETW events
ms.assetid: 87a9cff5-d86f-4e44-a06e-d12764d0dce2
ms.openlocfilehash: 93fcd3215bdcbb30960f19e23ae15f32bb9ddd84
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75716094"
---
# <a name="etw-events-in-task-parallel-library-and-plinq"></a>タスク並列ライブラリおよび PLINQ での ETW イベント

タスク並列ライブラリおよび PLINQ は、どちらも Windows イベント トレーシング (ETW) イベントを生成します。ETW イベントは、Windows パフォーマンス アナライザーなどのツールを使用して、アプリケーションのプロファイルやトラブルシューティングに使用できます。 ただし、ほとんどのシナリオでは、並列アプリケーションコードをプロファイリングする最良の方法は、Visual Studio で[同時実行ビジュアライザー](/visualstudio/profiling/concurrency-visualizer)を使用することです。

## <a name="task-parallel-library-etw-events"></a>タスク並列ライブラリの ETW イベント

EVENT_HEADER 構造体では、<xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType>、および <xref:System.Threading.Tasks.Parallel.Invoke%2A?displayProperty=nameWithType> で生成されたイベントの ProviderId GUID は、次のとおりです。

`0x2e5dba47, 0xa3d2, 0x4d16, 0x8e, 0xe0, 0x66, 0x71, 0xff, 0xdc, 0xd7, 0xb5`

### <a name="parallel-loop-begin"></a>並列ループの開始

EVENT_DESCRIPTOR.Task = 1

EVENT_DESCRIPTOR.Id = 1

#### <a name="user-data"></a>ユーザー データ

|**名前**|**型**|**説明**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|ループを開始した TaskScheduler の ID。|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|ループを開始したタスクの ID。|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|入れ子と fork/join セマンティクスでのイベントのペアを示すために使用される一意の識別子。|
|OperationType|<xref:System.Int32?displayProperty=nameWithType>|ループの種類:<br /><br /> 1 = ParallelInvoke<br /><br /> 2 = ParallelFor<br /><br /> 3 = ParallelForEach|
|InclusiveFrom|<xref:System.Int64?displayProperty=nameWithType>|ループ カウンターの開始値|
|ExclusiveTo|<xref:System.Int64?displayProperty=nameWithType>|ループ カウンターの終了値|

### <a name="parallel-loop-end"></a>並列ループの終了
 EVENT_DESCRIPTOR.Task = 2

 EVENT_DESCRIPTOR.Id = 2

#### <a name="user-data"></a>ユーザー データ

|**名前**|**型**|**説明**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|ループを開始した TaskScheduler の ID。|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|ループを開始したタスクの ID。|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|入れ子と fork/join セマンティクスでのイベントのペアを示すために使用される一意の識別子。|
|totalIterations|<xref:System.Int64?displayProperty=nameWithType>|イテレーションの合計数|

### <a name="parallel-invoke-begin"></a>並列呼び出しの開始
 EVENT_DESCRIPTOR.Task = 3

 EVENT_DESCRIPTOR.Id = 3

#### <a name="user-data"></a>ユーザー データ

|**名前**|**型**|**説明**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|ループを開始した TaskScheduler の ID。|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|ループを開始したタスクの ID。|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|入れ子と fork/join セマンティクスでのイベントのペアを示すために使用される一意の識別子。|
|totalIterations|<xref:System.Int64?displayProperty=nameWithType>|イテレーションの合計数|
|operationType|<xref:System.Int32?displayProperty=nameWithType>|ループの種類:<br /><br /> 1 = ParallelInvoke<br /><br /> 2 = ParallelFor<br /><br /> 3 = ParallelForEach|
|ActionCount|<xref:System.Int32?displayProperty=nameWithType>|並列呼び出しで実行されるアクションの数。|

### <a name="parallel-invoke-end"></a>並列呼び出しの終了
 EVENT_DESCRIPTOR.Task = 4

 EVENT_DESCRIPTOR.Id = 4

#### <a name="user-data"></a>ユーザー データ

|**名前**|**型**|**説明**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|ループを開始した TaskScheduler の ID。|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|ループを開始したタスクの ID。|
|ForkJoinContextID|<xref:System.Int32?displayProperty=nameWithType>|入れ子と fork/join セマンティクスでのイベントのペアを示すために使用される一意の識別子。|

## <a name="plinq-etw-events"></a>PLINQ ETW イベント
 PLINQ の EVENT_HEADER.ProviderId GUID は次のとおりです。

`0x159eeeec, 0x4a14, 0x4418, 0xa8, 0xfe, 0xfa, 0xab, 0xcd, 0x98, 0x78, 0x87`

### <a name="parallel-query-begin"></a>並列クエリの開始
 EVENT_DESCRIPTOR.Task = 1

 EVENT_DESCRIPTOR.Id = 1

#### <a name="user-data"></a>ユーザー データ

|**名前**|**型**|**説明**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|ループを開始した TaskScheduler の ID。|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|ループを開始したタスクの ID。|
|QueryID|<xref:System.Int32?displayProperty=nameWithType>|一意のクエリの識別子。|

### <a name="parallel-query-end"></a>並列クエリの終了
 EVENT_DESCRIPTOR.Task = 2

 EVENT_DESCRIPTOR.Id = 2

#### <a name="user-data"></a>ユーザー データ

|**名前**|**型**|**説明**|
|--------------|--------------|---------------------|
|OriginatingTaskSchedulerID|<xref:System.Int32?displayProperty=nameWithType>|ループを開始した TaskScheduler の ID。|
|OriginatingTaskID|<xref:System.Int32?displayProperty=nameWithType>|ループを開始したタスクの ID。|
|QueryID|<xref:System.Int32?displayProperty=nameWithType>|一意のクエリの識別子。|

## <a name="see-also"></a>関連項目

- [.NET Framework の ETW イベント](etw-events.md)
- [タスク並列ライブラリ (TPL)](../../standard/parallel-programming/task-parallel-library-tpl.md)
- [Parallel LINQ (PLINQ)](../../standard/parallel-programming/parallel-linq-plinq.md)
