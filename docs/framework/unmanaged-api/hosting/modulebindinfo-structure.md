---
title: ModuleBindInfo 構造体
ms.date: 03/30/2017
api_name:
- ModuleBindInfo
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ModuleBindInfo
helpviewer_keywords:
- ModuleBindInfo structure [.NET Framework hosting]
ms.assetid: 632d4adc-dbc9-4ce8-9397-abc3285c1c69
topic_type:
- apiref
ms.openlocfilehash: ae40d8adaae70ccff6e8058858a506267d58873f
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73133745"
---
# <a name="modulebindinfo-structure"></a>ModuleBindInfo 構造体
参照されるモジュールとそれを含むアセンブリに関する詳細情報を提供します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef struct _ModuleBindInfo {  
    DWORD    dwAppDomainId;  
    LPCWSTR  lpAssemblyIdentity;  
    LPCWSTR  lpModuleName  
} ModuleBindInfo;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`dwAppDomainId`|参照先のモジュールを読み込む[IHostAssemblyStore::P rovidemodule](../../../../docs/framework/unmanaged-api/hosting/ihostassemblystore-providemodule-method.md)メソッドの呼び出しによって返される `IStream` の一意の識別子。|  
|`lpAssemblyIdentity`|参照されたモジュールを含むアセンブリの一意の識別子。|  
|`lpModuleName`|参照されているモジュールの名前。|  
  
## <a name="remarks"></a>Remarks  
 `ModuleBindInfo` は、`IHostAssemblyStore::ProvideModule`にパラメーターとして渡されます。 ホストは、共通言語ランタイム (CLR) に `dwAppDomainId` 一意の識別子を提供します。 [IHostAssemblyStore::P rovideassembly](../../../../docs/framework/unmanaged-api/hosting/ihostassemblystore-provideassembly-method.md)メソッドの呼び出しが返されると、ランタイムは識別子を使用して、`IStream` の内容がマップされているかどうかを判断します。 その場合、ランタイムはストリームを再マップするのではなく、既存のコピーを読み込みます。 また、ランタイムは、`IHostAssemblyStore::ProvideAssembly` メソッドへの呼び出しから返されるストリームの参照キーとして、この識別子を使用します。 このため、識別子は、モジュール要求とアセンブリ要求に対して一意である必要があります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ホスト構造体](../../../../docs/framework/unmanaged-api/hosting/hosting-structures.md)
- [AssemblyBindInfo 構造体](../../../../docs/framework/unmanaged-api/hosting/assemblybindinfo-structure.md)
- [ICLRAssemblyIdentityManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrassemblyidentitymanager-interface.md)
- [ICLRAssemblyReferenceList インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrassemblyreferencelist-interface.md)
- [IHostAssemblyManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihostassemblymanager-interface.md)
