---
title: ICorDebugController::EnumerateThreads メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugController.EnumerateThreads
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController::EnumerateThreads
helpviewer_keywords:
- ICorDebugController::EnumerateThreads method [.NET Framework debugging]
- EnumerateThreads method [.NET Framework debugging]
ms.assetid: 73f536f6-4668-4a4a-b3e4-ac7df862d5be
topic_type:
- apiref
ms.openlocfilehash: 815dc63a2dedecc613506b0f98702f58d6e7bd04
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76783783"
---
# <a name="icordebugcontrollerenumeratethreads-method"></a>ICorDebugController::EnumerateThreads メソッド
プロセス内のアクティブなマネージスレッドの列挙子を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EnumerateThreads (  
    [out] ICorDebugThreadEnum **ppThreads  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `ppThreads`  
 入出力プロセス内でアクティブになっているすべてのマネージスレッドの列挙子を表す "いい Threadenum" オブジェクトのアドレスへのポインター。  
  
## <a name="remarks"></a>コメント  
 スレッドがアクティブであると見なされるのは、"[CreateThread](icordebugmanagedcallback-createthread-method.md)" コールバックがディスパッチされてから、" [Managedcallback:: exitthread](icordebugmanagedcallback-exitthread-method.md) " コールバックがディスパッチされる前です。 マネージスレッドは、必ずしもスタック上にマネージフレームを持つとは限りません。 スレッドは、によっては、"、 [" という](icordebugmanagedcallback-createprocess-method.md)ように列挙できます。 列挙体は、自然に空になります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目
