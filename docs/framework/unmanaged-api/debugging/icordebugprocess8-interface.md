---
title: ICorDebugProcess8 インターフェイス
ms.date: 03/30/2017
ms.assetid: 7ab1a70f-ec11-46ff-8869-cd8ca679cc51
ms.openlocfilehash: 813afc047f9fda060f1cf1af780336ff8b7c18be
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76792149"
---
# <a name="icordebugprocess8-interface"></a>ICorDebugProcess8 インターフェイス
[.NET Framework 4.6 以降のバージョンでサポートされています]  
  
 ICorDebugManagedCallback2 Process インターフェイスを論理的に拡張して、[特定の種類の](icordebugmanagedcallback2-interface.md)例外コールバックを有効または無効にします。
  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[EnableExceptionCallbacksOutsideOfMyCode メソッド](icordebugprocess8-enableexceptioncallbacksoutsideofmycode-method.md)|特定の種類の[ICorDebugManagedCallback2](icordebugmanagedcallback2-interface.md)例外コールバックを有効または無効にします。|  
  
## <a name="remarks"></a>コメント  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v46plus](../../../../includes/net-current-v46plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
