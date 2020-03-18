---
title: ICorDebugType::GetStaticFieldValue メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugType.GetStaticFieldValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugType::GetStaticFieldValue
helpviewer_keywords:
- GetStaticFieldValue method, ICorDebugType interface [.NET Framework debugging]
- ICorDebugType::GetStaticFieldValue method [.NET Framework debugging]
ms.assetid: 62eb5d55-53ee-4fb3-8d47-7b6c96808f9e
topic_type:
- apiref
ms.openlocfilehash: 37bf5abf66b613d8432af84c7d73aff60e9127cb
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791268"
---
# <a name="icordebugtypegetstaticfieldvalue-method"></a>ICorDebugType::GetStaticFieldValue メソッド
指定したスタックフレーム内の指定したフィールドトークンによって参照される静的フィールドの値を格納する、ICorDebugValue オブジェクトへのインターフェイスポインターを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetStaticFieldValue (  
    [in]  mdFieldDef        fieldDef,  
    [in]  ICorDebugFrame    *pFrame,  
    [out] ICorDebugValue    **ppValue  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `fieldDef`  
 から静的フィールドを指定する `mdFieldDef` トークンです。  
  
 `pFrame`  
 からスタックフレームを表す、テキストフレームへのポインター。  
  
 `ppValue`  
 入出力静的フィールドの値を格納している `ICorDebugValue` のアドレスへのポインター。  
  
## <a name="remarks"></a>コメント  
 `GetStaticFieldValue` メソッドは、型が ELEMENT_TYPE_CLASS または ELEMENT_TYPE_VALUETYPE の場合にのみ使用できます。この場合、は、テキスト[:: GetType](icordebugtype-gettype-method.md)メソッドによって示されます。  
  
 非ジェネリック型の場合、`GetStaticFieldValue` によって実行される操作は、によって返される、は、によって返された、テキスト:: [GetStaticFieldValue](icordebugclass-getstaticfieldvalue-method.md)と同じにすることができます、には、[テキスト:: getclass](icordebugtype-getclass-method.md)です。  
  
 ジェネリック型の場合、静的フィールド値は特定のインスタンス化に対して相対的になります。 また、静的フィールドがスレッド、コンテキスト、またはアプリケーションドメインに対して相対的である可能性がある場合は、デバッガーが適切な値を決定するためにスタックフレームが役立ちます。  
  
## <a name="remarks"></a>コメント  
 `GetStaticFieldValue` は、`ICorDebugType::GetType` の呼び出しによって ELEMENT_TYPE_CLASS または ELEMENT_TYPE_VALUETYPE の値が返された場合にのみ使用できます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
