---
title: IHostSyncManager インターフェイス
ms.date: 03/30/2017
api_name:
- IHostSyncManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSyncManager
helpviewer_keywords:
- IHostSyncManager interface [.NET Framework hosting]
ms.assetid: 2e081a37-6a28-4c93-b7ab-1c96a464637c
topic_type:
- apiref
ms.openlocfilehash: 02a59b8ef63f7e866e419db4e3232da7eec19558
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73132630"
---
# <a name="ihostsyncmanager-interface"></a>IHostSyncManager インターフェイス
Win32 同期関数を使用する代わりに、共通言語ランタイム (CLR) がホストを呼び出して同期プリミティブを作成できるようにするメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[CreateAutoEvent メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostsyncmanager-createautoevent-method.md)|自動リセットイベントオブジェクトを作成します。|  
|[CreateCrst メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostsyncmanager-createcrst-method.md)|同期のための重要なセクションオブジェクトを作成します。|  
|[CreateCrstWithSpinCount メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostsyncmanager-createcrstwithspincount-method.md)|同期のためにスピンカウントを持つクリティカルセクションオブジェクトを作成します。|  
|[CreateManualEvent メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostsyncmanager-createmanualevent-method.md)|手動リセットイベントオブジェクトを作成します。|  
|[CreateMonitorEvent メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostsyncmanager-createmonitorevent-method.md)|監視対象の自動リセットイベントオブジェクトを作成します。|  
|[CreateRWLockReaderEvent メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostsyncmanager-createrwlockreaderevent-method.md)|リーダーロックの実装のための手動リセットイベントオブジェクトを作成します。|  
|[CreateRWLockWriterEvent メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostsyncmanager-createrwlockwriterevent-method.md)|ライターロックの実装用の自動リセットイベントオブジェクトを作成します。|  
|[CreateSemaphore メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostsyncmanager-createsemaphore-method.md)|CLR が待機イベントのセマフォとして使用する[IHostSemaphore](../../../../docs/framework/unmanaged-api/hosting/ihostsemaphore-interface.md)オブジェクトを作成します。|  
|[SetCLRSyncManager メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostsyncmanager-setclrsyncmanager-method.md)|現在の `IHostSyncManager` インスタンスに関連付ける[ICLRSyncManager](../../../../docs/framework/unmanaged-api/hosting/iclrsyncmanager-interface.md)インスタンスを設定します。|  
  
## <a name="remarks"></a>Remarks  
 CLR は、IID_IHostSyncManager の `IID` を使用して[IHostControl:: GetHostManager](../../../../docs/framework/unmanaged-api/hosting/ihostcontrol-gethostmanager-method.md)メソッドを呼び出すことによって、ホストの `IHostSyncManager` の実装を検出します。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRSyncManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrsyncmanager-interface.md)
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
