---
title: IHostTask インターフェイス
ms.date: 03/30/2017
api_name:
- IHostTask
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTask
helpviewer_keywords:
- IHostTask interface [.NET Framework hosting]
ms.assetid: a71dbbd5-64b8-47eb-9f03-8e8c85fbe2bc
topic_type:
- apiref
ms.openlocfilehash: 18dfee606f3d41229aa58a5b4bb9380b87c4efa5
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73121395"
---
# <a name="ihosttask-interface"></a>IHostTask インターフェイス
共通言語ランタイム (CLR) がホストと通信してタスクを管理できるようにするメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[Alert メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttask-alert-method.md)|現在の `IHostTask` インスタンスによって表されるタスクをホストがスリープ解除するように要求します。これにより、タスクを中止できます。|  
|[GetPriority メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttask-getpriority-method.md)|現在の `IHostTask` インスタンスによって表されるタスクのスレッド優先度レベルを取得します。|  
|[Join メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttask-join-method.md)|現在の `IHostTask` インスタンスによって表されるタスクが完了するまで、または指定された時間間隔が経過するか、または[IHostTask:: Alert](../../../../docs/framework/unmanaged-api/hosting/ihosttask-alert-method.md)が呼び出されるまで、呼び出し元のタスクをブロックします。|  
|[SetCLRTask メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttask-setclrtask-method.md)|[ICLRTask インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtask-interface.md)インスタンスを現在の `IHostTask` インスタンスに関連付けます。|  
|[SetPriority メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttask-setpriority-method.md)|現在の `IHostTask` インスタンスによって表されるタスクのスレッドの優先度レベルをホストが調整するように要求します。|  
|[Start メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttask-start-method.md)|現在の `IHostTask` インスタンスによって表されるタスクを中断状態からライブ状態に移行するようホストに要求します。このとき、コードを実行できます。|  
  
## <a name="remarks"></a>Remarks  
 CLR は、`IHostTask` によって定義されたメソッドを呼び出して、タスクを開始したり、スレッドの優先度レベルを設定したりします。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRTask インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtask-interface.md)
- [ICLRTaskManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtaskmanager-interface.md)
- [IHostTaskManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-interface.md)
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
