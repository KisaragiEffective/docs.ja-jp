---
title: CorGenericParamAttr 列挙型
ms.date: 03/30/2017
api_name:
- CorGenericParamAttr
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorGenericParamAttr
helpviewer_keywords:
- CorGenericParamAttr enumeration [.NET Framework metadata]
ms.assetid: 36c76266-71d8-48dc-bd89-54943fa659c1
topic_type:
- apiref
ms.openlocfilehash: e4abf876681d5b04555c9f030a94b722874e326e
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74450285"
---
# <a name="corgenericparamattr-enumeration"></a>CorGenericParamAttr 列挙型
[IMetaDataEmit2::D efineGenericParam](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-definegenericparam-method.md)の呼び出しで使用される、ジェネリック型の <xref:System.Type> パラメーターを記述する値を格納します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorGenericParamAttr {  
  
    gpVarianceMask                     =   0x0003,  
    gpNonVariant                       =   0x0000,   
    gpCovariant                        =   0x0001,  
    gpContravariant                    =   0x0002,  
  
    gpSpecialConstraintMask            =   0x001C,  
    gpNoSpecialConstraint              =   0x0000,  
    gpReferenceTypeConstraint          =   0x0004,   
    gpNotNullableValueTypeConstraint   =   0x0008,  
    gpDefaultConstructorConstraint     =   0x0010  
  
} CorGenericParamAttr;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`gpVarianceMask`|パラメーターの分散は、インターフェイスとデリゲートのジェネリックパラメーターにのみ適用されます。|  
|`gpNonVariant`|分散が存在しないことを示します。|  
|`gpCovariant`|共変性を示します。|  
|`gpContravariant`|反変性を示します。|  
|`gpSpecialConstraintMask`|特殊な制約は、任意の <xref:System.Type> パラメーターに適用できます。|  
|`gpNoSpecialConstraint`|<xref:System.Type> パラメーターに制約が適用されないことを示します。|  
|`gpReferenceTypeConstraint`|<xref:System.Type> パラメーターが参照型である必要があることを示します。|  
|`gpNotNullableValueTypeConstraint`|<xref:System.Type> パラメーターを null 値にすることができない値型である必要があることを示します。|  
|`gpDefaultConstructorConstraint`|<xref:System.Type> パラメーターに、パラメーターをとらない既定のパブリックコンストラクターが必要であることを示します。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
