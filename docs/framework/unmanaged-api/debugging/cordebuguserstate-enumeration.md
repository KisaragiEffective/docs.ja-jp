---
title: CorDebugUserState 列挙型
ms.date: 03/30/2017
api_name:
- CorDebugUserState
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugUserState
helpviewer_keywords:
- CorDebugUserState enumeration [.NET Framework debugging]
ms.assetid: 5f6c2bcd-8102-4e3b-abc5-86ab0bd62def
topic_type:
- apiref
ms.openlocfilehash: c142b9656af2031b10de239645da76835c435655
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76789224"
---
# <a name="cordebuguserstate-enumeration"></a>CorDebugUserState 列挙型
スレッドのユーザーの状態を示します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorDebugUserState {  
    USER_STOP_REQUESTED     =  0x01,  
    USER_SUSPEND_REQUESTED  =  0x02,  
    USER_BACKGROUND         =  0x04,  
    USER_UNSTARTED          =  0x08,  
    USER_STOPPED            =  0x10,  
    USER_WAIT_SLEEP_JOIN    =  0x20,  
    USER_SUSPENDED          =  0x40,  
    USER_UNSAFE_POINT       =  0x80,  
    USER_THREADPOOL         = 0x100  
} CorDebugUserState;  
```  
  
## <a name="members"></a>メンバー  
  
|Value|説明|  
|-----------|-----------------|  
|`USER_STOP_REQUESTED`|スレッドの終了が要求されました。|  
|`USER_SUSPEND_REQUESTED`|スレッドの中断が要求されました。|  
|`USER_BACKGROUND`|スレッドがバックグラウンドで実行されています。|  
|`USER_UNSTARTED`|スレッドが実行を開始していません。|  
|`USER_STOPPED`|スレッドは終了されました。|  
|`USER_WAIT_SLEEP_JOIN`|スレッドは、別のスレッドがタスクを完了するのを待機しています。|  
|`USER_SUSPENDED`|スレッドが中断されました。|  
|`USER_UNSAFE_POINT`|スレッドが安全でないポイントにあります。 つまり、スレッドは、ガベージコレクションをブロックする可能性がある実行中のポイントにあります。<br /><br /> デバッグイベントは安全でないポイントからディスパッチできますが、アンセーフポイントでスレッドを中断すると、スレッドが再開されるまでデッドロックが発生する可能性が高くなります。 セーフポイントと unsafe ポイントは、just-in-time (JIT) とガベージコレクションの実装によって決定されます。|  
|`USER_THREADPOOL`|スレッドはスレッドプールからのものです。|  
  
## <a name="remarks"></a>コメント  
 スレッドのユーザー状態は、デバッガーがそのスレッドを調べたときのスレッドの状態です。 スレッドには、ユーザー状態を組み合わせて含めることができます。  
  
 スレッドのユーザー状態を取得するには、 [「いい thread:: GetUserState](icordebugthread-getuserstate-method.md)メソッド」を使用します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [列挙型のデバッグ](debugging-enumerations.md)
