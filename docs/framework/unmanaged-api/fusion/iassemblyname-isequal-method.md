---
title: IAssemblyName::IsEqual メソッド
ms.date: 03/30/2017
api_name:
- IAssemblyName.IsEqual
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyName::IsEqual
helpviewer_keywords:
- IsEqual method [.NET Framework fusion]
- IAssemblyName::IsEqual method [.NET Framework fusion]
ms.assetid: 6dfc220f-d0d4-45b3-bfce-5829f817766f
topic_type:
- apiref
ms.openlocfilehash: 23bc251053dd27a7c5accb48ab4759ecdb79fe09
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73134306"
---
# <a name="iassemblynameisequal-method"></a>IAssemblyName::IsEqual メソッド
指定した比較フラグに基づいて、指定した[IAssemblyName](iassemblyname-interface.md)オブジェクトがこの `IAssemblyName`と等しいかどうかを判断します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT IsEqual (  
    [in] IAssemblyName  *pName,  
    [in] DWORD          dwCmpFlags  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pName`  
 からこの `IAssemblyName`と比較する `IAssemblyName` オブジェクト。  
  
 `dwCmpFlags`  
 から比較に影響を与える[ASM_CMP_FLAGS](asm-cmp-flags-enumeration.md)値のビットごとの組み合わせ。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Fusion. h  
  
 **.Net Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IAssemblyName インターフェイス](iassemblyname-interface.md)
- [Fusion 列挙型](fusion-enumerations.md)
