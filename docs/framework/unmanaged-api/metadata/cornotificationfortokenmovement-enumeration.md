---
title: CorNotificationForTokenMovement 列挙型
ms.date: 03/30/2017
api_name:
- CorNotificationForTokenMovement
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorNotificationForTokenMovement
helpviewer_keywords:
- CorNotificationForTokenMovement enumeration [.NET Framework metadata]
ms.assetid: 1edd1670-976a-4fc8-bef7-7c41e60ad989
topic_type:
- apiref
ms.openlocfilehash: 411fad0accb59431f776c5bd66e8bd3027ddd907
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74450146"
---
# <a name="cornotificationfortokenmovement-enumeration"></a>CorNotificationForTokenMovement 列挙型
トークンの再マップが発生したときにメタデータ API クライアントに送信される通知を指定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorNotificationForTokenMovement {  
  
    MDNotifyDefault             = 0x0000000f,  
    MDNotifyAll                 = 0xffffffff,  
    MDNotifyNone                = 0x00000000,  
    MDNotifyMethodDef           = 0x00000001,  
    MDNotifyMemberRef           = 0x00000002,  
    MDNotifyFieldDef            = 0x00000004,  
    MDNotifyTypeRef             = 0x00000008,  
  
    MDNotifyTypeDef             = 0x00000010,  
    MDNotifyParamDef            = 0x00000020,  
    MDNotifyInterfaceImpl       = 0x00000040,  
    MDNotifyProperty            = 0x00000080,  
    MDNotifyEvent               = 0x00000100,  
    MDNotifySignature           = 0x00000200,  
    MDNotifyTypeSpec            = 0x00000400,  
    MDNotifyCustomAttribute     = 0x00000800,  
    MDNotifySecurityValue       = 0x00001000,  
    MDNotifyPermission          = 0x00002000,  
    MDNotifyModuleRef           = 0x00004000,  
  
    MDNotifyNameSpace           = 0x00008000,  
  
    MDNotifyAssemblyRef         = 0x01000000,  
    MDNotifyFile                = 0x02000000,  
    MDNotifyExportedType        = 0x04000000,  
    MDNotifyResource            = 0x08000000  
  
} CorNotificationForTokenMovement;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`MDNotifyDefault`|`mdTypeRef`、`mdMethodDef`、`mdMemberRef`、または `mdFieldDef` のトークンが移動したときに通知します。|  
|`MDNotifyAll`|任意のトークンが移動したときに通知します。|  
|`MDNotifyNone`|トークンの移動時に通知しません。|  
|`MDNotifyMethodDef`|`mdMethodDef` トークンが移動したときに通知します。|  
|`MDNotifyMemberRef`|`mdMemberRef` トークンが移動したときに通知します。|  
|`MDNotifyFieldDef`|`mdFieldDef` トークンが移動したときに通知します。|  
|`MDNotifyTypeRef`|`mdTypeRef` トークンが移動したときに通知します。|  
|`MDNotifyTypeDef`|`mdTypeDef` トークンが移動したときに通知します。|  
|`MDNotifyParamDef`|`mdParamDef` トークンが移動したときに通知します。|  
|`MDNotifyInterfaceImpl`|`mdInterfaceImpl` トークンが移動したときに通知します。|  
|`MDNotifyProperty`|`mdProperty` トークンが移動したときに通知します。|  
|`MDNotifyEvent`|`mdEvent` トークンが移動したときに通知します。|  
|`MDNotifySignature`|`mdSignature` トークンが移動したときに通知します。|  
|`MDNotifyTypeSpec`|`mdTypeSpec` トークンが移動したときに通知します。|  
|`MDNotifyCustomAttribute`|`mdCustomAttribute` トークンが移動したときに通知します。|  
|`MDNotifySecurityValue`|`mdSecurityValue` トークンが移動したときに通知します。|  
|`MDNotifyPermission`|`mdPermission` トークンが移動したときに通知します。|  
|`MDNotifyModuleRef`|`mdModuleRef` トークンが移動したときに通知します。|  
|`MDNotifyNameSpace`|`mdNameSpace` トークンが移動したときに通知します。|  
|`MDNotifyAssemblyRef`|`mdAssemblyRef` トークンが移動したときに通知します。|  
|`MDNotifyFile`|`mdFile` トークンが移動したときに通知します。|  
|`MDNotifyExportedType`|`mdExportedType` トークンが移動したときに通知します。|  
|`MDNotifyResource`|`mdManifestResource` トークンが移動したときに通知します。|  
  
## <a name="remarks"></a>コメント  
 メタデータのマージ中に、トークンが再マップ (移動) される場合があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
