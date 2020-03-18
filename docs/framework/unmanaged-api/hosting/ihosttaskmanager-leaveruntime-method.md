---
title: IHostTaskManager::LeaveRuntime メソッド
ms.date: 03/30/2017
api_name:
- IHostTaskManager.LeaveRuntime
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager::LeaveRuntime
helpviewer_keywords:
- IHostTaskManager::LeaveRuntime method [.NET Framework hosting]
- LeaveRuntime method [.NET Framework hosting]
ms.assetid: 43689cc4-e48e-46e5-a22d-bafd768b8759
topic_type:
- apiref
ms.openlocfilehash: 8ac1c18d094deca50d461ef9ff0933a4f87176e0
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73132994"
---
# <a name="ihosttaskmanagerleaveruntime-method"></a>IHostTaskManager::LeaveRuntime メソッド
現在実行中のタスクが共通言語ランタイム (CLR) を終了しようとしていることをホストに通知し、アンマネージコードを入力します。  
  
> [!IMPORTANT]
> [IHostTaskManager:: EnterRuntime](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-enterruntime-method.md)への対応する呼び出しは、現在実行中のタスクがマネージコードを再要求していることをホストに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT LeaveRuntime (  
    [in] SIZE_T target  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `target`  
 から呼び出されるアンマネージ関数の、マップされた移植可能な実行可能ファイル内のアドレス。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|`LeaveRuntime` が正常に返されました。|  
|HOST_E_CLRNOTAVAILABLE|CLR がプロセスに読み込まれていないか、CLR がマネージドコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドから E_FAIL が返された場合、そのプロセス内で CLR は使用できなくなります。 後続のホストメソッドの呼び出しでは、HOST_E_CLRNOTAVAILABLE が返されます。|  
|E_OUTOFMEMORY|要求された割り当てを完了するために必要なメモリが不足しています。|  
  
## <a name="remarks"></a>Remarks  
 アンマネージコードとの間の呼び出しシーケンスは入れ子にすることができます。 たとえば、次の一覧では、`LeaveRuntime`、 [IHostTaskManager:: ReverseEnterRuntime](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-reverseenterruntime-method.md)、 [IHostTaskManager:: ReverseLeaveRuntime](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-reverseleaveruntime-method.md)、および `IHostTaskManager::EnterRuntime` の呼び出しのシーケンスによって、ホストが入れ子になったレイヤー。  
  
|操作|対応するメソッド呼び出し|  
|------------|-------------------------------|  
|マネージ Visual Basic 実行可能ファイルは、プラットフォーム呼び出しを使用して C で記述されたアンマネージ関数を呼び出します。|`IHostTaskManager::LeaveRuntime`|  
|アンマネージ C 関数は、でC#記述されたマネージ DLL 内のメソッドを呼び出します。|`IHostTaskManager::ReverseEnterRuntime`|  
|マネージC#関数は、C で記述された別のアンマネージ関数を呼び出します。これは、プラットフォーム呼び出しを使用することもできます。|`IHostTaskManager::LeaveRuntime`|  
|2番目のアンマネージ関数はC# 、関数に実行を返します。|`IHostTaskManager::EnterRuntime`|  
|関数C#は、最初のアンマネージ関数に実行を返します。|`IHostTaskManager::ReverseLeaveRuntime`|  
|最初のアンマネージ関数は、実行を Visual Basic プログラムに返します。|`IHostTaskManager::EnterRuntime`|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRTask インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtask-interface.md)
- [ICLRTaskManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtaskmanager-interface.md)
- [IHostTask インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihosttask-interface.md)
- [IHostTaskManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-interface.md)
