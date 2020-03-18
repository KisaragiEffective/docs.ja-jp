---
title: CorImportOptions 列挙型
ms.date: 03/30/2017
api_name:
- CorImportOptions
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorImportOptions
helpviewer_keywords:
- CorImportOptions enumeration [.NET Framework metadata]
ms.assetid: 4e5d03cb-97c9-4ff4-8dbd-17d94ee374d3
topic_type:
- apiref
ms.openlocfilehash: 44d1776e2902988353ef4fd58aca20e56203b9da
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74442850"
---
# <a name="corimportoptions-enumeration"></a>CorImportOptions 列挙型
現在のスコープ外のアセンブリのインポート中の動作を制御するフラグ値が格納されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorImportOptions {  
  
    MDImportOptionDefault                = 0x00000000,  
    MDImportOptionAll                    = 0xFFFFFFFF,  
    MDImportOptionAllTypeDefs            = 0x00000001,  
    MDImportOptionAllMethodDefs          = 0x00000002,  
    MDImportOptionAllFieldDefs           = 0x00000004,  
    MDImportOptionAllProperties          = 0x00000008,  
    MDImportOptionAllEvents              = 0x00000010,  
    MDImportOptionAllCustomAttributes    = 0x00000020,  
    MDImportOptionAllExportedTypes       = 0x00000040  
  
} CorImportOptions;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`MDImportOptionDefault`|削除されたレコードをスキップする既定の動作を示します。|  
|`MDImportOptionAll`|すべてのメタデータを列挙する必要があることを示します。|  
|`MDImportOptionAllTypeDefs`|削除された Typedef も含め、すべての Typedef を列挙する必要があることを示します。|  
|`MDImportOptionAllMethodDefs`|削除されたものを含むすべての MethodDefs を列挙する必要があることを示します。|  
|`MDImportOptionAllFieldDefs`|削除されたものも含め、すべての FieldDefs を列挙する必要があることを示します。|  
|`MDImportOptionAllProperties`|削除されたものも含め、すべての PropertyDefs を列挙する必要があることを示します。|  
|`MDImportOptionAllEvents`|削除されたものも含め、すべての EventDefs を列挙する必要があることを示します。|  
|`MDImportOptionAllCustomAttributes`|削除された属性も含め、すべてのカスタム属性を列挙する必要があることを示します。|  
|`MDImportOptionAllExportedTypes`|削除された型も含めて、エクスポートされたすべての型を列挙する必要があることを示します。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
