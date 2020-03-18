---
title: ICorDebugEval::CreateValue メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugEval.CreateValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugEval::CreateValue
helpviewer_keywords:
- ICorDebugEval::CreateValue method [.NET Framework debugging]
- CreateValue method [.NET Framework debugging]
ms.assetid: 9a1c0b47-6f10-4fcb-844a-4ab2d7990140
topic_type:
- apiref
ms.openlocfilehash: bd6f1b2153404ba4567ef8348ff128b5d475c6fe
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76793488"
---
# <a name="icordebugevalcreatevalue-method"></a>ICorDebugEval::CreateValue メソッド
初期値を0または null にして、指定した型の値を作成します。  
  
 このメソッドは .NET Framework バージョン2.0 では廃止されています。 代わりに[ICorDebugEval2:: CreateValueForType](icordebugeval2-createvaluefortype-method.md)を使用してください。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT CreateValue (  
    [in] CorElementType     elementType,  
    [in] ICorDebugClass     *pElementClass,  
    [out] ICorDebugValue    **ppValue  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `elementType`  
 から値の型を指定する[Corelementtype](../../../../docs/framework/unmanaged-api/metadata/corelementtype-enumeration.md)列挙体の値。  
  
 `pElementClass`  
 から型がプリミティブ型ではない場合に、値のクラスを指定[する、のオブジェクトへ](icordebugclass-interface.md)のポインター。  
  
 `ppValue`  
 入出力値を表す "ICorDebugValue" オブジェクトのアドレスへのポインター。  
  
## <a name="remarks"></a>コメント  
 `CreateValue` 関数の評価で使用するための唯一の目的として、指定された型の `ICorDebugValue` オブジェクトを作成します。 この値オブジェクトを使用して、ユーザー定数をパラメーターとして渡すことができます。  
  
 値の型がプリミティブ型の場合、初期値は0または null になります。 の型の値を設定するには、を[使用します](icordebuggenericvalue-setvalue-method.md)。  
  
 `elementType` の値が ELEMENT_TYPE_CLASS 場合は、null オブジェクト参照を表す "" の値 "(`ppValue`で返される) を取得します。 このオブジェクトを使用すると、オブジェクト参照パラメーターを持つ関数の評価に null を渡すことができます。 `ICorDebugValue` を何にも設定することはできません。常に null のままです。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** 1.1、1.0  
  
## <a name="see-also"></a>関連項目

- [CreateValueForType メソッド](icordebugeval2-createvaluefortype-method.md)
- [の場合は、インターフェイス](icordebugeval-interface.md)
