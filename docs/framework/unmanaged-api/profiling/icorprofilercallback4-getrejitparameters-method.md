---
title: ICorProfilerCallback4::GetReJITParameters メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback4.GetReJITParameters
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback4::GetReJITParameters
helpviewer_keywords:
- ICorProfilerCallback4::GetReJITParameters method [.NET Framework profiling]
- GetReJITParameters method, ICorProfilerCallback4 interface [.NET Framework profiling]
ms.assetid: 0e9bfe07-9f20-498c-b568-9017c8f6056c
topic_type:
- apiref
ms.openlocfilehash: 858d65783515a89a434cf719ef9d5a999643094c
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76865312"
---
# <a name="icorprofilercallback4getrejitparameters-method"></a>ICorProfilerCallback4::GetReJITParameters メソッド
再コンパイルされた新しいメソッド本体の代替コード生成フラグをコードプロファイラーで設定できるようにします。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetReJITParameters(     [in] ModuleID moduleId,     [in] mdMethodDef methodId,     [in] ICorProfilerFunctionControl *pFunctionControl);  
```  
  
## <a name="parameters"></a>パラメーター  
 `moduleID`  
 からCLR が JIT 再コンパイルパラメーターを必要とするメソッドを含むモジュール。  
  
 `methodId`  
 からCLR が JIT 再コンパイルパラメーターを必要とするメソッドの `MethodDef`。  
  
 `pFunctionControl`  
 から再コンパイルされるメソッドの JIT 再コンパイル情報を提供するためにプロファイラーが使用できる[ICorProfilerFunctionControl](icorprofilerfunctioncontrol-interface.md)インターフェイスへのポインター。  
  
## <a name="remarks"></a>Remarks  
 CLR は、指定されたメソッドを再コンパイルするためのパラメーターをプロファイラーが指定できるように、`GetReJITParameters` コールバックを発行します。 `GetReJITParameters` コールバックは、関数ごとに1回のみ発行されます。プロファイラーによって提供されるパラメーターは、その関数のすべてのインスタンスに適用されます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
- [ICorProfilerCallback4 インターフェイス](icorprofilercallback4-interface.md)
- [JITCompilationStarted メソッド](icorprofilercallback-jitcompilationstarted-method.md)
- [ReJITCompilationStarted メソッド](icorprofilercallback4-rejitcompilationstarted-method.md)
