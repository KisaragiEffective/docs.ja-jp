---
title: IHostThreadPoolManager インターフェイス
ms.date: 03/30/2017
api_name:
- IHostThreadPoolManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostThreadPoolManager
helpviewer_keywords:
- IHostThreadPoolManager interface [.NET Framework hosting]
ms.assetid: c3a2cd90-7c4e-4374-bb87-b41befb8344f
topic_type:
- apiref
ms.openlocfilehash: 8aef78c7cf70e6b2d97af9c47d57908b86729ff7
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73122081"
---
# <a name="ihostthreadpoolmanager-interface"></a>IHostThreadPoolManager インターフェイス
共通言語ランタイム (CLR) がスレッドプールを構成し、作業項目をスレッドプールにキューするようにするメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetAvailableThreads メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostthreadpoolmanager-getavailablethreads-method.md)|現在作業項目を処理していないスレッドプール内のスレッドの数を取得します。|  
|[GetMaxThreads メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostthreadpoolmanager-getmaxthreads-method.md)|ホストがスレッドプール内で同時に保持するスレッドの最大数を取得します。|  
|[GetMinThreads メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostthreadpoolmanager-getminthreads-method.md)|要求を処理するためにホストが保持するアイドル状態のスレッドの最小数を取得します。|  
|[QueueUserWorkItem メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostthreadpoolmanager-queueuserworkitem-method.md)|実行する関数をキューに配置し、関数によって使用されるデータを格納しているオブジェクトを提供します。|  
|[SetMaxThreads メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostthreadpoolmanager-setmaxthreads-method.md)|ホストがスレッドプールで保持できるスレッドの最大数を設定します。|  
|[SetMinThreads メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostthreadpoolmanager-setminthreads-method.md)|ホストが要求を見越して保持する必要があるアイドル状態のスレッドの最小数を設定します。|  
  
## <a name="remarks"></a>Remarks  
 ホストは、`SetMaxThreads` および `SetMinThreads` メソッドの呼び出しで指定された値を使用してスレッドプールを構成する必要はありません。 この場合、ホストはこれらのメソッドから E_NOTIMPL の HRESULT 値を返す必要があります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Threading>
- <xref:System.Threading.ThreadPool>
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
