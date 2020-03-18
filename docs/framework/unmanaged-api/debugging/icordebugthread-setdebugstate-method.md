---
title: ICorDebugThread::SetDebugState メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugThread.SetDebugState
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread::SetDebugState
helpviewer_keywords:
- ICorDebugThread::SetDebugState method [.NET Framework debugging]
- SetDebugState method [.NET Framework debugging]
ms.assetid: 6382bdf6-d488-4952-b653-cb09b6e1c6c2
topic_type:
- apiref
ms.openlocfilehash: b7678c64a085a0d4951d398595b9be89af8eeb6b
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791438"
---
# <a name="icordebugthreadsetdebugstate-method"></a>ICorDebugThread::SetDebugState メソッド
このスレッドのデバッグ状態を記述するフラグを設定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetDebugState (  
    [in] CorDebugThreadState state  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `state`  
 からこのスレッドのデバッグ状態を指定する CorDebugThreadState 列挙値のビットごとの組み合わせ。  
  
## <a name="remarks"></a>コメント  
 スレッドの現在のデバッグ状態を `SetDebugState` 設定します。 ("現在のデバッグ状態" は、プロセスが継続されていて、実際の現在の状態ではない場合は、デバッグ状態を表します)。この場合の通常の値は THREAD_RUN です。 デバッガーだけが、スレッドのデバッグ状態に影響を与える可能性があります。 デバッグ状態は最後まで継続して実行されるので、スレッド THREAD_SUSPENDed を複数回継続したままにする場合は、1回設定するだけで、それ以降は心配する必要はありません。 スレッドを中断してプロセスを再開するとデッドロックが発生する可能性がありますが、通常はそうではありません。 これは、スレッドとプロセスの組み込みの品質であり、設計によるものです。 デバッガーは、非同期的にスレッドを中断して再開し、デッドロックを解除することができます。 スレッドのユーザー状態に USER_UNSAFE_POINT が含まれている場合、スレッドはガベージコレクション (GC) をブロックする可能性があります。 これは、中断されたスレッドがデッドロックの原因となる可能性が非常に高いことを意味します。 これは、既にキューに登録されているデバッグイベントには影響しません。 したがって、デバッガーは、スレッドを中断または再開する前に、イベントキュー全体 ( [HasQueuedCallbacks::](icordebugcontroller-hasqueuedcallbacks-method.md)を呼び出すことによって) をドレインする必要があります。 それ以外の場合は、既に中断されていると思われるスレッドでイベントを取得することがあります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
