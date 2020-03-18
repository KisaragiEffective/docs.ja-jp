---
title: IHostTaskManager インターフェイス
ms.date: 03/30/2017
api_name:
- IHostTaskManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager
helpviewer_keywords:
- IHostTaskManager interface [.NET Framework hosting]
ms.assetid: 4a0b05b9-3ef1-4607-b7c8-bd4dd43647a0
topic_type:
- apiref
ms.openlocfilehash: 470e2ac06f433dc12d66f6cac97337a6de1d8183
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73133018"
---
# <a name="ihosttaskmanager-interface"></a>IHostTaskManager インターフェイス
標準のオペレーティングシステムのスレッド処理やファイバー関数を使用する代わりに、共通言語ランタイム (CLR) がホストを通じてタスクを操作できるようにするメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[BeginDelayAbort メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-begindelayabort-method.md)|マネージコードが、現在のタスクを中止できない期間を入力していることをホストに通知します。|  
|[BeginThreadAffinity メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-beginthreadaffinity-method.md)|マネージコードが、現在のタスクを別のオペレーティングシステムのスレッドに移動できない期間を入力していることをホストに通知します。|  
|[CallNeedsHostHook メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-callneedshosthook-method.md)|共通言語ランタイムがアンマネージ関数に対して指定された呼び出しをインライン展開できるかどうかを指定できるようにします。|  
|[CreateTask メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-createtask-method.md)|ホストが新しいタスクを作成することを要求します。|  
|[EndDelayAbort メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-enddelayabort-method.md)|以前に `BeginDelayAbort`を呼び出した後に、マネージコードが現在のタスクを中止できない期間を終了することをホストに通知します。|  
|[EndThreadAffinity メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-endthreadaffinity-method.md)|以前に `BeginThreadAffinity`を呼び出した後に、マネージコードが、現在のタスクを別のオペレーティングシステムのスレッドに移動できない期間を終了していることをホストに通知します。|  
|[EnterRuntime メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-enterruntime-method.md)|プラットフォーム呼び出しメソッドなどのアンマネージメソッドへの呼び出しが実行制御を CLR に返すことをホストに通知します。|  
|[GetCurrentTask メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-getcurrenttask-method.md)|この呼び出しが行われたオペレーティングシステムスレッドで現在実行されているタスクへのインターフェイスポインターを取得します。|  
|[GetStackGuarantee メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-getstackguarantee-method.md)|スタック操作が完了した後、プロセスが終了する前に使用可能であることが保証されているスタック領域の量を取得します。|  
|[LeaveRuntime メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-leaveruntime-method.md)|マネージコードがアンマネージ関数の呼び出しを実行しようとしていることをホストに通知します。|  
|[ReverseEnterRuntime メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-reverseenterruntime-method.md)|アンマネージコードから共通言語ランタイム (CLR) への呼び出しが行われていることをホストに通知します。|  
|[ReverseLeaveRuntime メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-reverseleaveruntime-method.md)|コントロールが CLR から出ていること、およびマネージコードから呼び出されたアンマネージ関数を入力したことをホストに通知します。|  
|[SetCLRTaskManager メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-setclrtaskmanager-method.md)|CLR によって実装された[ICLRTaskManager](../../../../docs/framework/unmanaged-api/hosting/iclrtaskmanager-interface.md)インスタンスへのインターフェイスポインターをホストに提供します。|  
|[SetLocale メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-setlocale-method.md)|CLR によって現在のタスクのロケールが変更されたことをホストに通知します。|  
|[SetStackGuarantee メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-setstackguarantee-method.md)|内部使用専用に予約されています。|  
|[SetUILocale メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-setuilocale-method.md)|現在のタスクでユーザーインターフェイスのロケールが変更されたことをホストに通知します。|  
|[Sleep メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-sleep-method.md)|現在のタスクがスリープ状態になることをホストに通知します。|  
|[SwitchToTask メソッド](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-switchtotask-method.md)|現在のタスクを切り替える必要があることをホストに通知します。|  
  
## <a name="remarks"></a>Remarks  
 `IHostTaskManager` を使用すると、CLR はタスクを作成および管理できます。また、制御がマネージコードからアンマネージコードに転送されるときにホストがアクションを実行できるようにしたり、コードの実行中にホストが実行できる特定のアクションを指定したりすることができます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRTask インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtask-interface.md)
- [ICLRTaskManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtaskmanager-interface.md)
- [IHostTask インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihosttask-interface.md)
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
