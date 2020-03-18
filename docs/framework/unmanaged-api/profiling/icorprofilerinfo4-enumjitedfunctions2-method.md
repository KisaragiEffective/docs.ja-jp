---
title: ICorProfilerInfo4::EnumJITedFunctions2 メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4.EnumJITedFunctions2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4::EnumJITedFunctions2
helpviewer_keywords:
- EnumJITedFunctions2 method, ICorProfilerInfo4 interface [.NET Framework profiling]
- ICorProfilerInfo4::EnumJITedFunctions2 method [.NET Framework profiling]
ms.assetid: 40e9a1be-9bd2-4fad-9921-34a84b61c1e3
topic_type:
- apiref
ms.openlocfilehash: 3903ebf1ad35bd7eb1ba49b4f1acda9024678423
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76862205"
---
# <a name="icorprofilerinfo4enumjitedfunctions2-method"></a>ICorProfilerInfo4::EnumJITedFunctions2 メソッド
以前に JIT コンパイルおよび JIT 再コンパイルされたすべての関数の列挙子を返します。 このメソッドは、JIT 再コンパイルされた Id を列挙しない[ICorProfilerInfo3:: EnumJITedFunctions](icorprofilerinfo3-enumjitedfunctions-method.md)メソッドを置き換えます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EnumJITedFunctions([out] ICorProfilerFunctionEnum** ppEnum);  
```  
  
## <a name="parameters"></a>パラメーター  
 `ppEnum`  
 入出力[ICorProfilerFunctionEnum](icorprofilerfunctionenum-interface.md)列挙子へのポインター。  
  
## <a name="remarks"></a>コメント  
 このメソッドは、 [ICorProfilerCallback:: JITCompilationStarted](icorprofilercallback-jitcompilationstarted-method.md)メソッドなどの `JITCompilation` コールバックと重複する場合があります。 返される列挙体には、`COR_PRF_FUNCTION::reJitId` フィールドの値が含まれます。 `COR_PRF_FUNCTION::reJitId` フィールドは常に0に設定されているため、このメソッドで置き換える[ICorProfilerInfo3:: EnumJITedFunctions](icorprofilerinfo3-enumjitedfunctions-method.md)メソッドは、JIT 再コンパイルされた id を列挙しません。 `COR_PRF_FUNCTION::reJitId` フィールドが適切に設定されているため、`ICorProfilerInfo4::EnumJITedFunctions` メソッドは JIT 再コンパイルされた Id を列挙します。 [ICorProfilerInfo4:: EnumJITedFunctions2](icorprofilerinfo4-enumjitedfunctions2-method.md)メソッドではガベージコレクションをトリガーできるのに対し、 [ICorProfilerInfo3:: EnumJITedFunctions メソッド](icorprofilerinfo3-enumjitedfunctions-method.md)ではトリガーされないことに注意してください。  詳細については、「 [HRESULT CORPROF_E_UNSUPPORTED_CALL_SEQUENCE](corprof-e-unsupported-call-sequence-hresult.md)」を参照してください。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [EnumJITedFunctions メソッド](icorprofilerinfo3-enumjitedfunctions-method.md)
- [ICorProfilerInfo4 インターフェイス](icorprofilerinfo4-interface.md)
- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [プロファイル](index.md)
