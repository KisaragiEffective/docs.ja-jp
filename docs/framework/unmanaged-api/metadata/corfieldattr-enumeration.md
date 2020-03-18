---
title: CorFieldAttr 列挙型
ms.date: 03/30/2017
api_name:
- CorFieldAttr
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorFieldAttr
helpviewer_keywords:
- CorFieldAttr enumeration [.NET Framework metadata]
ms.assetid: 6ae2c4be-212c-4e74-9288-40a11dc26522
topic_type:
- apiref
ms.openlocfilehash: d28a0c8b7ee85f023026dde6f3cc8f3a8406aa64
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74450304"
---
# <a name="corfieldattr-enumeration"></a>CorFieldAttr 列挙型
フィールドについてのメタデータを記述する値が格納されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorFieldAttr {  
  
    fdFieldAccessMask           =   0x0007,  
    fdPrivateScope              =   0x0000,  
    fdPrivate                   =   0x0001,  
    fdFamANDAssem               =   0x0002,  
    fdAssembly                  =   0x0003,  
    fdFamily                    =   0x0004,  
    fdFamORAssem                =   0x0005,  
    fdPublic                    =   0x0006,  
  
    fdStatic                    =   0x0010,  
    fdInitOnly                  =   0x0020,  
    fdLiteral                   =   0x0040,  
    fdNotSerialized             =   0x0080,  
  
    fdSpecialName               =   0x0200,  
  
    fdPinvokeImpl               =   0x2000,  
  
    fdReservedMask              =   0x9500,  
    fdRTSpecialName             =   0x0400,  
    fdHasFieldMarshal           =   0x1000,  
    fdHasDefault                =   0x8000,  
    fdHasFieldRVA               =   0x0100  
  
} CorFieldAttr;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`fdFieldAccessMask`|アクセシビリティ情報を指定します。|  
|`fdPrivateScope`|フィールドを参照できないことを指定します。|  
|`fdPrivate`|フィールドがその親の型によってのみアクセス可能であることを指定します。|  
|`fdFamANDAssem`|アセンブリ内の派生クラスによってフィールドにアクセスできることを指定します。|  
|`fdAssembly`|アセンブリ内のすべての型からフィールドにアクセスできることを指定します。|  
|`fdFamily`|フィールドがその型および派生クラスによってのみアクセス可能であることを指定します。|  
|`fdFamORAssem`|派生クラスおよびそのアセンブリ内のすべての型によってフィールドにアクセスできることを指定します。|  
|`fdPublic`|このスコープの可視性を持つすべての型からフィールドにアクセスできることを指定します。|  
|`fdStatic`|フィールドがインスタンスメンバーではなく、その型のメンバーであることを指定します。|  
|`fdInitOnly`|初期化後にフィールドを変更できないことを指定します。|  
|`fdLiteral`|フィールド値がコンパイル時の定数であることを指定します。|  
|`fdNotSerialized`|型がリモート処理されるときに、フィールドをシリアル化しないことを指定します。|  
|`fdSpecialName`|フィールドが特別であること、およびその名前で方法が説明されていることを指定します。|  
|`fdPinvokeImpl`|フィールドの実装が PInvoke 経由で転送されることを指定します。|  
|`fdReservedMask`|共通言語ランタイムによる内部使用のために予約されています。|  
|`fdRTSpecialName`|共通言語ランタイムメタデータの内部 Api が名前のエンコーディングを確認する必要があることを指定します。|  
|`fdHasFieldMarshal`|フィールドにマーシャリング情報が含まれることを指定します。|  
|`fdHasDefault`|フィールドに既定値があることを指定します。|  
|`fdHasFieldRVA`|フィールドが相対仮想アドレスを持つことを指定します。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
