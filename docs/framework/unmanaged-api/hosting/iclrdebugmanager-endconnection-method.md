---
description: '詳細情報: ICLRDebugManager:: EndConnection メソッド'
title: ICLRDebugManager::EndConnection メソッド
ms.date: 03/30/2017
api_name:
- ICLRDebugManager.EndConnection
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRDebugManager::EndConnection
helpviewer_keywords:
- ICLRDebugManager::EndConnection method [.NET Framework hosting]
- EndConnection method [.NET Framework hosting]
ms.assetid: 89dc7363-2f29-4eb2-8f23-fccdda6a76a6
topic_type:
- apiref
ms.openlocfilehash: 06dc9e20ec02c3e3040090babcc443a2ae59848b
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99746053"
---
# <a name="iclrdebugmanagerendconnection-method"></a>ICLRDebugManager::EndConnection メソッド

タスクのリストと id とフレンドリ名の間の関連付けを削除します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EndConnection (  
    [in] CONNID dwConnectionId  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `dwConnectionId`  
 から接続のホスト固有の識別子と、関連付けられている共通言語ランタイム (CLR) タスクのリスト。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|`EndConnection` 正常に返されました。|  
|HOST_E_CLRNOTAVAILABLE|CLR がプロセスに読み込まれていないか、CLR がマネージドコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドが E_FAIL を返すと、そのプロセス内で CLR が使用できなくなります。 後続のホストメソッドの呼び出しでは HOST_E_CLRNOTAVAILABLE が返されます。|  
|E_INVALIDARG|[Beginconnection](iclrdebugmanager-beginconnection-method.md) がを使用して呼び出されていない `dwConnectionId` か、または `dwConnectionId` が0でした。|  
  
## <a name="remarks"></a>解説  

 [ICLRDebugManager](iclrdebugmanager-interface.md)には、、setconnectiontasks、およびの3つのメソッドが用意されており、 `BeginConnection` [](iclrdebugmanager-setconnectiontasks-method.md) `EndConnection` タスクリストを識別子と表示名に関連付けることができます。  
  
> [!IMPORTANT]
> これら3つのメソッドは、一連のタスクごとに特定の順序で呼び出す必要があります。 `BeginConnection` は、新しい接続を確立するために最初に呼び出されます。 `SetConnectionTasks` は、その接続に関連する一連のタスクを提供するために、次に呼び出されます。 `EndConnection` は、タスク一覧と識別子とフレンドリ名の間の関連付けを削除するために最後に呼び出されます。ただし、異なる接続の呼び出しは入れ子にすることができます。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** MSCorEE.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRControl インターフェイス](iclrcontrol-interface.md)
- [ICLRDebugManager インターフェイス](iclrdebugmanager-interface.md)
- [BeginConnection メソッド](iclrdebugmanager-beginconnection-method.md)
- [SetConnectionTasks メソッド](iclrdebugmanager-setconnectiontasks-method.md)
- [IHostControl インターフェイス](ihostcontrol-interface.md)
