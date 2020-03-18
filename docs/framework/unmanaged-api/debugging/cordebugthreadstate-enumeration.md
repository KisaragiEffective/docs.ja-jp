---
title: CorDebugThreadState 列挙型
ms.date: 03/30/2017
api_name:
- CorDebugThreadState
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugThreadState
helpviewer_keywords:
- CorDebugThreadState enumeration [.NET Framework debugging]
ms.assetid: a3ccdf18-4ec6-494d-9024-48e5c8c724f5
topic_type:
- apiref
ms.openlocfilehash: 69a8aabd1d79bb9bb4248259c99124ce50677600
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76789239"
---
# <a name="cordebugthreadstate-enumeration"></a>CorDebugThreadState 列挙型
デバッグのスレッドの状態を指定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorDebugThreadState {  
    THREAD_RUN,  
    THREAD_SUSPEND  
} CorDebugThreadState;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`THREAD_RUN`|デバッグイベントが発生しない限り、スレッドは自由に実行できます。|  
|`THREAD_SUSPEND`|スレッドを実行できません。|  
  
## <a name="remarks"></a>コメント  
 デバッガーは、`CorDebugThreadState` 列挙体を使用して、スレッドの実行を制御します。 スレッドの状態を設定するには、次のように指定する必要があります: [: SetDebugState](icordebugthread-setdebugstate-method.md)または、モジュール[:: SetAllThreadsDebugState](icordebugcontroller-setallthreadsdebugstate-method.md)メソッド。  
  
 [ホスティング API](../../../../docs/framework/unmanaged-api/hosting/index.md)に提供されるコールバックによってメッセージポンプが可能になるため、中断された状態は必要ありません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [列挙型のデバッグ](debugging-enumerations.md)
