---
title: CorErrorIfEmitOutOfOrder 列挙型
ms.date: 03/30/2017
api_name:
- CorErrorIfEmitOutOfOrder
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorErrorIfEmitOutOfOrder
helpviewer_keywords:
- CorErrorIfEmitOutOfOrder enumeration [.NET Framework metadata]
ms.assetid: 6d758aad-29a7-44fe-9481-bbff5b799a32
topic_type:
- apiref
ms.openlocfilehash: 57460ba30a8ce974b5ca89f76796c4dcf49ffecf
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74443588"
---
# <a name="corerrorifemitoutoforder-enumeration"></a>CorErrorIfEmitOutOfOrder 列挙型
メタデータの生成順序が不適切である場合にエラー メッセージが生成される条件を示すフラグ値が格納されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorErrorIfEmitOutOfOrder {  
  
    MDErrorOutOfOrderDefault    = 0x00000000,  
    MDErrorOutOfOrderNone       = 0x00000000,  
    MDErrorOutOfOrderAll        = 0xffffffff,  
    MDMethodOutOfOrder          = 0x00000001,  
    MDFieldOutOfOrder           = 0x00000002,  
    MDParamOutOfOrder           = 0x00000004,  
    MDPropertyOutOfOrder        = 0x00000008,  
    MDEventOutOfOrder           = 0x00000010  
  
} CorErrorIfEmitOutOfOrder;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`MDErrorOutOfOrderDefault`|エラーメッセージを生成しない既定の動作を示します。|  
|`MDErrorOutOfOrderNone`|コンパイラがエラーメッセージを生成しないことを示します。|  
|`MDErrorOutOfOrderAll`|フィールド、プロパティ、イベント、メソッド、またはパラメーターが順序どおりに生成されない場合に、コンパイラがエラーメッセージを生成することを示します。|  
|`MDMethodOutOfOrder`|メソッドが順序どおりに出力されない場合に、コンパイラがエラーメッセージを生成する必要があることを示します。|  
|`MDFieldOutOfOrder`|フィールドが順序どおりに出力されない場合に、コンパイラがエラーメッセージを生成する必要があることを示します。|  
|`MDParamOutOfOrder`|パラメーターが順序どおりに出力されない場合に、コンパイラがエラーメッセージを生成する必要があることを示します。|  
|`MDPropertyOutOfOrder`|プロパティが順序どおりに出力されない場合に、コンパイラがエラーメッセージを生成する必要があることを示します。|  
|`MDEventOutOfOrder`|イベントが順序どおりに生成されない場合に、コンパイラがエラーメッセージを生成する必要があることを示します。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorHdr. h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [メタデータ列挙型](../../../../docs/framework/unmanaged-api/metadata/metadata-enumerations.md)
