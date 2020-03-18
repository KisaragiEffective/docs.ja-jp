---
title: CorRefToDefCheck 列挙型
ms.date: 03/30/2017
api_name:
- CorRefToDefCheck
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorRefToDefCheck
helpviewer_keywords:
- CorRefToDefCheck enumeration [.NET Framework metadata]
ms.assetid: f9a80f1a-55af-4459-b095-8441aae16119
topic_type:
- apiref
ms.openlocfilehash: e6c3c9b842bd823e8975661964480fd801779b2d
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74450123"
---
# <a name="correftodefcheck-enumeration"></a>CorRefToDefCheck 列挙型
コードを最適化するために定義に変換される、参照先アイテムを制御するフラグを指定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorRefToDefCheck {  
    MDRefToDefDefault           = 0x00000003,  
    MDRefToDefAll               = 0xffffffff,  
    MDRefToDefNone              = 0x00000000,  
    MDTypeRefToDef              = 0x00000001,  
    MDMemberRefToDef            = 0x00000002  
} CorRefToDefCheck;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`MDRefToDefDefault`|型参照とメンバー参照を定義に変換する必要があることを指定します。 これは既定値 (`MDTypeRefToDef` &#124; `MDMemberRefToDef`) です。|  
|`MDRefToDefAll`|参照されるすべての項目を定義に変換することを指定します。|  
|`MDRefToDefNone`|参照される項目を定義に変換しないことを指定します。|  
|`MDTypeRefToDef`|型の参照のみを型定義に変換することを指定します。|  
|`MDMemberRefToDef`|メンバー参照のみを定義に変換することを指定します。 つまり、メンバー参照は、メソッド定義またはフィールド定義に変換される必要があります。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
