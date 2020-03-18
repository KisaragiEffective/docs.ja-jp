---
title: ICorDebugEval::NewArray メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugEval.NewArray
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugEval::NewArray
helpviewer_keywords:
- NewArray method [.NET Framework debugging]
- ICorDebugEval::NewArray method [.NET Framework debugging]
ms.assetid: cc79a67d-5368-434d-a943-209db90491b9
topic_type:
- apiref
ms.openlocfilehash: 13ac5379992f4e768b09a03d31591143ba9bf627
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76788721"
---
# <a name="icordebugevalnewarray-method"></a>ICorDebugEval::NewArray メソッド
指定した要素の型と次元の新しい配列を割り当てます。  
  
 このメソッドは .NET Framework バージョン2.0 では廃止されています。 代わりに[ICorDebugEval2:: NewParameterizedArray](icordebugeval2-newparameterizedarray-method.md)を使用してください。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT NewArray (  
    [in] CorElementType     elementType,  
    [in] ICorDebugClass     *pElementClass,  
    [in] ULONG32            rank,  
    [in, size_is(rank)] ULONG32 dims[],  
    [in, size_is(rank)] ULONG32 lowBounds[]  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `elementType`  
 から配列の要素の型を指定する CorElementType 列挙体の値。  
  
 `pElementClass`  
 から要素のクラスを指定する、ツールオブジェクトへのポインター。 要素の型がプリミティブ型の場合、この値は null になることがあります。  
  
 `rank`  
 から配列の次元数。 .NET Framework 2.0 では、この値は1である必要があります。  
  
 `dims`  
 から配列の各次元のサイズ (バイト単位)。  
  
 `lowBounds`  
 [in] オプション。 配列の各次元の下限。 この値を省略すると、次元ごとに下限0が想定されます。  
  
## <a name="remarks"></a>コメント  
 配列は常に、スレッドが現在実行されているアプリケーションドメインで作成されます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** 1.1、1.0
