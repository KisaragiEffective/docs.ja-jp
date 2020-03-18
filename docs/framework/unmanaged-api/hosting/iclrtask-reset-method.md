---
title: ICLRTask::Reset メソッド
ms.date: 03/30/2017
api_name:
- ICLRTask.Reset
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRTask::Reset
helpviewer_keywords:
- ICLRTask::Reset method [.NET Framework hosting]
- Reset method, ICLRTask interface [.NET Framework hosting]
ms.assetid: 1bfb5d3a-0ffd-4bb4-9bf6-aec00cb675b7
topic_type:
- apiref
ms.openlocfilehash: 17fca3e5a2d763277d3a5f9f72e2d35be6fc350c
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73124639"
---
# <a name="iclrtaskreset-method"></a>ICLRTask::Reset メソッド
ホストがタスクを完了したことを共通言語ランタイム (CLR) に通知し、CLR が現在の[ICLRTask](../../../../docs/framework/unmanaged-api/hosting/iclrtask-interface.md)インスタンスを再利用して別のタスクを表すことができるようにします。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT Reset (  
    [in] BOOL fFull  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `fFull`  
 [in] `true`、現在の `ICLRTask` インスタンスに関連するセキュリティおよびロケール情報に加えて、ランタイムがスレッド関連のすべての静的な値をリセットする必要がある場合はです。それ以外の場合は、`false`ます。  
  
 値が `true`場合、ランタイムは <xref:System.Threading.Thread.AllocateDataSlot%2A> または <xref:System.Threading.Thread.AllocateNamedDataSlot%2A>を使用して格納されたデータをリセットします。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|`Reset` が正常に返されました。|  
|HOST_E_CLRNOTAVAILABLE|CLR がプロセスに読み込まれていないか、CLR がマネージコードを実行できない状態であるか、または呼び出しを処理できません。 なく|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドから E_FAIL が返された場合、そのプロセス内で CLR は使用できなくなります。 後続のホストメソッドの呼び出しでは、HOST_E_CLRNOTAVAILABLE が返されます。|  
  
## <a name="remarks"></a>Remarks  
 CLR は、新しく作成された `ICLRTask` インスタンスをリサイクルして、新しいタスクが必要になるたびに新しいインスタンスを繰り返し作成するオーバーヘッドを回避できます。 ホストは、タスクを完了したときに、 [ICLRTask:: ExitTask](../../../../docs/framework/unmanaged-api/hosting/iclrtask-exittask-method.md)ではなく `ICLRTask::Reset` を呼び出すことによって、この機能を有効にします。 次の一覧は、`ICLRTask` インスタンスの通常のライフサイクルをまとめたものです。  
  
1. ランタイムは、新しい `ICLRTask` インスタンスを作成します。  
  
2. ランタイムは、 [IHostTaskManager:: GetCurrentTask](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-getcurrenttask-method.md)を呼び出して、現在のホストタスクへの参照を取得します。  
  
3. ランタイムは、新しいインスタンスをホストタスクに関連付けるために、 [IHostTask:: SetCLRTask](../../../../docs/framework/unmanaged-api/hosting/ihosttask-setclrtask-method.md)を呼び出します。  
  
4. タスクが実行され、完了します。  
  
5. ホストは `ICLRTask::ExitTask`を呼び出すことによってタスクを破棄します。  
  
 `Reset` は、このシナリオを次の2つの方法で変更します。 上記の手順5では、ホストは `Reset` を呼び出して、タスクをクリーンな状態にリセットしてから、関連付けられている[IHostTask](../../../../docs/framework/unmanaged-api/hosting/ihosttask-interface.md)インスタンスから `ICLRTask` インスタンスを切り離します。 必要に応じて、ホストは `IHostTask` インスタンスをキャッシュして再利用することもできます。 上記の手順 1. では、ランタイムは、新しいインスタンスを作成する代わりに、リサイクルされた `ICLRTask` をキャッシュから取得します。  
  
 この方法は、ホストに再利用可能なワーカータスクのプールがある場合に適しています。 ホストが `IHostTask` インスタンスのいずれかを破棄すると、`ExitTask`を呼び出すことによって、対応する `ICLRTask` が破棄されます。  
  
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
