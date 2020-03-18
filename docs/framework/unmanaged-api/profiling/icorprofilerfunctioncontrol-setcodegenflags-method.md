---
title: ICorProfilerFunctionControl::SetCodegenFlags メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerFunctionControl.SetCodegenFlags
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerFunctionControl::SetCodegenFlags
helpviewer_keywords:
- ICorProfilerFunctionControl::SetCodegenFlags method [.NET Framework profiling]
- SetCodegenFlags method, ICorProfilerFunctionControl interface [.NET Framework profiling]
ms.assetid: a2d5daa5-b990-4ae5-bf2a-c0862fe58bd7
topic_type:
- apiref
ms.openlocfilehash: 75414ec3d2b30fe8afc5db1e97c81f34b29a3115
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76864675"
---
# <a name="icorprofilerfunctioncontrolsetcodegenflags-method"></a>ICorProfilerFunctionControl::SetCodegenFlags メソッド
[COR_PRF_CODEGEN_FLAGS](cor-prf-codegen-flags-enumeration.md)列挙型の1つ以上のフラグを設定して、JUST-IN-TIME (JIT) 再コンパイル関数のコード生成を制御します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetCodegenFlags(  
    [in] DWORD flags);  
```  
  
## <a name="parameters"></a>パラメーター  
 `flags`  
 から[COR_PRF_CODEGEN_FLAGS](cor-prf-codegen-flags-enumeration.md)列挙体の1つ以上のフラグ。  
  
## <a name="remarks"></a>コメント  
 プロファイラーは、 [ICorProfilerCallback4:: GetReJITParameters](icorprofilercallback4-getrejitparameters-method.md)コールバックを使用して、このインターフェイスのインスタンスを取得します。 `SetCodegenFlags` を使用すると、再コンパイルされた関数のコード生成をプロファイラーで制御できます。 他のすべての JIT 再コンパイルパラメーターと同様に、コード生成フラグは関数のすべてのインスタンスに適用されます。  
  
 JIT コンパイラは、関数をコンパイルするときに、これらのコンパイルフラグを他のソースによって指定された他のフラグと共に考慮します。  その他のソースには、デバッガー、 [ICorProfilerInfo:: SetEventMask](icorprofilerinfo-seteventmask-method.md)メソッド (値 `COR_PRF_DISABLE_INLINING` と `COR_PRF_DISABLE_OPTIMIZATIONS`)、およびプロファイラーの[ICorProfilerCallback:: JITInlining](icorprofilercallback-jitinlining-method.md) callback を使用して、起動時にプロファイラーによって設定されるグローバルフラグが含まれます。  JIT コンパイラは、最小限の最適化を要求するソースに優先順位を付けます。  たとえば、プロファイラーが起動時に `COR_PRF_DISABLE_INLINING` を指定していても、 [ICorProfilerFunctionControl:: SetCodegenFlags](icorprofilerfunctioncontrol-setcodegenflags-method.md)コールバックで `COR_PRF_CODEGEN_DISABLE_INLINING` を指定していない場合でも、インライン展開は無効になります。  同様に、プロファイラーが `SetCodegenFlags`で `COR_PRF_CODEGEN_DISABLE_INLINING` を指定せず、 [ICorProfilerCallback:: JITInlining](icorprofilercallback-jitinlining-method.md)コールバックを使用してインライン展開を無効にした場合、インライン展開は無効になります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerFunctionControl インターフェイス](icorprofilerfunctioncontrol-interface.md)
