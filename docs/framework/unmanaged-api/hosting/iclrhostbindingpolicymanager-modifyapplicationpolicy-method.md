---
title: ICLRHostBindingPolicyManager::ModifyApplicationPolicy メソッド
ms.date: 03/30/2017
api_name:
- ICLRHostBindingPolicyManager.ModifyApplicationPolicy
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRHostBindingPolicyManager::ModifyApplicationPolicy
helpviewer_keywords:
- ICLRHostBindingPolicyManager::ModifyApplicationPolicy method [.NET Framework hosting]
- ModifyApplicationPolicy method [.NET Framework hosting]
ms.assetid: d82d633e-cce6-427c-8b02-8227e34e12ba
topic_type:
- apiref
ms.openlocfilehash: d5323538447e083a0c727e43261dd68337182b9b
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73141080"
---
# <a name="iclrhostbindingpolicymanagermodifyapplicationpolicy-method"></a>ICLRHostBindingPolicyManager::ModifyApplicationPolicy メソッド
指定したアセンブリのバインディングポリシーを変更し、新しいバージョンのポリシーを作成します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT  ModifyApplicationPolicy (  
    [in] LPCWSTR     pwzSourceAssemblyIdentity,   
    [in] LPCWSTR     pwzTargetAssemblyIdentity,  
    [in] BYTE       *pbApplicationPolicy,  
    [in] DWORD       cbAppPolicySize,  
    [in] DWORD       dwPolicyModifyFlags,  
    [out, size_is(*pcbNewAppPolicySize)] BYTE *pbNewApplicationPolicy,   
    [in, out] DWORD *pcbNewAppPolicySize  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pwzSourceAssemblyIdentity`  
 から変更するアセンブリの id。  
  
 `pwzTargetAssemblyIdentity`  
 から変更されたアセンブリの新しい id。  
  
 `pbApplicationPolicy`  
 から変更するアセンブリのバインディングポリシーデータを格納しているバッファーへのポインター。  
  
 `cbAppPolicySize`  
 から置換されるバインディングポリシーのサイズ。  
  
 `dwPolicyModifyFlags`  
 からリダイレクトの制御を示す[Ehostbindingpolicymodifyflags](../../../../docs/framework/unmanaged-api/hosting/ehostbindingpolicymodifyflags-enumeration.md)値の論理的または組み合わせ。  
  
 `pbNewApplicationPolicy`  
 入出力新しいバインドポリシーデータを格納しているバッファーへのポインター。  
  
 `pcbNewAppPolicySize`  
 [入力、出力]新しいバインドポリシーバッファーのサイズへのポインター。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|ポリシーが正常に変更されました。|  
|E_INVALIDARG|`pwzSourceAssemblyIdentity` または `pwzTargetAssemblyIdentity` が null 参照でした。|  
|ERROR_INSUFFICIENT_BUFFER|`pbNewApplicationPolicy` が小さすぎます。|  
|HOST_E_CLRNOTAVAILABLE|共通言語ランタイム (CLR) がプロセスに読み込まれていないか、CLR がマネージコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドから E_FAIL が返された後は、そのプロセス内で CLR を使用できなくなります。 後続のホストメソッドの呼び出しでは、HOST_E_CLRNOTAVAILABLE が返されます。|  
  
## <a name="remarks"></a>Remarks  
 `ModifyApplicationPolicy` メソッドは、2回呼び出すことができます。 最初の呼び出しでは、`pbNewApplicationPolicy` パラメーターに null 値を指定する必要があります。 この呼び出しは、`pcbNewAppPolicySize`に必要な値と共に返されます。 2番目の呼び出しでは、`pcbNewAppPolicySize`にこの値を指定し、`pbNewApplicationPolicy`のバッファーのサイズをポイントする必要があります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRHostBindingPolicyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrhostbindingpolicymanager-interface.md)
