---
title: IDENTITY_ATTRIBUTE 構造体
ms.date: 03/30/2017
api_name:
- IDENTITY_ATTRIBUTE
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IDENTITY_ATTRIBUTE
helpviewer_keywords:
- IDENTITY_ATTRIBUTE structure [.NET Framework fusion]
ms.assetid: 1ee7c434-9681-4fa8-badd-652cb1a9742b
topic_type:
- apiref
ms.openlocfilehash: 8b7edf1cc642228c4a79c855b51727264f31741c
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73107982"
---
# <a name="identity_attribute-structure"></a>IDENTITY_ATTRIBUTE 構造体
[IDefinitionIdentity](idefinitionidentity-interface.md)インスタンスに関するメタデータ属性情報を格納します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef struct _IDENTITY_ATTRIBUTE {  
    LPCWSTR  pszNamespace;  
    LPCWSTR  pszName;  
    LPCWSTR  pszValue;  
} IDENTITY_ATTRIBUTE;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`pszNamespace`|属性が含まれている名前空間を含む null で終わる文字列へのポインター。|  
|`pszName`|属性の名前を含む null で終わる文字列へのポインター。|  
|`pszValue`|属性の値を格納している null で終わる文字列へのポインター。|  
  
## <a name="remarks"></a>Remarks  
 `IDENTITY_ATTRIBUTE` 構造体には、null で終わる文字列への3つのポインターが含まれています。 これら3つの文字列は、1つの属性を表します。  
  
 `IDENTITY_ATTRIBUTE` 構造体のインスタンスは、 [IDENTITY_ATTRIBUTE_BLOB](identity-attribute-blob-structure.md)構造体のインスタンスに関連付けられています。 `IDENTITY_ATTRIBUTE` 構造体には実際の文字列が含まれ、対応する `IDENTITY_ATTRIBUTE_BLOB` 構造体には、`IDENTITY_ATTRIBUTE` 構造体に示されている3つの文字列へのオフセットが一覧表示されます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** 分離 .h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IDefinitionIdentity インターフェイス](idefinitionidentity-interface.md)
- [IDENTITY_ATTRIBUTE_BLOB 構造体](identity-attribute-blob-structure.md)
- [Fusion 構造体](fusion-structures.md)
