---
title: CorDebugMappingResult 列挙型
ms.date: 03/30/2017
api_name:
- CorDebugMappingResult
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugMappingResult
helpviewer_keywords:
- CorDebugMappingResult enumeration [.NET Framework debugging]
ms.assetid: 701281dd-2936-45c8-a1f0-3bf7332b093b
topic_type:
- apiref
ms.openlocfilehash: 317dc2fe8403ae25949410423f1a28ad365caf6a
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76789308"
---
# <a name="cordebugmappingresult-enumeration"></a>CorDebugMappingResult 列挙型
命令ポインター (IP) の値が得られた方法の詳細を提供します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorDebugMappingResult {  
    MAPPING_PROLOG              = 0x1,  
    MAPPING_EPILOG              = 0x2,  
    MAPPING_NO_INFO             = 0x4,  
    MAPPING_UNMAPPED_ADDRESS    = 0x8,  
    MAPPING_EXACT               = 0x10,  
    MAPPING_APPROXIMATE         = 0x20,  
} CorDebugMappingResult;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`MAPPING_PROLOG`|ネイティブコードはプロローグ内にあるため、IP の値は0です。|  
|`MAPPING_EPILOG`|ネイティブコードはエピローグ内にあるため、IP の値はメソッドの最後の命令のアドレスになります。|  
|`MAPPING_NO_INFO`|メソッドに使用できるマッピング情報がないため、IP の値は0になります。|  
|`MAPPING_UNMAPPED_ADDRESS`|メソッドのマッピング情報は存在しますが、現在のアドレスを MSIL (Microsoft 中間言語) コードにマップすることはできません。 IP の値は0です。|  
|`MAPPING_EXACT`|メソッドが MSIL コードに厳密にマップされているか、フレームが解釈されているため、IP の値は正確です。|  
|`MAPPING_APPROXIMATE`|メソッドは正常にマップされましたが、IP の値は概数である可能性があります。|  
  
## <a name="remarks"></a>コメント  
 指示ポインターの値を取得するには、「ツール」を[使用します](icordebugilframe-getip-method.md)。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [列挙型のデバッグ](debugging-enumerations.md)
