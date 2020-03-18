---
title: COR_PRF_SUSPEND_REASON 列挙型
ms.date: 03/30/2017
api_name:
- COR_PRF_SUSPEND_REASON
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_SUSPEND_REASON
helpviewer_keywords:
- COR_PRF_SUSPEND_REASON enumeration [.NET Framework profiling]
ms.assetid: 75594833-bed3-47b2-a426-b75c5fe6fbcf
topic_type:
- apiref
ms.openlocfilehash: 1036ecbdb735b95c0ad6897c1545e3bd8cb6c3a9
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76867075"
---
# <a name="cor_prf_suspend_reason-enumeration"></a>COR_PRF_SUSPEND_REASON 列挙型
ランタイムが中断された理由を示します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum {  
    COR_PRF_SUSPEND_OTHER                   = 0x00,  
    COR_PRF_SUSPEND_FOR_GC                  = 0x01,  
    COR_PRF_SUSPEND_FOR_APPDOMAIN_SHUTDOWN  = 0x02,  
    COR_PRF_SUSPEND_FOR_CODE_PITCHING       = 0x03,  
    COR_PRF_SUSPEND_FOR_SHUTDOWN            = 0x04,  
    COR_PRF_SUSPEND_FOR_INPROC_DEBUGGER     = 0x06,  
    COR_PRF_SUSPEND_FOR_GC_PREP             = 0x07,    COR_PRF_SUSPEND_FOR_REJIT               = 8  
} COR_PRF_SUSPEND_REASON;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`COR_PRF_FIELD_SUSPEND_OTHER`|ランタイムは、不特定の理由で中断されています。|  
|`COR_PRF_FIELD_SUSPEND_FOR_GC`|ランタイムは、ガベージコレクション要求を処理するために中断されています。<br /><br /> ガベージコレクションに関連するコールバックは、 [ICorProfilerCallback:: RuntimeSuspendFinished](icorprofilercallback-runtimesuspendfinished-method.md)と[ICorProfilerCallback:: RuntimeResumeStarted](icorprofilercallback-runtimeresumestarted-method.md)コールバックの間で発生します。|  
|`COR_PRF_FIELD_SUSPEND_FOR_APPDOMAIN_SHUTDOWN`|`AppDomain` をシャットダウンできるように、ランタイムは中断されています。<br /><br /> ランタイムが中断されている間、ランタイムは、シャットダウンされている `AppDomain` 内のスレッドを特定し、再開時に中止するように設定します。 この中断中に `AppDomain`固有のコールバックはありません。|  
|`COR_PRF_FIELD_SUSPEND_FOR_CODE_PITCHING`|コードピッチが発生するようにランタイムが中断されています。<br /><br /> Code ピッチ ensues は、just-in-time (JIT) コンパイラがアクティブであり、コードピッチが有効になっている場合にのみ使用します。 Code ピッチコールバックは、`ICorProfilerCallback::RuntimeSuspendFinished` と `ICorProfilerCallback::RuntimeResumeStarted` コールバックの間で発生します。 **注:** CLR JIT は .NET Framework バージョン2.0 の関数のピッチを調整しないため、この値は2.0 では使用されません。|  
|`COR_PRF_FIELD_SUSPEND_FOR_SHUTDOWN`|ランタイムは、シャットダウンできるように中断されています。 操作を完了するには、すべてのスレッドを中断する必要があります。|  
|`COR_PRF_FIELD_SUSPEND_FOR_INPROC_DEBUGGER`|ランタイムは、インプロセスデバッグのために中断されています。|  
|`COR_PRF_FIELD_SUSPEND_FOR_GC_PREP`|ランタイムは、ガベージコレクションの準備のために中断されています。|  
|`COR_PRF_SUSPEND_FOR_REJIT`|ランタイムは JIT 再コンパイルのために中断されています。|  
  
## <a name="remarks"></a>Remarks  
 アンマネージコード内のすべてのランタイムスレッドは、ランタイムを再入力しようとするまで実行を継続することができます。ランタイムは、ランタイムが再開されるまで中断されます。 これは、ランタイムに入る新しいスレッドにも当てはまります。 ランタイム内のすべてのスレッドは、中断可能なコードに含まれている場合はすぐに中断されます。または、中断可能なコードに到着したときに中断するように求められます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [列挙型のプロファイリング](profiling-enumerations.md)
