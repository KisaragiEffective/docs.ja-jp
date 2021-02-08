---
description: ': ICLRTask:: NeedsPriorityScheduling メソッドの詳細について説明します。'
title: ICLRTask::NeedsPriorityScheduling メソッド
ms.date: 03/30/2017
api_name:
- ICLRTask.NeedsPriorityScheduling
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRTask::NeedsPriorityScheduling
helpviewer_keywords:
- ICLRTask::NeedsPriorityScheduling method [.NET Framework hosting]
- NeedsPriorityScheduling method [.NET Framework hosting]
ms.assetid: 9c9db3f3-26bf-4317-88de-5eb926a22a1d
topic_type:
- apiref
ms.openlocfilehash: e6e1b93b38d86259dc2f405f8512ec1063fe7b3b
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99781767"
---
# <a name="iclrtaskneedspriorityscheduling-method"></a>ICLRTask::NeedsPriorityScheduling メソッド

現在のタスクが、切り替え先としてマークされている必要があるかどうかを示す値を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT NeedsPriorityScheduling (  
    [out] BOOL *pbNeedsPriorityScheduling  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `pbNeedsPriorityRescheduling`  
 [out] `true` 。ホストが現在のタスクインスタンスの再スケジュールをできるだけ早く試行する場合は。それ以外の場合は `false` 。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|`NeedsPriorityRescheduling` 正常に返されました。|  
|HOST_E_CLRNOTAVAILABLE|共通言語ランタイム (CLR) がプロセスに読み込まれていないか、CLR がマネージコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドが E_FAIL を返すと、そのプロセス内で CLR が使用できなくなります。 後続のホストメソッドの呼び出しでは HOST_E_CLRNOTAVAILABLE が返されます。|  
  
## <a name="remarks"></a>解説  

 ガベージコレクターによってタスクが収集されるのを終了する場合、CLR はの値をに設定して `pbNeedsPriorityScheduling` `true` 、優先度の高い再スケジュールを示します。 これにより、ホストはタスクを迅速に再スケジュールすることができます。これにより、ガベージコレクションの遅延の可能性が最小限に抑えられ、ホストとランタイムがメモリリソースを節約できるようになります。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** MSCorEE.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRTask インターフェイス](iclrtask-interface.md)
- [ICLRTaskManager インターフェイス](iclrtaskmanager-interface.md)
- [IHostTask インターフェイス](ihosttask-interface.md)
- [IHostTaskManager インターフェイス](ihosttaskmanager-interface.md)
