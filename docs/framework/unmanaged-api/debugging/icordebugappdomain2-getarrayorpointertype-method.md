---
title: ICorDebugAppDomain2::GetArrayOrPointerType メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugAppDomain2.GetArrayOrPointerType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugAppDomain2::GetArrayOrPointerType
helpviewer_keywords:
- GetArrayOrPointerType method [.NET Framework debugging]
- ICorDebugAppDomain2::GetArrayOrPointerType method [.NET Framework debugging]
ms.assetid: 97e493f5-3a62-4ec7-b42f-4af57bf71f57
topic_type:
- apiref
ms.openlocfilehash: 166f6bb50849df8550871958d7034fdf2a841abb
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73089116"
---
# <a name="icordebugappdomain2getarrayorpointertype-method"></a>ICorDebugAppDomain2::GetArrayOrPointerType メソッド
指定した型、または指定した型へのポインターまたは参照の配列を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetArrayOrPointerType (  
    [in]  CorElementType    elementType,  
    [in]  ULONG32           nRank,  
    [in]  ICorDebugType     *pTypeArg,  
    [out] ICorDebugType     **ppType  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `elementType`  
 から作成する基になるネイティブ型 (配列、ポインター、または参照) を指定する CorElementType 列挙体の値。  
  
 `nRank`  
 から配列のランク (次元の数)。 `elementType` がポインターまたは参照型を指定する場合、この値は0にする必要があります。  
  
 `pTypeArg`  
 から作成される配列、ポインター、または参照の型を表す、の型のオブジェクトへのポインター。  
  
 `ppType`  
 入出力構築された配列、ポインター型、または参照型を表す `ICorDebugType` オブジェクトのアドレスへのポインター。  
  
## <a name="remarks"></a>Remarks  
 *ElementType*の値には、次のいずれかを指定する必要があります。  
  
- ELEMENT_TYPE_PTR  
  
- ELEMENT_TYPE_BYREF  
  
- ELEMENT_TYPE_ARRAY または ELEMENT_TYPE_SZARRAY  
  
 *ElementType*の値が ELEMENT_TYPE_PTR または ELEMENT_TYPE_BYREF の場合、 *nrank*は0にする必要があります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
