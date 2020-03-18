---
title: COR_PRF_STATIC_TYPE 列挙型
ms.date: 03/30/2017
api_name:
- COR_PRF_STATIC_TYPE
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_STATIC_TYPE
helpviewer_keywords:
- COR_PRF_STATIC_TYPE enumeration [.NET Framework profiling]
ms.assetid: 441d7809-5b65-41a5-ba64-2910a8008315
topic_type:
- apiref
ms.openlocfilehash: 880c9bd186d6cb2acb277e9cc55d3063fb8d51d8
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76867037"
---
# <a name="cor_prf_static_type-enumeration"></a>COR_PRF_STATIC_TYPE 列挙型
フィールドが静的であるかどうかを示し、静的な場合は、フィールドに適用される静的なクオリティを示します。 これらの値は、ビットごとの OR 演算を使用して組み合わせて、フィールドに複数の異なる静的品質があることを示すことができます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum {  
    COR_PRF_FIELD_NOT_A_STATIC = 0x0,  
    COR_PRF_FIELD_APP_DOMAIN_STATIC = 0x1,  
    COR_PRF_FIELD_THREAD_STATIC = 0x2,  
    COR_PRF_FIELD_CONTEXT_STATIC = 0x4,  
    COR_PRF_FIELD_RVA_STATIC = 0x8  
} COR_PRF_STATIC_TYPE;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`COR_PRF_FIELD_NOT_A_STATIC`|フィールドは静的ではありません。|  
|`COR_PRF_FIELD_APP_DOMAIN_STATIC`|フィールドは [アプリケーションドメイン-静的] です。|  
|`COR_PRF_FIELD_THREAD_STATIC`|フィールドはスレッド静的です。|  
|`COR_PRF_FIELD_CONTEXT_STATIC`|フィールドは、コンテキスト静的です。|  
|`COR_PRF_FIELD_RVA_STATIC`|フィールドは、相対仮想アドレス (RVA)-static です。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [列挙型のプロファイリング](profiling-enumerations.md)
