---
title: ICLRMetaHostPolicy::GetRequestedRuntime メソッド
ms.date: 03/30/2017
api_name:
- ICLRMetaHostPolicy.GetRequestedRuntime
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRMetaHostPolicy::GetRequestedRuntime
helpviewer_keywords:
- GetRequestedRuntime method [.NET Framework hosting]
- ICLRMetaHostPolicy::GetRequestedRuntime method [.NET Framework hosting]
ms.assetid: 59ec1832-9cc1-4b5c-983d-03407e51de56
topic_type:
- apiref
ms.openlocfilehash: 1b07029990ef529ded57bc569beff1061ad0f938
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73140871"
---
# <a name="iclrmetahostpolicygetrequestedruntime-method"></a>ICLRMetaHostPolicy::GetRequestedRuntime メソッド

ホスト ポリシー、マネージド アセンブリ、バージョン文字列、および構成ストリームに基づいて、適切な共通言語ランタイム (CLR) のバージョンへのインターフェイスを提供します。 このメソッドは、実際には CLR の読み込みもアクティブ化も行いませんが、単にポリシー結果を表す[ICLRRuntimeInfo](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)インターフェイスを返します。 このメソッドは、 [Getrequestedruntimeinfo](../../../../docs/framework/unmanaged-api/hosting/getrequestedruntimeinfo-function.md)、 [getrequestedruntimeinfo](../../../../docs/framework/unmanaged-api/hosting/getrequestedruntimeversion-function.md)、 [corbindtoruntimehost](../../../../docs/framework/unmanaged-api/hosting/corbindtoruntimehost-function.md)、 [Corbindtoruntimebycfg](../../../../docs/framework/unmanaged-api/hosting/corbindtoruntimebycfg-function.md)、および[getcorrequiredversion](../../../../docs/framework/unmanaged-api/hosting/getcorrequiredversion-function.md)メソッドを置き換えます。

## <a name="syntax"></a>構文

```cpp
HRESULT GetRequestedRuntime(
    [in]  METAHOST_POLICY_FLAGS dwPolicyFlags,
    [in]  LPCWSTR pwzBinary,
    [in]  IStream *pCfgStream,
    [in, out, size_is(*pcchVersion)] LPWSTR pwzVersion,
    [in, out]  DWORD *pcchVersion,
    [out, size_is(*pcchImageVersion)] LPWSTR pwzImageVersion,
    [in, out]  DWORD *pcchImageVersion,
    [out] DWORD *pdwConfigFlags,
    [in]  REFIID  riid
    [out, iid_is(riid), retval] LPVOID *ppRuntime);
```

## <a name="parameters"></a>パラメーター

|名|説明|
|----------|-----------------|
|`dwPolicyFlags`|[in] 必須。 バインドポリシーを表す[METAHOST_POLICY_FLAGS](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)列挙体のメンバーと、任意の数の修飾子を指定します。 現在使用できるポリシーは[METAHOST_POLICY_HIGHCOMPAT](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)だけです。<br /><br /> 修飾子には、 [METAHOST_POLICY_EMULATE_EXE_LAUNCH](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)、 [METAHOST_POLICY_APPLY_UPGRADE_POLICY](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)、 [METAHOST_POLICY_SHOW_ERROR_DIALOG](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)、 [METAHOST_POLICY_USE_PROCESS_IMAGE_PATH](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)、および METAHOST_POLICY_ が含まれます。 [ENSURE_SKU_SUPPORTED](../../../../docs/framework/unmanaged-api/hosting/metahost-policy-flags-enumeration.md)。|
|`pwzBinary`|[in] オプション。 アセンブリのファイル パスを指定します。|
|`pCfgStream`|[in] オプション。 構成ファイルを <xref:System.Runtime.InteropServices.ComTypes.IStream?displayProperty=nameWithType> として指定します。|
|`pwzVersion`|[in、out] 省略可能です。 読み込む適切な CLR のバージョンを指定するか返します。|
|`pcchVersion`|[in、out] 必須です。 バッファー オーバーランを回避するため、入力として必要なサイズの `pwzVersion` を指定します。 `pwzVersion` が null の場合、`GetRequestedRuntime` が返されるときに、`pcchVersion` には必要なサイズの `pwzVersion` が含まれて、事前の割り当てが可能になります。それ以外の場合、`pcchVersion` には `pwzVersion` に書き込まれる文字数が含まれます。|
|`pwzImageVersion`|[out] 省略可能です。 `GetRequestedRuntime` が返された場合、返される[ICLRRuntimeInfo](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)インターフェイスに対応する CLR バージョンが格納されます。|
|`pcchImageVersion`|[in、out] 省略可能です。 バッファー オーバーランを回避するため、入力として `pwzImageVersion` のサイズを指定します。 `pwzImageVersion` が null の場合、`GetRequestedRuntime` が返されるとき、`pcchImageVersion` には必要なサイズの `pwzImageVersion` が含まれて、事前の割り当てが可能になります。|
|`pdwConfigFlags`|[out] 省略可能です。 バインドプロセス中に構成ファイルを使用する `GetRequestedRuntime` 場合、`pdwConfigFlags` には、 [\<スタートアップ >](../../../../docs/framework/configure-apps/file-schema/startup/startup-element.md)要素に `useLegacyV2RuntimeActivationPolicy` 属性が設定されているかどうかを示す[METAHOST_CONFIG_FLAGS](../../../../docs/framework/unmanaged-api/hosting/metahost-config-flags-enumeration.md)値と、の値が含まれます。属性。 `useLegacyV2RuntimeActivationPolicy`に関連する値を取得するには、`pdwConfigFlags` に[METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_MASK](../../../../docs/framework/unmanaged-api/hosting/metahost-config-flags-enumeration.md) MASK を適用します。|
|`riid`|から要求された[ICLRRuntimeInfo](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)インターフェイスのインターフェイス識別子 IID_ICLRRuntimeInfo を指定します。|
|`ppRuntime`|入出力`GetRequestedRuntime` が返されるときに、対応する[ICLRRuntimeInfo](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)インターフェイスへのポインターが含まれています。|

## <a name="remarks"></a>Remarks

このメソッドが正常に実行されると、`<configuration><runtime>` セクション内の構成ストリームに次の要素の 1 つ以上が存在する場合 にのみ、返されるランタイム インターフェイスの追加フラグと現在の既定のスタートアップ フラグが結合するという副作用が発生します。

- `<gcServer enabled="true"/>` により `STARTUP_SERVER_GC` が設定されます。

- `<etwEnable enabled="true"/>` により `STARTUP_ETW` が設定されます。

- `<appDomainResourceMonitoring enabled="true"/>` により `STARTUP_ARM` が設定されます。

結果として得られる既定の `STARTUP_FLAGS` 値は、上記の既定のスタートアップ フラグの一覧から設定される値のビットごとの OR の組み合わせです。

## <a name="return-value"></a>戻り値

このメソッドは、次の特定の HRESULT と、メソッドの失敗を示す HRESULT エラーも返します。

|HRESULT|説明|
|-------------|-----------------|
|S_OK|メソッドは正常に完了しました。|
|E_POINTER|`pwzVersion` は null 以外で、`pcchVersion` は null です。<br /><br /> -または-<br /><br /> `pwzImageVersion` は null 以外で、`pcchImageVersion` は null です。|
|E_INVALIDARG|`dwPolicyFlags` は `METAHOST_POLICY_HIGHCOMPAT` を指定しません。|
|ERROR_INSUFFICIENT_BUFFER|`pwzVersion` に割り当てられたメモリが不十分です。<br /><br /> -または-<br /><br /> `pwzImageVersion` に割り当てられたメモリが不十分です。|
|CLR_E_SHIM_RUNTIMELOAD|`dwPolicyFlags` には METAHOST_POLICY_APPLY_UPGRADE_POLICY が含まれ、`pwzVersion` と `pcchVersion` はいずれも null です。|

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。

**ヘッダー:** メタホスト .h

**ライブラリ:** Mscoree.dll にリソースとして含まれています

**.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]

## <a name="see-also"></a>関連項目

- [ICLRMetaHostPolicy インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrmetahostpolicy-interface.md)
- [.NET Framework 4 および 4.5 で追加された CLR ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/clr-hosting-interfaces-added-in-the-net-framework-4-and-4-5.md)
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
- [ホスティング](../../../../docs/framework/unmanaged-api/hosting/index.md)
