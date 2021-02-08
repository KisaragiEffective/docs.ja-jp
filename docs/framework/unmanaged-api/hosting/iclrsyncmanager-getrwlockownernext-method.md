---
description: '詳細について: ICLRSyncManager:: GetRWLockOwnerNext メソッド'
title: ICLRSyncManager::GetRWLockOwnerNext メソッド
ms.date: 03/30/2017
api_name:
- ICLRSyncManager.GetRWLockOwnerNext
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRSyncManager::GetRWLockOwnerNext
helpviewer_keywords:
- ICLRSyncManager::GetRWLockOwnerNext method [.NET Framework hosting]
- GetRWLockOwnerNext method [.NET Framework hosting]
ms.assetid: 0e025b6a-280e-40a2-a2d0-b15f58777b81
topic_type:
- apiref
ms.openlocfilehash: f49bdd5065ac896147967c0a013347ab39ce1eff
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99784995"
---
# <a name="iclrsyncmanagergetrwlockownernext-method"></a>ICLRSyncManager::GetRWLockOwnerNext メソッド

現在のリーダーライターロックでブロックされている次の [IHostTask](ihosttask-interface.md) インスタンスを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp
HRESULT GetRWLockOwnerNext (  
    [in] SIZE_T       Iterator,  
    [out] IHostTask  *ppOwnerHostTask  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `Iterator`  
 から [Createrwlockowneriterator](iclrsyncmanager-createrwlockowneriterator-method.md)の呼び出しを使用して作成された反復子。  
  
 `ppOwnerHostTask`  
 入出力ロックを待機している次のへのポインター `IHostTask` 。タスクが待機していない場合は null。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|`GetRWLockOwnerNext` 正常に返されました。|  
|HOST_E_CLRNOTAVAILABLE|共通言語ランタイム (CLR) がプロセスに読み込まれていないか、CLR がマネージコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドが E_FAIL を返すと、そのプロセス内で CLR が使用できなくなります。 後続のホストメソッドの呼び出しでは HOST_E_CLRNOTAVAILABLE が返されます。|  
  
## <a name="remarks"></a>解説  

 `ppOwnerHostTask`が null に設定されている場合、イテレーションは終了し、ホストは[DeleteRWLockOwnerIterator](iclrsyncmanager-deleterwlockowneriterator-method.md)メソッドを呼び出す必要があります。  
  
> [!NOTE]
> CLR は `AddRef` 、 `IHostTask` `ppOwnerHostTask` ホストがポインターを保持している間に、このタスクが終了しないようにするを指すに対してを呼び出します。 ホストは、 `Release` 終了時に参照カウントをデクリメントするためにを呼び出す必要があります。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** MSCorEE.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRSyncManager インターフェイス](iclrsyncmanager-interface.md)
- [IHostSyncManager インターフェイス](ihostsyncmanager-interface.md)
