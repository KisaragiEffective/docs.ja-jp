---
title: ICorProfilerCallback9 インターフェイス
ms.date: 04/10/2018
api_name:
- ICorProfilerCallback9
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: c383a2e221e61770d3c28a65c561c48f6059b6d6
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73136561"
---
# <a name="icorprofilercallback9-interface"></a>ICorProfilerCallback9 インターフェイス
[.NET Framework 4.7.2 以降のバージョンでサポートされています]  

 動的メソッドがガベージコレクションされ、その後アンロードされたことをプロファイラーに通知するために、共通言語ランタイムによって使用されるコールバックメソッドを提供する[ICorProfilerCallback8](icorprofilercallback8-interface.md)のサブクラスです。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[DynamicMethodUnloaded メソッド](ICorProfilerCallback9-dynamicmethodunloaded-method.md)|動的メソッドがガベージコレクションされ、その後アンロードされたことをプロファイラーに通知します。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  

## <a name="see-also"></a>関連項目

- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [ICorProfilerCallback8 インターフェイス](icorprofilercallback9-interface.md)
- [ICorProfilerCallback8 DynamicMethodJITCompilationStarted メソッド](icorprofilercallback8-dynamicmethodjitcompilationstarted-method.md)
- [ICorProfilerCallback8 DynamicMethodJITCompilationFinished メソッド](icorprofilercallback8-dynamicmethodjitcompilationfinished-method.md)
