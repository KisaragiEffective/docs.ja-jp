---
title: CorCheckDuplicatesFor 列挙型
ms.date: 03/30/2017
api_name:
- CorCheckDuplicatesFor
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorCheckDuplicatesFor
helpviewer_keywords:
- CorCheckDuplicatesFor enumeration [.NET Framework metadata]
ms.assetid: d8ec8d3c-70f7-4cc6-9957-68068fd8f49c
topic_type:
- apiref
ms.openlocfilehash: 6b551743227dc1c6069796038782a515e6dbe8c4
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74443777"
---
# <a name="corcheckduplicatesfor-enumeration"></a>CorCheckDuplicatesFor 列挙型
重複をチェックするメタデータトークンを指定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorCheckDuplicatesFor {  
  
    MDDupAll                    = 0xffffffff,  
    MDDupENC                    = MDDupAll,  
    MDNoDupChecks               = 0x00000000,  
    MDDupTypeDef                = 0x00000001,  
    MDDupInterfaceImpl          = 0x00000002,  
    MDDupMethodDef              = 0x00000004,  
    MDDupTypeRef                = 0x00000008,  
    MDDupMemberRef              = 0x00000010,  
    MDDupCustomAttribute        = 0x00000020,  
    MDDupParamDef               = 0x00000040,  
    MDDupPermission             = 0x00000080,  
    MDDupProperty               = 0x00000100,  
    MDDupEvent                  = 0x00000200,  
    MDDupFieldDef               = 0x00000400,  
    MDDupSignature              = 0x00000800,  
    MDDupModuleRef              = 0x00001000,  
    MDDupTypeSpec               = 0x00002000,  
    MDDupImplMap                = 0x00004000,  
    MDDupAssemblyRef            = 0x00008000,  
    MDDupFile                   = 0x00010000,  
    MDDupExportedType           = 0x00020000,  
    MDDupManifestResource       = 0x00040000,  
    MDDupGenericParam           = 0x00080000,  
    MDDupMethodSpec             = 0x00100000,  
    MDDupGenericParamConstraint = 0x00200000,  
  
    MDDupAssembly               = 0x10000000,  
  
    MDDupDefault =   
        MDNoDupChecks | MDDupTypeRef | MDDupMemberRef |   
        MDDupSignature | MDDupTypeSpec | MDDupMethodSpec  
  
} CorCheckDuplicatesFor;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`MDDupAll`|すべてのメタデータトークンの重複を確認します。|  
|`MDDupENC`|使用しません。|  
|`MDNoDupChecks`|メタデータトークンの重複をチェックしないでください。|  
|`MDDupTypeDef`|`mdTypeDef` トークンの重複を確認します。|  
|`MDDupInterfaceImpl`|`mdInterfaceImpl` トークンの重複を確認します。|  
|`MDDupMethodDef`|`mdMethodDef` トークンの重複を確認します。|  
|`MDDupTypeRef`|`mdTypeRef` トークンの重複を確認します。|  
|`MDDupMemberRef`|`mdMemberRef` トークンの重複を確認します。|  
|`MDDupCustomAttribute`|`mdCustomAttribute` トークンの重複を確認します。|  
|`MDDupParamDef`|`mdParamDef` トークンの重複を確認します。|  
|`MDDupPermission`|`mdPermission` トークンの重複を確認します。|  
|`MDDupProperty`|`mdProperty` トークンの重複を確認します。|  
|`MDDupEvent`|`mdEvent` トークンの重複を確認します。|  
|`MDDupFieldDef`|`mdFieldDef` トークンの重複を確認します。|  
|`MDDupSignature`|`mdSignature` トークンの重複を確認します。|  
|`MDDupModuleRef`|`mdModuleRef` トークンの重複を確認します。|  
|`MDDupTypeSpec`|`mdTypeSpec` トークンの重複を確認します。|  
|`MDDupImplMap`|`mdImplMap` トークンの重複を確認します。|  
|`MDDupAssemblyRef`|`mdAssemblyRef` トークンの重複を確認します。|  
|`MDDupFile`|`mdFile` トークンの重複を確認します。|  
|`MDDupExportedType`|`mdExportedType` トークンの重複を確認します。|  
|`MDDupManifestResource`|`mdManifestResource` トークンの重複を確認します。|  
|`MDDupGenericParam`|`mdGenericParam` トークンの重複を確認します。|  
|`MDDupMethodSpec`|`mdMethodSpec` トークンの重複を確認します。|  
|`MDDupGenericParamConstraint`|`mdGenericParamConstraint` トークンの重複を確認します。|  
|`MDDupAssembly`|`mdAssembly` トークンの重複を確認します。|  
|`MDDupDefault`|`mdMemberRef`、`mdTypeRef`、`mdSignature`、`mdTypeSpec`、および `mdMethodSpec` トークンの重複を確認します。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
