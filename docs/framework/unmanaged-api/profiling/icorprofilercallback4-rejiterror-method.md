---
title: ICorProfilerCallback4::ReJITError メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback4.ReJITError
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback4::ReJITError
helpviewer_keywords:
- ReJITError method, ICorProfilerCallback4 interface [.NET Framework profiling]
- ICorProfilerCallback4::ReJITError method [.NET Framework profiling]
ms.assetid: d7888aa9-dfaa-420f-9f99-e06ab35ca482
topic_type:
- apiref
ms.openlocfilehash: 66195ea9df4c8e9ce847b38f7d020a3bebffcd37
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76865182"
---
# <a name="icorprofilercallback4rejiterror-method"></a>ICorProfilerCallback4::ReJITError メソッド
Just-in-time (JIT) コンパイラが再コンパイルプロセスでエラーを検出したことをプロファイラーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT ReJITError(  
    [in] ModuleID    moduleId,  
    [in] mdMethodDef methodId,  
    [in] FunctionID  functionId,  
    [in] HRESULT     hrStatus);  
```  
  
## <a name="parameters"></a>パラメーター  
 `moduleID`  
 から再コンパイルの試行が失敗した `ModuleID`。  
  
 `methodId`  
 から再コンパイルの試行が失敗したメソッドの `MethodDef`。  
  
 `functionId`  
 から再コンパイルまたは再コンパイルのマークが付けられている関数インスタンス。 この値は、インスタンス化ごとにではなく、メソッドごとにエラーが発生した場合に `NULL` ことがあります (たとえば、プロファイラーがメソッドを再コンパイルするために無効なメタデータトークンを指定した場合など)。  
  
 `hrStatus`  
 からエラーの性質を示す HRESULT。 値の一覧については、「Status HRESULT」セクションを参照してください。  
  
## <a name="return-value"></a>戻り値  
 このコールバックからの戻り値は無視されます。  
  
## <a name="status-hresults"></a>状態 HRESULT  
  
|状態配列 HRESULT|説明|  
|--------------------------|-----------------|  
|E_INVALIDARG|`moduleID` または `methodDef` トークンが `NULL`。|  
|CORPROF_E_DATAINCOMPLETE|モジュールが完全に読み込まれていないか、またはアンロード中です。|  
|CORPROF_E_MODULE_IS_DYNAMIC|指定されたモジュールは (たとえば、`Reflection.Emit`によって) 動的に生成されたため、このメソッドではサポートされません。|  
|CORPROF_E_FUNCTION_IS_COLLECTIBLE|メソッドは、収集可能なアセンブリにインスタンス化されるため、再コンパイルできません。 非リフレクションコンテキストで定義されている型および関数 (`List<MyCollectibleStruct>`など) は、収集可能なアセンブリにインスタンス化できます。|  
|E_OUTOFMEMORY|CLR は、指定されたメソッドに JIT 再コンパイルのマークを付けようとしている間にメモリ不足になりました。|  
|その他|オペレーティング システムは、CLR 制御範囲外のエラーを返しました。 たとえば、メモリページのアクセス保護を変更するシステムコールが失敗した場合、オペレーティングシステムエラーが表示されます。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
- [ICorProfilerCallback4 インターフェイス](icorprofilercallback4-interface.md)
