---
title: ICLRTask インターフェイス
ms.date: 03/30/2017
api_name:
- ICLRTask
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRTask
helpviewer_keywords:
- ICLRTask interface [.NET Framework hosting]
ms.assetid: b3a44df3-578a-4451-b55e-70c8e7695f5e
topic_type:
- apiref
ms.openlocfilehash: c4f27a73022b0495b2772c0485c14a1b007dc883
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73132644"
---
# <a name="iclrtask-interface"></a>ICLRTask インターフェイス
ホストが共通言語ランタイム (CLR) の要求を行うことができるようにするメソッド、または関連付けられたタスクについて CLR に通知を提供するためのメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[Abort メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask-abort-method.md)|現在の `ICLRTask` インスタンスが表すタスクを CLR が中止するように要求します。|  
|[ExitTask メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask-exittask-method.md)|現在の `ICLRTask` インスタンスに関連付けられているタスクが終了していることを CLR に通知し、タスクを正常にシャットダウンしようとします。|  
|[GetMemStats メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask-getmemstats-method.md)|現在の `ICLRTask` インスタンスによって表されるタスクによるメモリリソースの使用に関する統計情報を取得します。|  
|[LocksHeld メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask-locksheld-method.md)|タスクに現在保持されているロックの数を取得します。|  
|[NeedsPriorityScheduling メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask-needspriorityscheduling-method.md)|現在の `ICLRTask` インスタンスによって表されるタスクを再スケジュールするために、優先順位の高いホストを割り当てる必要があるかどうかを示す値を取得します。|  
|[Reset メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask-reset-method.md)|ホストがタスクを完了したことを CLR に通知し、CLR が現在の `ICLRTask` インスタンスを再利用して別のタスクを表すことができるようにします。|  
|[RudeAbort メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask-rudeabort-method.md)|CLR が、ファイナライザーが実行されることを保証せずに、現在の `ICLRTask` インスタンスによって表されるタスクをすぐに中止します。|  
|[SetTaskIdentifier メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask-settaskidentifier-method.md)|現在の `ICLRTask` インスタンスによって表されるタスクの一意識別子を設定します。この識別子はデバッグに使用されます。|  
|[SwitchIn メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask-switchin-method.md)|現在の `ICLRTask` インスタンスによって表されるタスクが操作可能な状態であることを CLR に通知します。|  
|[SwitchOut メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask-switchout-method.md)|現在の `ICLRTask` インスタンスによって表されるタスクが操作可能な状態ではなくなったことを CLR に通知します。|  
|[YieldTask メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask-yieldtask-method.md)|CLR がプロセッサ時間を他のタスクで使用できるようにすることを要求します。 CLR では、処理時間を生成できる状態にタスクが配置されるという保証はありません。|  
  
## <a name="remarks"></a>Remarks  
 `ICLRTask` は、CLR のタスクを表します。 コードの実行中はいつでも、実行中または実行の待機中のいずれかのタスクを記述できます。 ホストは `ICLRTask::SwitchIn` メソッドを呼び出して、現在の `ICLRTask` インスタンスが表すタスクが操作可能な状態になったことを CLR に通知します。 `ICLRTask::SwitchIn`の呼び出しの後、ホストは任意のオペレーティングシステムスレッドでタスクをスケジュールできます。ただし、ランタイムにスレッドアフィニティが必要な場合は、 [IHostTaskManager:: BeginThreadAffinity](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-beginthreadaffinity-method.md)と[IHostTaskManager:: の呼び出しで指定されます。EndThreadAffinity](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-endthreadaffinity-method.md)メソッド。 しばらくすると、オペレーティングシステムは、スレッドからタスクを削除して、実行されていない状態にする可能性があります。 たとえば、タスクが同期プリミティブでブロックされた場合や、i/o 操作が完了するまで待機している場合に発生することがあります。 ホストは[Switchout](../../../../docs/framework/unmanaged-api/hosting/iclrtask-switchout-method.md)を呼び出して、現在の `ICLRTask` インスタンスによって表されるタスクが操作可能な状態ではなくなったことを CLR に通知します。  
  
 タスクは通常、コード実行の終了時に終了します。 その時点で、ホストは `ICLRTask::ExitTask` を呼び出して、関連付けられている `ICLRTask`を破棄します。 ただし、`ICLRTask::Reset`の呼び出しを使用してタスクをリサイクルすることもできます。これにより、`ICLRTask` インスタンスを再度使用できます。 この方法では、インスタンスを繰り返し作成および破棄するオーバーヘッドを回避できます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRTaskManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtaskmanager-interface.md)
- [IHostTask インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihosttask-interface.md)
- [IHostTaskManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-interface.md)
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
- [ICLRTask2 インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtask2-interface.md)
