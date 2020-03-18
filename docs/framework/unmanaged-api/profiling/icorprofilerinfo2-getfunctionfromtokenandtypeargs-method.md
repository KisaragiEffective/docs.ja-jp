---
title: ICorProfilerInfo2::GetFunctionFromTokenAndTypeArgs メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetFunctionFromTokenAndTypeArgs
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetFunctionFromTokenAndTypeArgs
helpviewer_keywords:
- ICorProfilerInfo2::GetFunctionFromTokenAndTypeArgs method [.NET Framework profiling]
- GetFunctionFromTokenAndTypeArgs method [.NET Framework profiling]
ms.assetid: ce8f6aa6-4ebf-4a86-b429-4bbc8af41a8f
topic_type:
- apiref
ms.openlocfilehash: 945cf84e6f6201879514e29a21f7f5462aa33fdb
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76868669"
---
# <a name="icorprofilerinfo2getfunctionfromtokenandtypeargs-method"></a>ICorProfilerInfo2::GetFunctionFromTokenAndTypeArgs メソッド
指定されたメタデータトークン、格納クラス、および任意の型引数の `ClassID` 値を使用して、関数の `FunctionID` を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetFunctionFromTokenAndTypeArgs(  
    [in] ModuleID moduleID,  
    [in] mdMethodDef funcDef,  
    [in] ClassID classId,  
    [in] ULONG32 cTypeArgs,  
    [in, size_is(cTypeArgs)] ClassID typeArgs[],  
    [out] FunctionID* pFunctionID);  
```  
  
## <a name="parameters"></a>パラメーター  
 `moduleID`  
 から関数が存在するモジュールの ID。  
  
 `funcDef`  
 から関数を参照する `mdMethodDef` メタデータトークン。  
  
 `classId`  
 からクラスを含んでいる関数の ID。  
  
 `cTypeArgs`  
 から指定された関数の型パラメーターの数。 非ジェネリック関数の場合、この値は0である必要があります。  
  
 `typeArgs`  
 から`ClassID` 値の配列。それぞれが関数の引数です。 `cTypeArgs` が0に設定されている場合、`typeArgs` の値は NULL になります。  
  
 `pFunctionID`  
 入出力指定した関数の `FunctionID` へのポインター。  
  
## <a name="remarks"></a>コメント  
 `mdMethodDef` メタデータトークンではなく `mdMethodRef` メタデータを使用して `GetFunctionFromTokenAndTypeArgs` メソッドを呼び出すと、予期しない結果になる場合があります。 呼び出し元は、`mdMethodRef` を渡すときに `mdMethodDef` に解決する必要があります。  
  
 関数がまだ読み込まれていない場合は、`GetFunctionFromTokenAndTypeArgs` を呼び出すと読み込みが発生します。これは、多くのコンテキストでは危険な操作です。 たとえば、モジュールまたは型の読み込み中にこのメソッドを呼び出すと、ランタイムが循環読み込みを試みたときに無限ループが発生する可能性があります。  
  
 一般に、`GetFunctionFromTokenAndTypeArgs` の使用は推奨されていません。 プロファイラーが特定の関数のイベントに関心を持っている場合は、その関数の `ModuleID` と `mdMethodDef` を格納し、 [ICorProfilerInfo2:: GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md)を使用して、特定の `FunctionID` が目的の関数のものであるかどうかを確認する必要があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 インターフェイス](icorprofilerinfo2-interface.md)
