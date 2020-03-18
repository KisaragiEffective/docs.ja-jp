---
title: CorCallingConvention 列挙型
ms.date: 03/30/2017
api_name:
- CorCallingConvention
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorCallingConvention
helpviewer_keywords:
- CorCallingConvention enumeration [.NET Framework metadata]
ms.assetid: 69156fbf-7219-43bf-b4b8-b13f1a2fcb86
topic_type:
- apiref
ms.openlocfilehash: 9d4690cb6adedc77717e577d409cb52b18b1b5ca
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74443837"
---
# <a name="corcallingconvention-enumeration"></a>CorCallingConvention 列挙型
マネージド コードで作成される呼び出し規則のタイプを記述する値が格納されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorCallingConvention  
{  
    IMAGE_CEE_CS_CALLCONV_DEFAULT       = 0x0,  
  
    IMAGE_CEE_CS_CALLCONV_VARARG        = 0x5,  
    IMAGE_CEE_CS_CALLCONV_FIELD         = 0x6,  
    IMAGE_CEE_CS_CALLCONV_LOCAL_SIG     = 0x7,  
    IMAGE_CEE_CS_CALLCONV_PROPERTY      = 0x8,  
    IMAGE_CEE_CS_CALLCONV_UNMGD         = 0x9,  
    IMAGE_CEE_CS_CALLCONV_GENERICINST   = 0xa,  
    IMAGE_CEE_CS_CALLCONV_NATIVEVARARG  = 0xb,  
    IMAGE_CEE_CS_CALLCONV_MAX           = 0xc,  
  
    IMAGE_CEE_CS_CALLCONV_MASK          = 0x0f,  
    IMAGE_CEE_CS_CALLCONV_HASTHIS       = 0x20,  
    IMAGE_CEE_CS_CALLCONV_EXPLICITTHIS  = 0x40,  
    IMAGE_CEE_CS_CALLCONV_GENERIC       = 0x10  
  
} CorCallingConvention;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`IMAGE_CEE_CS_CALLCONV_DEFAULT`|既定の呼び出し規約を示します。|  
|`IMAGE_CEE_CS_CALLCONV_VARARG`|メソッドが可変個のパラメーターを受け取ることを示します。|  
|`IMAGE_CEE_CS_CALLCONV_FIELD`|は、フィールドへの呼び出しであることを示します。|  
|`IMAGE_CEE_CS_CALLCONV_LOCAL_SIG`|呼び出しがローカルメソッドに対して行うことを示します。|  
|`IMAGE_CEE_CS_CALLCONV_PROPERTY`|は、プロパティへの呼び出しであることを示します。|  
|`IMAGE_CEE_CS_CALLCONV_UNMGD`|呼び出しがアンマネージであることを示します。|  
|`IMAGE_CEE_CS_CALLCONV_GENERICINST`|ジェネリックメソッドのインスタンス化を示します。|  
|`IMAGE_CEE_CS_CALLCONV_NATIVEVARARG`|可変個のパラメーターを受け取るメソッドへの64ビット PInvoke 呼び出しを示します。|  
|`IMAGE_CEE_CS_CALLCONV_MAX`|4ビットの値が無効であることを示します。|  
|`IMAGE_CEE_CS_CALLCONV_MASK`|呼び出し規則が下位4ビットによって記述されていることを示します。|  
|`IMAGE_CEE_CS_CALLCONV_HASTHIS`|最上位ビットが `this` パラメーターを記述することを示します。|  
|`IMAGE_CEE_CS_CALLCONV_EXPLICITTHIS`|`this` パラメーターがシグネチャに明示的に記述されていることを示します。|  
|`IMAGE_CEE_CS_CALLCONV_GENERIC`|型引数の数が明示的に指定されたジェネリックメソッドシグネチャを示します。 これは、通常のパラメーターカウントの前になります。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
