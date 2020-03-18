---
title: ICorDebugValue3::GetSize64 メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugValue3::GetSize64
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugValue3::GetSize64
helpviewer_keywords:
- GetSize64 method, ICorDebugValue3 interface [.NET Framework debugging]
- ICorDebugValue3::GetSize64 method [.NET Framework debugging]
ms.assetid: fee56a29-3154-4192-958d-71da2ced3740
topic_type:
- apiref
ms.openlocfilehash: 7ae06d825565faff70b0c8be2ccbee5228737e41
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791100"
---
# <a name="icordebugvalue3getsize64-method"></a>ICorDebugValue3::GetSize64 メソッド
この[ICorDebugValue3](icordebugvalue3-interface.md)オブジェクトのサイズ (バイト単位) を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetSize64(  
    [out] ULONG64 *pSize  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 pSize  
 入出力このオブジェクトのサイズ (バイト単位) へのポインター。  
  
## <a name="remarks"></a>コメント  
 この値の型が参照型の場合、このメソッドはオブジェクトのサイズではなく、ポインターのサイズを返します。  
  
 `ICorDebugValue3::GetSize` メソッドは、その出力パラメーターの型の[ICorDebugValue:: GetSize](icordebugvalue-getsize-method.md)メソッドとは異なります。 [Icordebugvalue::getsize](icordebugvalue-getsize-method.md)、出力パラメーターが、 `ULONG32`;`ICorDebugValue3::GetSize`は、`ULONG64`です。 これにより、 [ICorDebugValue3](icordebugvalue3-interface.md)インターフェイスは、2gb を超える配列のサイズを報告できます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebugValue3 インターフェイス](icordebugvalue3-interface.md)
- [デバッグ インターフェイス](debugging-interfaces.md)
