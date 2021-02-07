---
description: '詳細について: IHostManualEvent:: Wait メソッド'
title: IHostManualEvent::Wait メソッド
ms.date: 03/30/2017
api_name:
- IHostManualEvent.Wait
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostManualEvent::Wait
helpviewer_keywords:
- IHostManualEvent::Wait method [.NET Framework hosting]
- Wait method, IHostManualEvent interface [.NET Framework hosting]
ms.assetid: 1fbb7d8b-8a23-4c2b-8376-1a70cd2d6030
topic_type:
- apiref
ms.openlocfilehash: f9e681e5075f1de34dc45ed2b2485a0e1269cb11
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99707869"
---
# <a name="ihostmanualeventwait-method"></a>IHostManualEvent::Wait メソッド

現在の [IHostManualEvent](ihostmanualevent-interface.md) インスタンスが所有されるか、指定された時間が経過するまで待機するようにします。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT Wait (  
    [in] DWORD dwMilliseconds,  
    [in] DWORD option  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `dwMilliseconds`  
 から現在のインスタンスが所有されていない場合に、を返す前に待機するミリ秒数 `IHostManualEvent` 。  
  
 `option`  
 から [WAIT_OPTION](wait-option-enumeration.md) のいずれかの値。この操作がブロックされた場合にホストが実行するアクションを示します。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|`Wait` 正常に返されました。|  
|HOST_E_CLRNOTAVAILABLE|共通言語ランタイム (CLR) がプロセスに読み込まれていないか、CLR がマネージコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドが E_FAIL を返すと、そのプロセス内で CLR が使用できなくなります。 後続のホストメソッドの呼び出しでは HOST_E_CLRNOTAVAILABLE が返されます。|  
|HOST_E_DEADLOCK|ホストは待機間隔中にデッドロックを検出し、現在の `IHostManualEvent` インスタンスをデッドロックの対象として選択しました。|  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** MSCorEE.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRSyncManager インターフェイス](iclrsyncmanager-interface.md)
- [IHostAutoEvent インターフェイス](ihostautoevent-interface.md)
- [IHostManualEvent インターフェイス](ihostmanualevent-interface.md)
- [IHostSemaphore インターフェイス](ihostsemaphore-interface.md)
- [IHostSyncManager インターフェイス](ihostsyncmanager-interface.md)
