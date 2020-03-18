---
title: ICorDebugEval2::CreateValueForType メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugEval2.CreateValueForType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugEval2::CreateValueForType
helpviewer_keywords:
- CreateValueForType method [.NET Framework debugging]
- ICorDebugEval2::CreateValueForType method [.NET Framework debugging]
ms.assetid: ea38ae20-7e0a-427a-be77-d78fae719d82
topic_type:
- apiref
ms.openlocfilehash: 8632799b68ae8f92835d1774472bc1432d886f3b
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76793485"
---
# <a name="icordebugeval2createvaluefortype-method"></a>ICorDebugEval2::CreateValueForType メソッド
初期値が0または null の、指定した型の新しい ICorDebugValue へのポインターを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT CreateValueForType (  
    [in] ICorDebugType         *pType,  
    [out] ICorDebugValue       **ppValue  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pType`  
 から型を表す、の型のオブジェクトへのポインター。  
  
 `ppValue`  
 入出力値を表す `ICorDebugValue` オブジェクトのアドレスへのポインター。  
  
## <a name="remarks"></a>コメント  
 `List<int>`などの構築された型を含む任意のオブジェクトの種類を指定できるようにすることで、`CreateValueForType` 一般化の[eval:: CreateValue](icordebugeval-createvalue-method.md)を使用します。 このメソッドの唯一の目的は、関数の評価に渡すことができる値を生成することです。  
  
 型はクラスまたは値型である必要があります。 このメソッドを使用して配列値や文字列値を作成することはできません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
