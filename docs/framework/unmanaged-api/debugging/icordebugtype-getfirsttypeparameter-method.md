---
description: '詳細については、次を参照してください:、テキスト:: テキストメソッド'
title: ICorDebugType::GetFirstTypeParameter メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugType.GetFirstTypeParameter
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugType::GetFirstTypeParameter
helpviewer_keywords:
- ICorDebugType::GetFirstTypeParameter method [.NET Framework debugging]
- GetFirstTypeParameter method [.NET Framework debugging]
ms.assetid: 35bb594f-af6a-4349-83fe-e98702674e03
topic_type:
- apiref
ms.openlocfilehash: 4c37217f34f80c916d618d88e4917eab794a1d90
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99658283"
---
# <a name="icordebugtypegetfirsttypeparameter-method"></a>ICorDebugType::GetFirstTypeParameter メソッド

<xref:System.Type>このによって表される型の最初のパラメーターを表す、の型へのインターフェイスポインターを取得し `ICorDebugType` ます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetFirstTypeParameter (  
    [out] ICorDebugType   **value  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `value`  
 入出力最初のパラメーターを表すオブジェクトのアドレスへのポインター `ICorDebugType` 。  
  
## <a name="remarks"></a>解説  

 `GetFirstTypeParameter` 型に関する追加情報には、最大で1つの型パラメーターが含まれる場合に、を呼び出すことができます。 具体的には、型が ELEMENT_TYPE_ARRAY、ELEMENT_TYPE_SZARRAY、ELEMENT_TYPE_BYREF、または ELEMENT_TYPE_PTR である場合に使用できます。これは、のように、の型 [:: GetType](icordebugtype-gettype-method.md) メソッドによって示されます。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
