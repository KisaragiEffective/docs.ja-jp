---
title: ICorDebugObjectValue::GetFieldValue メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugObjectValue.GetFieldValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugObjectValue::GetFieldValue
helpviewer_keywords:
- ICorDebugObjectValue::GetFieldValue method [.NET Framework debugging]
- GetFieldValue method [.NET Framework debugging]
ms.assetid: c96770b0-3e09-47bb-bd29-20353b043459
topic_type:
- apiref
ms.openlocfilehash: 002c6cccb3ddf29b831ba5e14baa5e51f1b82433
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73095889"
---
# <a name="icordebugobjectvaluegetfieldvalue-method"></a>ICorDebugObjectValue::GetFieldValue メソッド
このオブジェクト値について、指定したクラスの指定したフィールドの値を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetFieldValue (  
    [in]  ICorDebugClass     *pClass,  
    [in]  mdFieldDef         fieldDef,  
    [out] ICorDebugValue     **ppValue  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pClass`  
 からフィールド値を取得する対象のクラスを表す "表示クラス" オブジェクトへのポインター。  
  
 `fieldDef`  
 からフィールドを記述するメタデータを参照する `mdFieldDef` トークン。  
  
 `ppValue`  
 入出力指定したフィールドの値を表す "ICorDebugValue" オブジェクトへのポインター。  
  
## <a name="remarks"></a>Remarks  
 `pClass` パラメーターで指定されたクラスは、オブジェクト値のクラスの階層内に存在する必要があり、フィールドはそのクラスのフィールドである必要があります。  
  
 `GetFieldValue` メソッドは、ジェネリックオブジェクトおよびジェネリッククラスでも成功します。 たとえば、MyDictionary\<V > が Dictionary\<string, V > から継承され、オブジェクト値が MyDictionary\<int32 > 型である場合、Dictionary `ICorDebugClass` K の\<オブジェクトを渡すと、V > は次のフィールドを正常に取得します。Dictionary\<string、int32 >。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目
