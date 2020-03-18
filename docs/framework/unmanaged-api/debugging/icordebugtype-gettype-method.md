---
title: ICorDebugType::GetType メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugType.GetType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugType::GetType
helpviewer_keywords:
- ICorDebugType::GetType method [.NET Framework debugging]
- GetType method, ICorDebugType interface [.NET Framework debugging]
ms.assetid: d6e64534-4d47-4ad0-a340-7590e07e2b4a
topic_type:
- apiref
ms.openlocfilehash: 25ffbf73fbefbb3c584450283c3080dfc11ee598
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791240"
---
# <a name="icordebugtypegettype-method"></a>ICorDebugType::GetType メソッド
このテキスト型で表される共通言語ランタイム (CLR) <xref:System.Type> のネイティブ型を記述する CorElementType 値を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetType (  
    [out] CorElementType   *ty  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `ty`  
 入出力この `ICorDebugType` が表す CLR <xref:System.Type> を示す `CorElementType` 列挙値へのポインター。  
  
## <a name="remarks"></a>コメント  
 `ty` の値が ELEMENT_TYPE_CLASS または ELEMENT_TYPE_VALUETYPE の場合、ジェネリック型のインスタンス型を取得するために、の型[:: GetClass](icordebugtype-getclass-method.md)メソッドを呼び出すことができます。それ以外の場合は、`ICorDebugType::GetClass`を呼び出さないでください。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
