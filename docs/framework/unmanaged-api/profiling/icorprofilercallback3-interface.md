---
title: ICorProfilerCallback3 インターフェイス
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback3
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback3
helpviewer_keywords:
- ICorProfilerCallback3 interface [.NET Framework profiling]
ms.assetid: be83af41-3dec-4c77-8529-9dd6b8042af6
topic_type:
- apiref
ms.openlocfilehash: ad9c5445cbef0be7919570db64b027556d923752
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76865572"
---
# <a name="icorprofilercallback3-interface"></a>ICorProfilerCallback3 インターフェイス
共通言語ランタイム (CLR) が、プロファイラーにアタッチおよびデタッチ状態情報を伝達するために使用するコールバックメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[InitializeForAttach メソッド](icorprofilercallback3-initializeforattach-method.md)|アタッチ操作後にその状態を初期化する機会をプロファイラーに与えるために、CLR によって呼び出されます。|  
|[ProfilerAttachComplete メソッド](icorprofilercallback3-profilerattachcomplete-method.md)|プロファイラーがキャッチアップメソッドを呼び出せるようになったことを示すために、CLR によって呼び出されます。|  
|[ProfilerDetachSucceeded メソッド](icorprofilercallback3-profilerdetachsucceeded-method.md)|共通言語ランタイム (CLR: Common Language Runtime) がプロファイラー DLL をアンロードしようとしていることをプロファイラーに通知します。|  
  
## <a name="remarks"></a>Remarks  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
- [ICorProfilerCallback2 インターフェイス](icorprofilercallback2-interface.md)
- [ICorProfilerCallback4 インターフェイス](icorprofilercallback4-interface.md)
