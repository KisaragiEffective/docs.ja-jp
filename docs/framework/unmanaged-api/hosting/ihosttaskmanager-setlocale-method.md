---
description: '詳細について: IHostTaskManager:: SetLocale メソッド'
title: IHostTaskManager::SetLocale メソッド
ms.date: 03/30/2017
api_name:
- IHostTaskManager.SetLocale
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager::SetLocale
helpviewer_keywords:
- SetLocale method, IHostTaskManager interface [.NET Framework hosting]
- IHostTaskManager::SetLocale method [.NET Framework hosting]
ms.assetid: 747ee407-ee8c-484d-9583-25089236d2d1
topic_type:
- apiref
ms.openlocfilehash: 522a3da9bcd8d61754684091f6de3f11f7ed478c
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99789412"
---
# <a name="ihosttaskmanagersetlocale-method"></a>IHostTaskManager::SetLocale メソッド

共通言語ランタイム (CLR) によって、現在実行中のタスクのロケール (カルチャ) が変更されたことをホストに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetLocale (  
    [in] LCID lcid  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `lcid`  
 から新しく割り当てられた地理的カルチャおよび言語にマップされるロケール識別子の値。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|`SetLocale` 正常に返されました。|  
|HOST_E_CLRNOTAVAILABLE|CLR がプロセスに読み込まれていないか、CLR がマネージドコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドが E_FAIL を返すと、そのプロセス内で CLR が使用できなくなります。 後続のホストメソッドの呼び出しでは HOST_E_CLRNOTAVAILABLE が返されます。|  
|E_NOTIMPL|ホストでは、マネージユーザーコードでロケールを変更することはできません。|  
  
## <a name="remarks"></a>解説  

 ランタイムは、 `SetLocale` プロパティの値 <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> がマネージコードによって変更されたときにを呼び出します。 この方法を使用すると、ホストはロケールの同期に必要なメカニズムを実行できます。 ホストでロケールをマネージコードから変更できない場合、またはロケールを同期する機構を実装していない場合は、このメソッドから E_NOTIMPL が返されます。  
  
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
- [SetUILocale メソッド](ihosttaskmanager-setuilocale-method.md)
