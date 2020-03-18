---
title: ICorDebugComObjectValue のインターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugComObjectValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugComObjectValue
helpviewer_keywords:
- ICorDebugComObjectValue interface [.NET Framework debugging]
ms.assetid: 505a7f6c-d92b-42b4-b539-433f5102ea9b
topic_type:
- apiref
ms.openlocfilehash: ed5b39ed4b2a14c071bf23fb04efbad6834e8a9d
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76783973"
---
# <a name="icordebugcomobjectvalue-interface"></a>ICorDebugComObjectValue のインターフェイス
ランタイム呼び出し可能ラッパー (RCW) に関連付けられている情報を取得するメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetCachedInterfacePointers メソッド](icordebugcomobjectvalue-getcachedinterfacepointers-method.md)|現在の RCW でキャッシュされている生のインターフェイスポインターを取得します。|  
|[GetCachedInterfaceTypes メソッド](icordebugcomobjectvalue-getcachedinterfacetypes-method.md)|現在のオブジェクトで使用または使用されているインターフェイス型の列挙子を提供します。|  
  
## <a name="remarks"></a>コメント  
 "ICorDebugValue" インターフェイスのインスタンスが RCW を表すかどうかを確認するには、デバッガーが `IID_ICorDebugComObjectValue`で "ICorDebugValue" に対して `QueryInterface` を呼び出します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
