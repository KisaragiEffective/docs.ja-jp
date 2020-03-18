---
title: ICLRHostBindingPolicyManager::EvaluatePolicy メソッド
ms.date: 03/30/2017
api_name:
- ICLRHostBindingPolicyManager.EvaluatePolicy
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRHostBindingPolicyManager::EvaluatePolicy
helpviewer_keywords:
- ICLRHostBindingPolicyManager::EvaluatePolicy method [.NET Framework hosting]
- EvaluatePolicy method [.NET Framework hosting]
ms.assetid: 3a3a9446-7a4e-4836-9b27-5c536c15993d
topic_type:
- apiref
ms.openlocfilehash: 9600573a0a730cee10247d5644d587e75856cdd9
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73141184"
---
# <a name="iclrhostbindingpolicymanagerevaluatepolicy-method"></a>ICLRHostBindingPolicyManager::EvaluatePolicy メソッド
ホストの代わりにバインドポリシーを評価します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EvaluatePolicy (  
    [in] LPCWSTR     pwzReferenceIdentity,  
    [in] BYTE       *pbApplicationPolicy,  
    [in] DWORD       cbAppPolicySize,  
    [out, size_is(*pcchPostPolicyReferenceIdentity)] LPWSTR pwzPostPolicyReferenceIdentity,  
    [in, out] DWORD *pcchPostPolicyReferenceIdentity,  
    [out] DWORD     *pdwPoliciesApplied  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pwzReferenceIdentity`  
 からポリシー評価の前のアセンブリへの参照。  
  
 `pbApplicationPolicy`  
 からポリシーデータを格納しているバッファーへのポインター。  
  
 `cbAppPolicySize`  
 から`pbApplicationPolicy` バッファーのサイズ。  
  
 `pwzPostPolicyReferenceIdentity`  
 入出力新しいポリシーデータを評価した後のアセンブリへの参照。  
  
 `pcchPostPolicyReferenceIdentity`  
 [入力、出力]新しいポリシーデータを評価した後のアセンブリ id 参照バッファーのサイズへのポインター。  
  
 `pdwPoliciesApplied`  
 入出力適用されているポリシーを示す[Ebindpolicylevels](../../../../docs/framework/unmanaged-api/hosting/ebindpolicylevels-enumeration.md)値の論理和の組み合わせへのポインター。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|評価が正常に完了しました。|  
|E_INVALIDARG|`pwzReferenceIdentity` または `pbApplicationPolicy` のいずれかが null 参照です。|  
|ERROR_INSUFFICIENT_BUFFER|`cbAppPolicySize` が小さすぎます。|  
|HOST_E_CLRNOTAVAILABLE|共通言語ランタイム (CLR) がプロセスに読み込まれていないか、CLR がマネージコードを実行できない状態であるか、または呼び出しが正常に処理されていません。|  
|HOST_E_TIMEOUT|呼び出しがタイムアウトしました。|  
|HOST_E_NOT_OWNER|呼び出し元がロックを所有していません。|  
|HOST_E_ABANDONED|ブロックされたスレッドまたはファイバーが待機しているときに、イベントが取り消されました。|  
|E_FAIL|原因不明の致命的なエラーが発生しました。 メソッドから E_FAIL が返された後は、そのプロセス内で CLR を使用できなくなります。 後続のホストメソッドの呼び出しでは、HOST_E_CLRNOTAVAILABLE が返されます。|  
  
## <a name="remarks"></a>Remarks  
 `EvaluatePolicy` メソッドを使用すると、ホストは、ホスト固有のアセンブリのバージョン管理要件を維持するために、バインドポリシーに影響を与えることができます。 ポリシーエンジン自体は CLR 内に残ります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRHostBindingPolicyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrhostbindingpolicymanager-interface.md)
