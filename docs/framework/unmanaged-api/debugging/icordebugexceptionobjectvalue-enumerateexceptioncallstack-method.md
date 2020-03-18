---
title: ICorDebugExceptionObjectValue::EnumerateExceptionCallStack メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugExceptionObjectValue.EnumerateExceptionCallStack
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugExceptionObjectValue::EnumerateExceptionCallStack
helpviewer_keywords:
- EnumerateExceptionCallStack method, ICorDebugExceptionObjectValue interface [.NET Framework debugging]
- ICorDebugExceptionObjectValue::EnumerateExceptionCallStack method [.NET Framework debugging]
ms.assetid: 00c64533-15dd-47f4-bb97-fe80a1ebadef
topic_type:
- apiref
ms.openlocfilehash: 57eb284bfe39ce92b2d6c03a2aeb4ae84d6aba91
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76788675"
---
# <a name="icordebugexceptionobjectvalueenumerateexceptioncallstack-method"></a>ICorDebugExceptionObjectValue::EnumerateExceptionCallStack メソッド
例外オブジェクトに埋め込まれている呼び出し履歴に対する列挙子を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EnumerateExceptionCallStack(  
    [out] ICorDebugExceptionObjectCallStackEnum **ppCallStackEnum  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 ppCallStackEnum  
 入出力マネージ例外オブジェクトの[スタックトレース](icordebugexceptionobjectcallstackenum-interface.md)列挙子である、というコードのアドレスへのポインターを示します。
  
  
## <a name="remarks"></a>コメント  
 呼び出し履歴情報が使用できない場合、メソッドは `S_OK`を返します。また、は、値が0の有効な列挙[子です。](icordebugexceptionobjectcallstackenum-interface.md) メソッドがスタックトレース情報を取得できない場合、戻り値は `E_FAIL`、列挙子は返されません。  
  
 は、例外オブジェクトの `_stackTrace` フィールドからスタックトレースデータをデコードするために、のオブジェクトを[使用します](icordebugexceptionobjectcallstackenum-interface.md)。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebugExceptionObjectValue インターフェイス](icordebugexceptionobjectvalue-interface.md)
- [デバッグ インターフェイス](debugging-interfaces.md)
