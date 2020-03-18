---
title: ICorDebugEval::NewObject メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugEval.NewObject
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugEval::NewObject
helpviewer_keywords:
- NewObject method [.NET Framework debugging]
- ICorDebugEval::NewObject method [.NET Framework debugging]
ms.assetid: ce3025e8-defa-4c5e-8298-f49d71fa5736
topic_type:
- apiref
ms.openlocfilehash: 38cc98f1bfd966d1f764e43b30003a2bae66297d
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76793467"
---
# <a name="icordebugevalnewobject-method"></a>ICorDebugEval::NewObject メソッド
新しいオブジェクトインスタンスを割り当て、指定したコンストラクターメソッドを呼び出します。  
  
 このメソッドは .NET Framework バージョン2.0 では廃止されています。 代わりに[ICorDebugEval2:: NewParameterizedObject](icordebugeval2-newparameterizedobject-method.md)を使用してください。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT NewObject (  
    [in] ICorDebugFunction  *pConstructor,  
    [in] ULONG32            nArgs,  
    [in, size_is(nArgs)] ICorDebugValue *ppArgs[]  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pConstructor`  
 から呼び出されるコンストラクター。  
  
 `nArgs`  
 [in] `ppArgs` 配列のサイズ。  
  
 `ppArgs`  
 からICorDebugValue オブジェクトの配列。各オブジェクトは、コンストラクターに渡される引数を表します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** 1.1、1.0  
  
## <a name="see-also"></a>関連項目

- [NewParameterizedObject メソッド](icordebugeval2-newparameterizedobject-method.md)
