---
title: IHostCrst インターフェイス
ms.date: 03/30/2017
api_name:
- IHostCrst
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostCrst
helpviewer_keywords:
- IHostCrst interface [.NET Framework hosting]
ms.assetid: ac298ebd-0815-47e4-a823-30b31baab903
topic_type:
- apiref
ms.openlocfilehash: 108492ba298e9f8429b2acd890ab3404365bc602
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130525"
---
# <a name="ihostcrst-interface"></a>IHostCrst インターフェイス
スレッド処理のためのクリティカルセクションのホストの表現として機能します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[Enter メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostcrst-enter-method.md)|クリティカルセクションに入ります。|  
|[Leave メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostcrst-leave-method.md)|クリティカルセクションを残します。|  
|[SetSpinCount メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostcrst-setspincount-method.md)|クリティカルセクションのスピンカウントを設定します。|  
|[TryEnter メソッド](../../../../docs/framework/unmanaged-api/hosting/ihostcrst-tryenter-method.md)|クリティカルセクションへの入力を試行し、成功または失敗を直ちに報告します。|  
  
## <a name="remarks"></a>Remarks  
 `IHostCrst` を使用すると、共通言語ランタイム (CLR) は、`EnterCriticalSection` や `LeaveCriticalSection`などの Win32 関数を使用するのではなく、クリティカルセクションのホストの表現と直接通信できます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRSyncManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrsyncmanager-interface.md)
- [IHostSyncManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostsyncmanager-interface.md)
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
