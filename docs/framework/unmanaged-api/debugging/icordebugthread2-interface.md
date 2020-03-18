---
title: ICorDebugThread2 インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugThread2
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread2
helpviewer_keywords:
- ICorDebugThread2 interface [.NET Framework debugging]
ms.assetid: 678f89f9-cce7-46d1-af87-5e989abaa93c
topic_type:
- apiref
ms.openlocfilehash: fdaad46b739721ff95b712d4b6461a793ae0a480
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791430"
---
# <a name="icordebugthread2-interface"></a>ICorDebugThread2 インターフェイス
は、"、" スレッドインターフェイスの論理的な拡張として機能します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetActiveFunctions メソッド](icordebugthread2-getactivefunctions-method.md)|スレッドのフレーム内のアクティブな関数に関するデータを格納している COR_ACTIVE_FUNCTION インスタンスの配列を取得します。|  
|[GetConnectionID メソッド](icordebugthread2-getconnectionid-method.md)|この `ICorDebugThread2`の接続識別子を取得します。|  
|[GetTaskID メソッド](icordebugthread2-gettaskid-method.md)|この `ICorDebugThread2`のタスク識別子を取得します。|  
|[GetVolatileOSThreadID メソッド](icordebugthread2-getvolatileosthreadid-method.md)|この `ICorDebugThread2`のオペレーティングシステムスレッド識別子を取得します。|  
|[InterceptCurrentException メソッド](icordebugthread2-interceptcurrentexception-method.md)|デバッガーがスレッドの現在の例外をインターセプトできるようにします。|  
  
## <a name="remarks"></a>コメント  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
