---
title: ICLRRuntimeInfo::BindAsLegacyV2Runtime メソッド
ms.date: 03/30/2017
api_name:
- ICLRRuntimeInfo.BindAsLegacyV2Runtime
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeInfo::BindAsLegacyV2Runtime
helpviewer_keywords:
- ICLRRuntimeInfo::BindAsLegacyV2Runtime method [.NET Framework hosting]
- BindAsLegacyV2Runtime method [.NET Framework hosting]
ms.assetid: 65fd55ac-4a24-4479-9384-a2e8013bfb2b
topic_type:
- apiref
ms.openlocfilehash: d37ec8e17e62f58212a5f79f4d6b6aa75f57bf7c
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73120254"
---
# <a name="iclrruntimeinfobindaslegacyv2runtime-method"></a>ICLRRuntimeInfo::BindAsLegacyV2Runtime メソッド
すべてのレガシ共通言語ランタイム (CLR) バージョン2アクティブ化ポリシーの決定に現在のランタイムをバインドします。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT BindAsLegacyV2Runtime ();  
```  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、次の特定の Hresult を返します。  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|バインドが成功したか、またはこのランタイムが従来の CLR バージョン2アクティブ化ポリシーのランタイムとして既にバインドされています。|  
|CLR_E_SHIM_LEGACYRUNTIMEALREADYBOUND|以前の CLR バージョン2のアクティブ化ポリシーに、別のランタイムが既にバインドされています。|  
  
## <a name="remarks"></a>Remarks  
 現在のランタイムが、すべてのレガシ CLR バージョン2アクティブ化ポリシーの決定に対して既にバインドされている場合 (たとえば、構成ファイルの[\<startup > 要素](../../../../docs/framework/configure-apps/file-schema/startup/startup-element.md)の `useLegacyV2RuntimeActivationPolicy` 属性を使用した場合)、このメソッドはエラー結果を返しません。代わりに、メソッドが従来のアクティブ化ポリシーに正常にバインドされている場合と同様に、結果は S_OK になります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** メタホスト .h  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRRuntimeInfo インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-interface.md)
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
- [ホスティング](../../../../docs/framework/unmanaged-api/hosting/index.md)
- [\<startup> 要素](../../../../docs/framework/configure-apps/file-schema/startup/startup-element.md)
