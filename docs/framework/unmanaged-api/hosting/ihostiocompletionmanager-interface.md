---
title: IHostIoCompletionManager インターフェイス
ms.date: 03/30/2017
api_name:
- IHostIoCompletionManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostIoCompletionManager
helpviewer_keywords:
- IHostIoCompletionManager interface [.NET Framework hosting]
ms.assetid: c28d1983-83f7-46e2-990f-dbb9dc07c818
topic_type:
- apiref
ms.openlocfilehash: 51d79c398c94ec355528140325da2c25422cbad9
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73133846"
---
# <a name="ihostiocompletionmanager-interface"></a>IHostIoCompletionManager インターフェイス
共通言語ランタイム (CLR) がホストによって提供される i/o 完了ポートと対話できるようにするメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[Bind メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-bind-method.md)|I/o 完了ポートにハンドルをバインドします。|  
|[CloseIoCompletionPort メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-closeiocompletionport-method.md)|以前の `CreateIoCompletionPort`の呼び出しで作成されたポートを閉じます。|  
|[CreateIoCompletionPort メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-createiocompletionport-method.md)|ホストが新しい i/o 完了ポートを作成することを要求します。|  
|[GetAvailableThreads メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-getavailablethreads-method.md)|現在要求を処理していない i/o 完了スレッドの数を取得します。|  
|[GetHostOverlappedSize メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-gethostoverlappedsize-method.md)|ホストが i/o 要求に追加しようとしているカスタムデータのサイズを取得します。|  
|[GetMaxThreads メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-getmaxthreads-method.md)|ホストがサービス i/o 要求に割り当てることができるスレッドの最大数を取得します。|  
|[GetMinThreads メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-getminthreads-method.md)|ホストが i/o 要求を処理するために提供するスレッドの最小数を取得します。|  
|[InitializeHostOverlapped メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-initializehostoverlapped-method.md)|I/o 要求に関するカスタムデータを初期化する機会をホストに提供します。|  
|[SetCLRIoCompletionManager メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-setclriocompletionmanager-method.md)|CLR によって実装されている[Iclrio提供マネージャー](../../../../docs/framework/unmanaged-api/hosting/iclriocompletionmanager-interface.md)インスタンスへのインターフェイスポインターをホストに提供します。|  
|[SetMaxThreads メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-setmaxthreads-method.md)|ホストが大量の i/o 要求を処理するスレッドの最大数を設定します。|  
|[SetMinThreads メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-setminthreads-method.md)|ホストが i/o 完了に割り当てる必要があるスレッドの最小数を設定します。|  
  
## <a name="remarks"></a>Remarks  
 `IHostIoCompletionManager` は、CLR によって実装される `ICLRIoCompletionManager` インターフェイスに対応します。 CLR は、`IHostIoCompletionManager` のメソッドを呼び出して、ホストが提供するポートにハンドルをバインドします。また、ホストは、`ICLRIoCompletionManager` のメソッドを呼び出して、i/o 要求の完了を報告します。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
