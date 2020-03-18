---
title: CorSerializationType 列挙型
ms.date: 03/30/2017
api_name:
- CorSerializationType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorSerializationType
helpviewer_keywords:
- CorSerializationType enumeration [.NET Framework metadata]
ms.assetid: 6b1fcd11-c7fb-4be2-8910-abc862d4caf4
topic_type:
- apiref
ms.openlocfilehash: 064374285216e9fb054b299937087f1ca7c351a4
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74432879"
---
# <a name="corserializationtype-enumeration"></a>CorSerializationType 列挙型
共通言語ランタイムによってオブジェクトをシリアル化する方法を指定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorSerializationType {  
  
    SERIALIZATION_TYPE_UNDEFINED     = 0,  
    SERIALIZATION_TYPE_BOOLEAN       = ELEMENT_TYPE_BOOLEAN,  
    SERIALIZATION_TYPE_CHAR          = ELEMENT_TYPE_CHAR,  
    SERIALIZATION_TYPE_I1            = ELEMENT_TYPE_I1,  
    SERIALIZATION_TYPE_U1            = ELEMENT_TYPE_U1,  
    SERIALIZATION_TYPE_I2            = ELEMENT_TYPE_I2,  
    SERIALIZATION_TYPE_U2            = ELEMENT_TYPE_U2,  
    SERIALIZATION_TYPE_I4            = ELEMENT_TYPE_I4,  
    SERIALIZATION_TYPE_U4            = ELEMENT_TYPE_U4,  
    SERIALIZATION_TYPE_I8            = ELEMENT_TYPE_I8,  
    SERIALIZATION_TYPE_U8            = ELEMENT_TYPE_U8,  
    SERIALIZATION_TYPE_R4            = ELEMENT_TYPE_R4,  
    SERIALIZATION_TYPE_R8            = ELEMENT_TYPE_R8,  
    SERIALIZATION_TYPE_STRING        = ELEMENT_TYPE_STRING,  
    SERIALIZATION_TYPE_SZARRAY       = ELEMENT_TYPE_SZARRAY,  
    SERIALIZATION_TYPE_TYPE          = 0x50,  
    SERIALIZATION_TYPE_TAGGED_OBJECT = 0x51,  
    SERIALIZATION_TYPE_FIELD         = 0x53,  
    SERIALIZATION_TYPE_PROPERTY      = 0x54,  
    SERIALIZATION_TYPE_ENUM          = 0x55  
  
} CorSerializationType;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`SERIALIZATION_TYPE_UNDEFINED`|オブジェクトのシリアル化が定義されていません。|  
|`SERIALIZATION_TYPE_BOOLEAN`|オブジェクトはブール型としてシリアル化されます|  
|`SERIALIZATION_TYPE_CHAR`|オブジェクトは文字型としてシリアル化されます。|  
|`SERIALIZATION_TYPE_I1`|オブジェクトは符号付き1バイト整数としてシリアル化されます。|  
|`SERIALIZATION_TYPE_U1`|オブジェクトは、符号なし1バイト整数としてシリアル化されます。|  
|`SERIALIZATION_TYPE_I2`|オブジェクトは、符号付き2バイト整数としてシリアル化されます。|  
|`SERIALIZATION_TYPE_U2`|オブジェクトは、符号なし2バイト整数としてシリアル化されます。|  
|`SERIALIZATION_TYPE_I4`|オブジェクトは、符号付き4バイト整数としてシリアル化されます。|  
|`SERIALIZATION_TYPE_U4`|オブジェクトは、4バイトの符号なし整数としてシリアル化されます。|  
|`SERIALIZATION_TYPE_I8`|オブジェクトは、符号付き8バイト整数としてシリアル化されます。|  
|`SERIALIZATION_TYPE_U8`|オブジェクトは、8バイトの符号なし整数としてシリアル化されます。|  
|`SERIALIZATION_TYPE_R4`|オブジェクトは、4バイトの浮動小数点としてシリアル化されます。|  
|`SERIALIZATION_TYPE_R8`|オブジェクトは8バイト浮動小数点としてシリアル化されます。|  
|`SERIALIZATION_TYPE_STRING`|オブジェクトは System.string 型としてシリアル化されます。|  
|`SERIALIZATION_TYPE_SZARRAY`|オブジェクトは、1次元の下限の配列としてシリアル化されます。|  
|`SERIALIZATION_TYPE_TYPE`|オブジェクトは、ジェネリック型としてシリアル化されます。|  
|`SERIALIZATION_TYPE_TAGGED_OBJECT`|オブジェクトはタグ付きオブジェクトとしてシリアル化されます。|  
|`SERIALIZATION_TYPE_FIELD`|オブジェクトはフィールドとしてシリアル化されます。|  
|`SERIALIZATION_TYPE_PROPERTY`|オブジェクトはプロパティとしてシリアル化されます。|  
|`SERIALIZATION_TYPE_ENUM`|オブジェクトは列挙体としてシリアル化されます。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
