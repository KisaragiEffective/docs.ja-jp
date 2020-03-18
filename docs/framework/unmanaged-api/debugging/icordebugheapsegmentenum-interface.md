---
title: ICorDebugHeapSegmentEnum インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugHealRegionEnum
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugHeapSegmentEnum
helpviewer_keywords:
- ICorDebugHeapSegmentEnum interface [.NET Framework debugging]
ms.assetid: 20fc1b9d-e228-4107-bd76-53934c1724b9
topic_type:
- apiref
ms.openlocfilehash: 98e3351c9c86961d11b0985117259d0ff3b609ae
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794419"
---
# <a name="icordebugheapsegmentenum-interface"></a>ICorDebugHeapSegmentEnum インターフェイス
マネージド ヒープのメモリ領域の列挙子を提供します。 このインターフェイスは、ICorDebugEnum インターフェイスのサブクラスです。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[Next メソッド](icordebugheapsegmentenum-next-method.md)|マネージヒープの領域に関する情報を格納している、指定した数の[COR_HEAPOBJECT](cor-heapobject-structure.md)インスタンスを取得します。|  
  
## <a name="remarks"></a>コメント  
 `ICorDebugHeapSegmentEnum` インターフェイスは、ICorDebugEnum インターフェイスを実装します。  
  
 `ICorDebugHeapSegmentEnum` インスタンスには、 [ICorDebugProcess5:: EnumerateHeapRegions](icordebugprocess5-enumerateheapregions-method.md)メソッドを呼び出すことによって、 [COR_HEAPOBJECT](cor-heapobject-structure.md)インスタンスが設定されます。 コレクション内の[COR_HEAPOBJECT](cor-heapobject-structure.md)オブジェクトは、 [ICorDebugHeapSegmentEnum:: Next](icordebugheapsegmentenum-next-method.md)メソッドを呼び出すことによって列挙できます。  
  
 `ICorDebugHeapSegmentEnum` collection オブジェクトは、マネージオブジェクトが含まれている可能性があるすべてのメモリ領域を列挙しますが、それらの領域にマネージオブジェクトが実際に存在することは保証しません。 これには、空または予約済みのメモリ領域に関する情報が含まれる場合があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
