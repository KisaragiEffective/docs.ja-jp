---
title: ICorProfilerInfo2::GetClassFromTokenAndTypeArgs メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetClassFromTokenAndTypeArgs
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetClassFromTokenAndTypeArgs
helpviewer_keywords:
- GetClassFromTokenAndTypeArgs method [.NET Framework profiling]
- ICorProfilerInfo2::GetClassFromTokenAndTypeArgs method [.NET Framework profiling]
ms.assetid: b25c88f0-71b9-443b-8eea-1c94db0a44b9
topic_type:
- apiref
ms.openlocfilehash: e0663ff122397ba639a0a219e513be2f3f0cbbef
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76862777"
---
# <a name="icorprofilerinfo2getclassfromtokenandtypeargs-method"></a>ICorProfilerInfo2::GetClassFromTokenAndTypeArgs メソッド
指定されたメタデータトークンと任意の型引数の `ClassID` 値を使用して、型の `ClassID` を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetClassFromTokenAndTypeArgs(  
    [in] ModuleID moduleID,  
    [in] mdTypeDef typeDef,  
    [in] ULONG32 cTypeArgs,  
    [in, size_is(cTypeArgs)] ClassID typeArgs[],  
    [out] ClassID* pClassID);  
```  
  
## <a name="parameters"></a>パラメーター  
 `moduleID`  
 から型が存在するモジュールの ID。  
  
 `typeDef`  
 から型を参照する `mdTypeDef` メタデータトークン。  
  
 `cTypeArgs`  
 から指定された型の型パラメーターの数。 非ジェネリック型の場合、この値は0である必要があります。  
  
 `typeArgs`  
 から`ClassID` 値の配列。それぞれが型の引数です。 `cTypeArgs` が0に設定されている場合、`typeArgs` の値は NULL になります。  
  
 `pClassID`  
 入出力指定した型の `ClassID` へのポインター。  
  
## <a name="remarks"></a>コメント  
 `mdTypeDef` メタデータトークンではなく `mdTypeRef` を使用して `GetClassFromTokenAndTypeArgs` メソッドを呼び出すと、予期しない結果が発生する可能性があります。呼び出し元は、`mdTypeRef` を渡すときに `mdTypeDef` に解決する必要があります。  
  
 型がまだ読み込まれていない場合は、`GetClassFromTokenAndTypeArgs` を呼び出すと読み込みがトリガーされます。これは、多くのコンテキストでは危険な操作です。 たとえば、モジュールまたはその他の型の読み込み中にこのメソッドを呼び出すと、ランタイムが循環読み込みを試みたときに無限ループが発生する可能性があります。  
  
 一般に、`GetClassFromTokenAndTypeArgs` の使用は推奨されていません。 プロファイラーが特定の型のイベントに関心を持っている場合は、その型の `ModuleID` と `mdTypeDef` を格納し、 [ICorProfilerInfo2:: GetClassIDInfo2](icorprofilerinfo2-getclassidinfo2-method.md)を使用して、特定の `ClassID` が目的の型であるかどうかを確認する必要があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 インターフェイス](icorprofilerinfo2-interface.md)
