---
title: ICorDebug::Terminate メソッド
ms.date: 03/30/2017
api_name:
- ICorDebug.Terminate
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebug::Terminate
helpviewer_keywords:
- Terminate method, ICorDebug interface [.NET Framework debugging]
- ICorDebug::Terminate method [.NET Framework debugging]
ms.assetid: fffe5616-0896-4426-ab5e-21869b514883
topic_type:
- apiref
ms.openlocfilehash: 2d3f8360a1fb1164fd4e26f85246251409bee376
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76788961"
---
# <a name="icordebugterminate-method"></a>ICorDebug::Terminate メソッド
`ICorDebug` オブジェクトを終了します。  
  
> [!NOTE]
> デバッグ中のすべてのプロセスに対して、 [ExitProcess callback::](icordebugmanagedcallback-exitprocess-method.md)コールバックを受信するまでは、`Terminate` を呼び出すことはできません。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT Terminate ();  
```  
  
## <a name="remarks"></a>コメント  
 `ICorDebug` オブジェクトが不要になった場合は、`Terminate` を呼び出す必要があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebug インターフェイス](icordebug-interface.md)
