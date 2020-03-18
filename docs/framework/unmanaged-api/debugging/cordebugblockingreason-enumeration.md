---
title: CorDebugBlockingReason 列挙体
ms.date: 03/30/2017
api_name:
- CorDebugBlockingReason
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugBlockingReason
helpviewer_keywords:
- CorDebugBlockingReason enumeration [.NET Framework debugging]
ms.assetid: a6ac2531-ddfe-46fd-88fe-8b1eabe0b255
topic_type:
- apiref
ms.openlocfilehash: bc488e55bf64468eb62e2dc6eaedca62ebde3310
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73098986"
---
# <a name="cordebugblockingreason-enumeration"></a>CorDebugBlockingReason 列挙体
指定されたオブジェクト上でスレッドがブロックされる理由を指定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
Typedef enum CorDebugBlockingReason  
{  
   BLOCKING_NONE = 0  
   BLOCKING_MONITOR_CRITICAL_SECTION = 1  
   BLOCKING_MONITOR_EVENT = 2  
}  CorDebugBlockingReason;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`BLOCKING_NONE`|内部使用のみ。|  
|`BLOCKING_MONITOR_CRITICAL_SECTION`|スレッドが、オブジェクトのモニターロックに関連付けられているクリティカルセクションを取得しようとしています。 通常、これは、<xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType> または <xref:System.Threading.Monitor.TryEnter%2A?displayProperty=nameWithType> のいずれかのメソッドを呼び出すと発生します。|  
|`BLOCKING_MONITOR_EVENT`|スレッドが、オブジェクトのモニターロックに関連付けられているイベントを待機しています。 通常、これは、<xref:System.Threading.Monitor?displayProperty=nameWithType>`Wait` メソッドの1つを呼び出すと発生します。|  
  
## <a name="remarks"></a>Remarks  
 `BLOCKING_MONITOR_CRITICAL_SECTION` または `BLOCKING_MONITOR_EVENT` のメンバーが[CorDebugBlockingObject](cordebugblockingobject-structure.md)構造体で使用されている場合、構造体の `pBlockingObject` メンバーは、入力されるオブジェクトを表す "ICorDebugValue" インターフェイスを指します。 また、 [ICorDebugHeapValue3](icordebugheapvalue3-interface.md)インターフェイスを実装することも保証されます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [列挙型のデバッグ](debugging-enumerations.md)
- [デバッグ](index.md)
