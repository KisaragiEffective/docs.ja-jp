---
title: ICorDebugStackWalk インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugStackWalk
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugStackWalk
helpviewer_keywords:
- ICorDebugStackWalk interface [.NET Framework debugging]
ms.assetid: 16d695e8-975d-431b-8421-e9e6c3e3f476
topic_type:
- apiref
ms.openlocfilehash: a6283d699263dc9b79e457010f31923f77443129
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791876"
---
# <a name="icordebugstackwalk-interface"></a>ICorDebugStackWalk インターフェイス
スレッドのスタック上のマネージド メソッド (フレーム) を取得するメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetContext メソッド](icordebugstackwalk-getcontext-method.md)|`ICorDebugStackWalk` オブジェクト内の現在のフレームのコンテキストを返します。|  
|[SetContext メソッド](icordebugstackwalk-setcontext-method.md)|`ICorDebugStackWalk` オブジェクトの現在のコンテキストをスレッドの有効なコンテキストに設定します。|  
|[Next メソッド](icordebugstackwalk-next-method.md)|`ICorDebugStackWalk` オブジェクトを次のフレームに移動します。|  
|[GetFrame メソッド](icordebugstackwalk-getframe-method.md)|`ICorDebugStackWalk` オブジェクト内の現在のフレームを取得します。|  
  
## <a name="remarks"></a>コメント  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
