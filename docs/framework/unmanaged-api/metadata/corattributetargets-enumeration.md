---
title: CorAttributeTargets 列挙型
ms.date: 03/30/2017
api_name:
- CorAttributeTargets
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorAttributeTargets
helpviewer_keywords:
- CorAttributeTargets enumeration [.NET Framework metadata]
ms.assetid: 694c0fa0-7011-41a9-9dfd-f0e16ea574b5
topic_type:
- apiref
ms.openlocfilehash: 5f83cb96e39b257a1d35786130cd5ed31d071de7
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74443871"
---
# <a name="corattributetargets-enumeration"></a>CorAttributeTargets 列挙型
属性を適用できるアプリケーション要素を指定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorAttributeTargets  
{  
    catAssembly            = 0x0001,  
    catModule              = 0x0002,  
    catClass               = 0x0004,  
    catStruct              = 0x0008,  
    catEnum                = 0x0010,  
    catConstructor         = 0x0020,  
    catMethod              = 0x0040,  
    catProperty            = 0x0080,  
    catField               = 0x0100,  
    catEvent               = 0x0200,  
    catInterface           = 0x0400,  
    catParameter           = 0x0800,  
    catDelegate            = 0x1000,  
    catGenericParameter    = 0x4000,  
  
    catAll                 =   
        catAssembly | catModule | catClass | catStruct |   
        catEnum | catConstructor | catMethod | catProperty |   
        catField | catEvent | catInterface | catParameter |   
        catDelegate | catGenericParameter,  
  
    catClassMembers        =   
        catClass | catStruct | catEnum | catConstructor |   
        catMethod | catProperty | catField | catEvent |   
        catDelegate | catInterface  
  
} CorAttributeTargets;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`catAssembly`|アセンブリに属性を適用できます。|  
|`catModule`|属性は、移植可能な実行可能ファイル (.dll または .exe) モジュールに適用できます。|  
|`catClass`|クラスに属性を適用できます。|  
|`catStruct`|構造体に属性を適用できます。つまり、値型です。|  
|`catEnum`|属性を列挙に適用できます。|  
|`catConstructor`|コンストラクターに属性を適用できます。|  
|`catMethod`|メソッドに属性を適用できます。|  
|`catProperty`|属性をプロパティに適用できます。|  
|`catField`|フィールドに属性を適用できます。|  
|`catEvent`|イベントに属性を適用できます。|  
|`catInterface`|インターフェイスに属性を適用できます。|  
|`catParameter`|パラメーターに属性を適用できます。|  
|`catDelegate`|デリゲートに属性を適用できます。|  
|`catGenericParameter`|ジェネリックパラメーターに属性を適用できます。|  
|`catAll`|任意のアプリケーション要素に属性を適用できます。|  
|`catClassMembers`|属性は、クラスのメンバーに適用できます。|  
  
## <a name="remarks"></a>コメント  
 `CorAttributeTargets` 列挙値をビットごとの OR 演算と組み合わせて、適切な組み合わせを取得できます。  
  
 `CorAttributeTargets` は、マネージ <xref:System.AttributeTargets?displayProperty=nameWithType> 列挙体と同じです。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
