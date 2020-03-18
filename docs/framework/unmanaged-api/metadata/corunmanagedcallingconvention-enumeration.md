---
title: CorUnmanagedCallingConvention 列挙型
ms.date: 03/30/2017
api_name:
- CorUnmanagedCallingConvention
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorUnmanagedCallingConvention
helpviewer_keywords:
- CorUnmanagedCallingConvention enumeration [.NET Framework metadata]
ms.assetid: 83058790-160b-4703-a5eb-74b66acbdfa9
topic_type:
- apiref
ms.openlocfilehash: 58d30e71929d314ee36adb9f83270858ff8a161b
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74442440"
---
# <a name="corunmanagedcallingconvention-enumeration"></a>CorUnmanagedCallingConvention 列挙型
アンマネージコードの呼び出し規約を指定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorUnmanagedCallingConvention {  
  
    IMAGE_CEE_UNMANAGED_CALLCONV_C         = 0x1,  
    IMAGE_CEE_UNMANAGED_CALLCONV_STDCALL   = 0x2,  
    IMAGE_CEE_UNMANAGED_CALLCONV_THISCALL  = 0x3,  
    IMAGE_CEE_UNMANAGED_CALLCONV_FASTCALL  = 0x4,  
  
    IMAGE_CEE_CS_CALLCONV_C                = 0x1,  
    IMAGE_CEE_CS_CALLCONV_STDCALL          = 0x2,  
    IMAGE_CEE_CS_CALLCONV_THISCALL         = 0x3,  
    IMAGE_CEE_CS_CALLCONV_FASTCALL         = 0x4  
  
} CorUnmanagedCallingConvention;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`IMAGE_CEE_UNMANAGED_CALLCONV_C`|C 言語の呼び出し規約。|  
|`IMAGE_CEE_UNMANAGED_CALLCONV_STDCALL`|標準の呼び出し規約。|  
|`IMAGE_CEE_UNMANAGED_CALLCONV_THISCALL`|"This" 呼び出し規約。|  
|`IMAGE_CEE_UNMANAGED_CALLCONV_FASTCALL`|"高速" 呼び出し規約。|  
|`IMAGE_CEE_CS_CALLCONV_C`|使用しません。|  
|`IMAGE_CEE_CS_CALLCONV_STDCALL`|使用しません。|  
|`IMAGE_CEE_CS_CALLCONV_THISCALL`|使用しません。|  
|`IMAGE_CEE_CS_CALLCONV_FASTCALL`|使用しません。|  
  
## <a name="remarks"></a>コメント  
 CLR では、.NET Framework バージョン1.0 での "高速" 呼び出し規約はサポートされていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
