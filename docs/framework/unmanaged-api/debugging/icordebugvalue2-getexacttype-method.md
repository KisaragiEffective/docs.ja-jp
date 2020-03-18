---
title: ICorDebugValue2::GetExactType メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugValue2.GetExactType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugValue2::GetExactType
helpviewer_keywords:
- ICorDebugValue2::GetExactType method [.NET Framework debugging]
- GetExactType method [.NET Framework debugging]
ms.assetid: 8e9aae1b-d1b7-4b6e-b577-6faf36dcec85
topic_type:
- apiref
ms.openlocfilehash: 6c5e5d1c9f734e097fc9e871d7a0cffdc9bb9138
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791123"
---
# <a name="icordebugvalue2getexacttype-method"></a>ICorDebugValue2::GetExactType メソッド
この値の <xref:System.Type> を表す "の型のオブジェクトへのインターフェイスポインターを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetExactType (  
    [out] ICorDebugType   **ppType  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `ppType`  
 入出力この "ICorDebugValue2" オブジェクトによって表される値の <xref:System.Type> を表す `ICorDebugType` オブジェクトのアドレスへのポインター。  
  
## <a name="remarks"></a>コメント  
 ジェネリック対応の `GetExactType` メソッドは、、の各メソッドと、値の型に関する情報を返す、それぞれのメソッドと[ICorDebugValue:: GetType](icordebugvalue-gettype-method.md) [メソッドの両方](icordebugobjectvalue-getclass-method.md)を置き換えます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目
