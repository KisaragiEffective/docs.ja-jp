---
title: ICLRPolicyManager::SetActionOnFailure メソッド
ms.date: 03/30/2017
api_name:
- ICLRPolicyManager.SetActionOnFailure
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRPolicyManager::SetActionOnFailure
helpviewer_keywords:
- SetActionOnFailure method [.NET Framework hosting]
- ICLRPolicyManager::SetActionOnFailure method [.NET Framework hosting]
ms.assetid: 4664033f-db97-4388-b988-2ec470796e58
topic_type:
- apiref
ms.openlocfilehash: 143052febe136e969987c35bc06f6c3b3356aedf
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73140779"
---
# <a name="iclrpolicymanagersetactiononfailure-method"></a>ICLRPolicyManager::SetActionOnFailure メソッド
指定したエラーが発生したときに共通言語ランタイム (CLR) が実行するポリシーアクションを指定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetActionOnFailure (  
    [in] EClrFailure   failure,  
    [in] EPolicyAction action  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `failure`  
 から[Eclrfailure](../../../../docs/framework/unmanaged-api/hosting/eclrfailure-enumeration.md)値の1つで、アクションを実行するエラーの種類を示します。  
  
 `action`  
 から[Epolicyaction](../../../../docs/framework/unmanaged-api/hosting/epolicyaction-enumeration.md)値の1つ。エラーが発生したときに実行するアクションを示します。 サポートされている値の一覧については、「解説」を参照してください。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|`SetActionOnFailure` が正常に返されました。|  
|HOST_E_CLRNOTAVAILABLE|CLR がプロセスに読み込まれていないか、CLR がマネージドコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドから E_FAIL が返された後は、そのプロセス内で CLR を使用できなくなります。 後続のホストメソッドの呼び出しでは、HOST_E_CLRNOTAVAILABLE が返されます。|  
|E_INVALIDARG|指定された操作に対してポリシーアクションを設定できないか、操作に無効なポリシーアクションが指定されました。|  
  
## <a name="remarks"></a>Remarks  
 既定では、メモリなどのリソースの割り当てに失敗した場合、CLR は例外をスローします。 `SetActionOnFailure` では、エラー発生時に実行するポリシーアクションを指定することにより、ホストはこの動作をオーバーライドできます。 次の表は、サポートされている[Eclrfailure](../../../../docs/framework/unmanaged-api/hosting/eclrfailure-enumeration.md)値と[epolicyaction](../../../../docs/framework/unmanaged-api/hosting/epolicyaction-enumeration.md)値の組み合わせを示しています。 (FAIL_ プレフィックスは[Eclrfailure](../../../../docs/framework/unmanaged-api/hosting/eclrfailure-enumeration.md)値から省略されています)。  
  
||NonCriticalResource|CriticalResource|Fat (Alruntime)|OrphanedLock|StackOverflow|AccessViolation|CodeContract|  
|-|-------------------------|----------------------|------------------|------------------|-------------------|---------------------|------------------|  
|`eNoAction`|x|x||||N/A||  
|の例外|x|x||||N/A||  
|`eAbortThread`|x|x||||N/A|x|  
|`eRudeAbortThread`|x|x||||N/A|x|  
|`eUnloadAppDomain`|x|x||x||N/A|x|  
|`eRudeUnloadAppDomain`|x|x||x|x|N/A|x|  
|`eExitProcess`|x|x||x|x|N/A|x|  
|eFastExitProcess|x|x||x|x|N/A||  
|`eRudeExitProcess`|x|x|x|x|x|N/A||  
|`eDisableRuntime`|x|x|x|x|x|N/A||  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [EClrFailure 列挙型](../../../../docs/framework/unmanaged-api/hosting/eclrfailure-enumeration.md)
- [EPolicyAction 列挙型](../../../../docs/framework/unmanaged-api/hosting/epolicyaction-enumeration.md)
- [ICLRControl インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrcontrol-interface.md)
- [ICLRPolicyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrpolicymanager-interface.md)
