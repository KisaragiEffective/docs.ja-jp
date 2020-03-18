---
title: ICLRTask2::BeginPreventAsyncAbort メソッド
ms.date: 03/30/2017
api_name:
- ICLRTask2.BeginPreventAsyncAbort
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRTask2::BeginPreventAsyncAbort
helpviewer_keywords:
- ICLRTask2::BeginPreventAsyncAbort method [.NET Framework hosting]
- BeginPreventAsyncAbort method [.NET Framework hosting]
ms.assetid: 75754c2f-38c7-4707-85fe-559db4542729
topic_type:
- apiref
ms.openlocfilehash: 67841bbcd796e41b3b81f922020fe6c3677730c4
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73124560"
---
# <a name="iclrtask2beginpreventasyncabort-method"></a>ICLRTask2::BeginPreventAsyncAbort メソッド
現在のスレッドでのスレッドの中止要求から、新しいスレッド中止要求を遅延します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT BeginPreventAsyncAbort();  
```  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、次の特定の HRESULT と、メソッドの失敗を示す HRESULT エラーも返します。  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|メソッドは正常に完了しました。|  
|HOST_E_INVALIDOPERATION|メソッドは、現在のスレッドではないスレッドで呼び出されました。|  
  
## <a name="remarks"></a>Remarks  
 このメソッドを呼び出すと、現在のスレッドの遅延スレッド中止カウンターが1つ増加します。  
  
 `BeginPreventAsyncAbort` と[ICLRTask2:: EndPreventAsyncAbort](../../../../docs/framework/unmanaged-api/hosting/iclrtask2-endpreventasyncabort-method.md)への呼び出しは入れ子にすることができます。 カウンターがゼロより大きい限り、現在のスレッドのスレッド中止は遅延されます。 この呼び出しが `EndPreventAsyncAbort` メソッドの呼び出しとペアになっていない場合、現在のスレッドに対してスレッド中止が配信されない状態になる可能性があります。  
  
 遅延は、それ自体を中止するスレッドには適用されません。  
  
 この機能によって公開されている機能は、仮想マシン (VM) によって内部的に使用されます。 これらの方法を誤用すると、VM で特定できない動作が発生する可能性があります。 たとえば、最初に `BeginPreventAsyncAbort` を呼び出さずに `EndPreventAsyncAbort` を呼び出すと、VM が以前にインクリメントしたときにカウンターを0に設定できます。 同様に、内部カウンターのオーバーフローはチェックされません。 ホストと VM の両方によってインクリメントされるために整数の制限を超えた場合、結果として得られる動作は指定されません。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [EndPreventAsyncAbort メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask2-endpreventasyncabort-method.md)
- [ICLRTask2 インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtask2-interface.md)
- [ICLRTaskManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtaskmanager-interface.md)
- [IHostTask インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihosttask-interface.md)
- [IHostTaskManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-interface.md)
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
