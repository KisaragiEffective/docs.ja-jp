---
title: ICorDebugType::EnumerateTypeParameters メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugType.EnumerateTypeParameters
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugType::EnumerateTypeParameters
helpviewer_keywords:
- EnumerateTypeParameters method, ICorDebugType interface [.NET Framework debugging]
- ICorDebugType::EnumerateTypeParameters method [.NET Framework debugging]
ms.assetid: 1ee1f6e6-1bd7-4ebb-83b8-ff9a08ca03de
topic_type:
- apiref
ms.openlocfilehash: b2c381d093069f5ee86be1b19d75f5c2d69ad9fa
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791306"
---
# <a name="icordebugtypeenumeratetypeparameters-method"></a>ICorDebugType::EnumerateTypeParameters メソッド
このテキストの型によって参照されるクラスの <xref:System.Type> パラメーターを格納している、テキスト型へのインターフェイスポインターを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EnumerateTypeParameters (  
    [out] ICorDebugTypeEnum   **ppTyParEnum  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `ppTyParEnum`  
 入出力型のパラメーターを格納している `ICorDebugTypeEnum` のアドレスへのポインター。  
  
## <a name="remarks"></a>コメント  
 `EnumerateTypeParameters` を使用することができます、によって返される CorElementType[値は、](icordebugtype-gettype-method.md) ELEMENT_TYPE_CLASS、ELEMENT_TYPE_VALUETYPE、ELEMENT_TYPE_ARRAY、ELEMENT_TYPE_SZARRAY、ELEMENT_TYPE_BYREF、ELEMENT_TYPE_PTR、または ELEMENT_TYPE_FNPTR です。 パラメーターの数と順序は、型によって異なります。  
  
- ELEMENT_TYPE_CLASS または ELEMENT_TYPE_VALUETYPE: このメソッドが返す `ICorDebugTypeEnum` に含まれる型パラメーターの数は、対応するクラスの仮型パラメーターの数によって異なります。 たとえば、型が `class Dict<String,int32>`の場合、`EnumerateTypeParameters` は `String` および `int32` を表すオブジェクトを順番に含む `ICorDebugTypeEnum` を返します。  
  
- ELEMENT_TYPE_FNPTR: `ICorDebugTypeEnum` に格納されている型パラメーターの数は、関数で許容される引数の数より1だけ大きくなります。 `ICorDebugTypeEnum` に格納されている最初の型パラメーターは、関数の戻り値の型であり、後続の型パラメーターは関数のパラメーターです。  
  
- ELEMENT_TYPE_ARRAY、ELEMENT_TYPE_SZARRAY、ELEMENT_TYPE_BYREF、または ELEMENT_TYPE_PTR: 1 つの型パラメーターが返されます。 たとえば、型が `int32[]`などの配列型である場合、`EnumerateTypeParameters` は `int32`を表すオブジェクトを含む `ICorDebugTypeEnum` を返します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
