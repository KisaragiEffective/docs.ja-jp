---
title: COR_PRF_MONITOR 列挙型
ms.date: 03/30/2017
api_name:
- COR_PRF_MONITOR
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_MONITOR
helpviewer_keywords:
- COR_PRF_MONITOR enumeration [.NET Framework profiling]
ms.assetid: 9294d702-b4e5-441c-a930-e63d27b86bfd
topic_type:
- apiref
ms.openlocfilehash: 2d7984ce109fb2bac5a36ab5e4c83f386de5a488
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76867101"
---
# <a name="cor_prf_monitor-enumeration"></a>COR_PRF_MONITOR 列挙型
プロファイラーがサブスクライブしようとする動作、機能、またはイベントの指定で使用する値を含めます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum {  
    COR_PRF_MONITOR_NONE                = 0x00000000,  
    COR_PRF_MONITOR_FUNCTION_UNLOADS    = 0x00000001,  
    COR_PRF_MONITOR_CLASS_LOADS         = 0x00000002,  
    COR_PRF_MONITOR_MODULE_LOADS        = 0x00000004,  
    COR_PRF_MONITOR_ASSEMBLY_LOADS      = 0x00000008,  
    COR_PRF_MONITOR_APPDOMAIN_LOADS     = 0x00000010,  
    COR_PRF_MONITOR_JIT_COMPILATION     = 0x00000020,  
    COR_PRF_MONITOR_EXCEPTIONS          = 0x00000040,  
    COR_PRF_MONITOR_GC                  = 0x00000080,  
    COR_PRF_MONITOR_OBJECT_ALLOCATED    = 0x00000100,  
    COR_PRF_MONITOR_THREADS             = 0x00000200,  
    COR_PRF_MONITOR_REMOTING            = 0x00000400,  
    COR_PRF_MONITOR_CODE_TRANSITIONS    = 0x00000800,  
    COR_PRF_MONITOR_ENTERLEAVE          = 0x00001000,  
    COR_PRF_MONITOR_CCW                 = 0x00002000,  
    COR_PRF_MONITOR_REMOTING_COOKIE     = 0x00004000 |   
                                          COR_PRF_MONITOR_REMOTING,  
    COR_PRF_MONITOR_REMOTING_ASYNC      = 0x00008000 |   
                                          COR_PRF_MONITOR_REMOTING,  
    COR_PRF_MONITOR_SUSPENDS            = 0x00010000,  
    COR_PRF_MONITOR_CACHE_SEARCHES      = 0x00020000,  
    COR_PRF_ENABLE_REJIT                = 0x00040000,  
    COR_PRF_ENABLE_INPROC_DEBUGGING     = 0x00080000,  
    COR_PRF_ENABLE_JIT_MAPS             = 0x00100000,  
    COR_PRF_DISABLE_INLINING            = 0x00200000,  
    COR_PRF_DISABLE_OPTIMIZATIONS       = 0x00400000,  
    COR_PRF_ENABLE_OBJECT_ALLOCATED     = 0x00800000,  
    COR_PRF_MONITOR_CLR_EXCEPTIONS      = 0x01000000,  
    COR_PRF_MONITOR_ALL                 = 0x0107FFFF,  
    COR_PRF_ENABLE_FUNCTION_ARGS        = 0X02000000,  
    COR_PRF_ENABLE_FUNCTION_RETVAL      = 0X04000000,  
    COR_PRF_ENABLE_FRAME_INFO           = 0X08000000,  
    COR_PRF_ENABLE_STACK_SNAPSHOT       = 0X10000000,  
    COR_PRF_USE_PROFILE_IMAGES          = 0x20000000,  
    COR_PRF_DISABLE_TRANSPARENCY_CHECKS_UNDER_FULL_TRUST  
                                        = 0x40000000,  
    COR_PRF_DISABLE_ALL_NGEN_IMAGES     = 0x80000000,  
    COR_PRF_ALL                         = 0x8FFFFFFF,  
    COR_PRF_REQUIRE_PROFILE_IMAGE       = COR_PRF_USE_PROFILE_IMAGES |   
                                          COR_PRF_MONITOR_CODE_TRANSITIONS |   
                                          COR_PRF_MONITOR_ENTERLEAVE,  
    COR_PRF_ALLOWABLE_AFTER_ATTACH      = COR_PRF_MONITOR_THREADS |  
                                          COR_PRF_MONITOR_MODULE_LOADS |  
                                          COR_PRF_MONITOR_ASSEMBLY_LOADS |  
                                          COR_PRF_MONITOR_APPDOMAIN_LOADS |  
                                          COR_PRF_ENABLE_STACK_SNAPSHOT |  
                                          COR_PRF_MONITOR_GC |  
                                          COR_PRF_MONITOR_SUSPENDS |  
                                          COR_PRF_MONITOR_CLASS_LOADS |  
                                          COR_PRF_MONITOR_JIT_COMPILATION,  
    COR_PRF_MONITOR_IMMUTABLE           = COR_PRF_MONITOR_CODE_TRANSITIONS |  
                                          COR_PRF_MONITOR_REMOTING |  
                                          COR_PRF_MONITOR_REMOTING_COOKIE |  
                                          COR_PRF_MONITOR_REMOTING_ASYNC |  
                                          COR_PRF_ENABLE_REJIT |  
                                          COR_PRF_ENABLE_INPROC_DEBUGGING |  
                                          COR_PRF_ENABLE_JIT_MAPS |  
                                          COR_PRF_DISABLE_OPTIMIZATIONS |  
                                          COR_PRF_DISABLE_INLINING |  
                                          COR_PRF_ENABLE_OBJECT_ALLOCATED |  
                                          COR_PRF_ENABLE_FUNCTION_ARGS |  
                                          COR_PRF_ENABLE_FUNCTION_RETVAL |  
                                          COR_PRF_ENABLE_FRAME_INFO |  
                                          COR_PRF_USE_PROFILE_IMAGES |  
                     COR_PRF_DISABLE_TRANSPARENCY_CHECKS_UNDER_FULL_TRUST |  
                                          COR_PRF_DISABLE_ALL_NGEN_IMAGES  
} COR_PRF_MONITOR;  
```  
  
## <a name="members"></a>メンバー  
 次のセクションでは、`COR_PRF_MONITOR` 列挙型メンバーをカテゴリ別に示します。 カテゴリは次のとおりです。  
  
- [フラグが設定していません](#None)  
  
- [コールバックフラグ](#Callback)  
  
- [機能の有効化フラグ](#Feature)  
  
- [構成フラグ](#Config)  
  
- [複合フラグ](#Composite)  
  
<a name="None"></a>   
### <a name="no-flags-set"></a>フラグが設定していません  
  
|メンバー|説明|  
|------------|-----------------|  
|`COR_PRF_MONITOR_NONE`|フラグが設定されていません。|  
  
<a name="Callback"></a>   
### <a name="callback-flags"></a>コールバック フラグ  
  
|メンバー|説明|  
|------------|-----------------|  
|`COR_PRF_MONITOR_ALL`|すべてのコールバック イベントを有効にします。|  
|`COR_PRF_MONITOR_APPDOMAIN_LOADS`|[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスの `AppDomainCreation*` および `AppDomainShutdown*` コールバックを制御します。|  
|`COR_PRF_MONITOR_ASSEMBLY_LOADS`|[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスの `AssemblyLoad*` および `AssemblyUnload*` コールバックを制御します。|  
|`COR_PRF_MONITOR_CACHE_SEARCHES`|[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスのコールバック `JITCachedFunctionSearch*` を制御します。<br /><br /> このフラグの動作は、.NET Framework バージョン 2.0 で変更されています。|  
|`COR_PRF_MONITOR_CCW`|[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスのコールバック `COMClassicVTable*` を制御します。|  
|`COR_PRF_MONITOR_CLASS_LOADS`|[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスの `ClassLoad*` および `ClassUnload*` コールバックを制御します。|  
|`COR_PRF_MONITOR_CLR_EXCEPTIONS`|[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスのコールバック `ExceptionCLRCatcher*` を制御します。|  
|`COR_PRF_MONITOR_CODE_TRANSITIONS`|[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイス内の[UnmanagedToManagedTransition](icorprofilercallback-unmanagedtomanagedtransition-method.md)および[ManagedToUnmanagedTransition](icorprofilercallback-managedtounmanagedtransition-method.md)コールバックを制御します。|  
|`COR_PRF_MONITOR_ENTERLEAVE`|[グローバル静的関数](profiling-global-static-functions.md)`FunctionEnter*`、`FunctionLeave*`、および `FunctionTailCall*`を制御します。|  
|`COR_PRF_MONITOR_EXCEPTIONS`|[Exceptionthrown](icorprofilercallback-exceptionthrown-method.md)されたコールバックと、 [ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスの `ExceptionSearch*`、`ExceptionOSHandler*`、`ExceptionUnwind*`、および `ExceptionCatcher*` のコールバックを制御します。|  
|`COR_PRF_MONITOR_FUNCTION_UNLOADS`|[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスの[FunctionUnloadStarted](icorprofilercallback-functionunloadstarted-method.md)コールバックを制御します。|  
|`COR_PRF_MONITOR_GC`|`ICorProfilerCallback*` インターフェイスの[GarbageCollectionStarted](icorprofilercallback2-garbagecollectionstarted-method.md)、 [GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md)、 [movedreferences](icorprofilercallback-movedreferences-method.md)、 [MovedReferences2](icorprofilercallback4-movedreferences2-method.md)、 [SurvivingReferences](icorprofilercallback2-survivingreferences-method.md)、 [SurvivingReferences2](icorprofilercallback4-survivingreferences2-method.md)、 [ObjectReferences](icorprofilercallback-objectreferences-method.md)、 [objectsallocatedbyclass](icorprofilercallback-objectsallocatedbyclass-method.md)、 [rootreferences](icorprofilercallback-rootreferences-method.md)、 [RootReferences2](icorprofilercallback2-rootreferences2-method.md)、 [HandleCreated](icorprofilercallback2-handlecreated-method.md)、 [handledestroyed](icorprofilercallback2-handledestroyed-method.md)、および[finalizeableobjectqueued](icorprofilercallback2-finalizeableobjectqueued-method.md)コールバックを制御します。 `COR_PRF_MONITOR_GC` が割り当てられている場合、同時実行ガベージコレクションは無効になります。|  
|`COR_PRF_MONITOR_JIT_COMPILATION`|[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイス内の `JITCompilation*`、 [JITFunctionPitched](icorprofilercallback-jitfunctionpitched-method.md)、 [JITInlining](icorprofilercallback-jitinlining-method.md)の各コールバックを制御します。|  
|`COR_PRF_MONITOR_MODULE_LOADS`|[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスの `ModuleLoad*`、`ModuleUnload*`、および[ModuleAttachedToAssembly](icorprofilercallback-moduleattachedtoassembly-method.md)コールバックを制御します。|  
|`COR_PRF_MONITOR_OBJECT_ALLOCATED`|[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスの[ObjectAllocated](icorprofilercallback-objectallocated-method.md)コールバックを制御します。|  
|`COR_PRF_MONITOR_REMOTING`|[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスのコールバック `Remoting*` を制御します。|  
|`COR_PRF_MONITOR_REMOTING_ASYNC`|`Remoting*` コールバックが非同期イベントを監視するかどうかを制御します。|  
|`COR_PRF_MONITOR_REMOTING_COOKIE`|クッキーが `Remoting*` コールバックに渡されるかどうかを制御します。|  
|`COR_PRF_MONITOR_SUSPENDS`|[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスの `RuntimeSuspend*`、`RuntimeResume*`、 [runtimethreadsuspended](icorprofilercallback-runtimethreadsuspended-method.md)、および[runtimethreadsuspended](icorprofilercallback-runtimethreadresumed-method.md)コールバックを制御します。|  
|`COR_PRF_MONITOR_THREADS`|[ICorProfilerCallback](icorprofilercallback-interface.md)および[ICorProfilerCallback2](icorprofilercallback2-interface.md)インターフェイスの[threadcreated](icorprofilercallback-threadcreated-method.md)、 [threadcreated](icorprofilercallback-threadassignedtoosthread-method.md)、および[threadnamechanged](icorprofilercallback2-threadnamechanged-method.md)コールバックを制御します。 [ICorProfilerCallback2](icorprofilercallback-threaddestroyed-method.md)|  
  
<a name="Feature"></a>   
### <a name="feature-enabling-flags"></a>機能の有効化フラグ  
  
|メンバー|説明|  
|------------|-----------------|  
|`COR_PRF_ENABLE_FRAME_INFO`|[FunctionEnter2](functionenter2-function.md)コールバックによって返された `COR_PRF_FRAME_INFO` 値を使用して[GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md)メソッドを呼び出すことによって、ジェネリック関数の正確な `ClassID` を取得できるようにします。|  
|`COR_PRF_ENABLE_FUNCTION_ARGS`|[FunctionEnter2](functionenter2-function.md) callback または[FunctionEnter3WithInfo](functionenter3withinfo-function.md) callback と[GetFunctionEnter3Info](icorprofilerinfo3-getfunctionenter3info-method.md)メソッドを使用して、引数のトレースを有効にします。|  
|`COR_PRF_ENABLE_FUNCTION_RETVAL`|[FunctionLeave2](functionleave2-function.md) callback または[FunctionLeave3WithInfo](functionleave3withinfo-function.md) callback および[GetFunctionLeave3Info](icorprofilerinfo3-getfunctionleave3info-method.md)メソッドを使用して、戻り値のトレースを有効にします。|  
|`COR_PRF_ENABLE_INPROC_DEBUGGING`|非推奨。<br /><br /> プロセス中のデバッグはサポートされていません。 このフラグは無効です。|  
|`COR_PRF_ENABLE_JIT_MAPS`|非推奨。<br /><br /> [GetILToNativeMapping](icorprofilerinfo-getiltonativemapping-method.md)を使用して、プロファイラーが IL からネイティブへのマップを取得できるようにします。 .NET Framework 2.0 以降、ランタイムは常に IL-to-native マップを追跡するため、このフラグは常に設定されていると見なされます。|  
|`COR_PRF_ENABLE_OBJECT_ALLOCATED`|プロファイラーがオブジェクトの割り当て通知を必要とする可能性があることをランタイムに通知します。 このフラグは、初期化中に設定する必要があります。 これにより、プロファイラーは、後で `COR_PRF_MONITOR_OBJECT_ALLOCATED` フラグを使用して[ObjectAllocated](icorprofilercallback-objectallocated-method.md)コールバックを受け取ることができます。|  
|`COR_PRF_ENABLE_REJIT`|[RequestReJIT](icorprofilerinfo4-requestrejit-method.md)メソッドと[RequestRevert](icorprofilerinfo4-requestrevert-method.md)メソッドの呼び出しを有効にします。 プロファイラーは起動時にこのフラグを設定する必要があります。  プロファイラーがこのフラグを設定する場合は、`COR_PRF_DISABLE_ALL_NGEN_IMAGES` も指定する必要があります。|  
|`COR_PRF_ENABLE_STACK_SNAPSHOT`|[DoStackSnapshot](icorprofilerinfo2-dostacksnapshot-method.md)メソッドの呼び出しを有効にします。|  
  
<a name="Config"></a>   
### <a name="configuration-flags"></a>構成フラグ  
  
|メンバー|説明|  
|------------|-----------------|  
|`COR_PRF_DISABLE_ALL_NGEN_IMAGES`|(プロファイラーが拡張したイメージも含めて) すべてのネイティブ イメージがロードされないようにします。  このフラグと `COR_PRF_USE_PROFILE_IMAGES` フラグが両方指定されている場合は、`COR_PRF_DISABLE_ALL_NGEN_IMAGES` が使用されます。|  
|`COR_PRF_DISABLE_INLINING`|すべてのインライン展開を無効にします。|  
|`COR_PRF_DISABLE_OPTIMIZATIONS`|すべてのコードの最適化を無効にします。|  
|`COR_PRF_DISABLE_TRANSPARENCY_CHECKS_UNDER_FULL_TRUST`|セキュリティの透過性チェックを無効にします。このチェックは通常 just-in-time (JIT) コンパイル中に行われ、完全に信頼されているアセンブリではクラスのロード中に行われます。 これにより、いくつかのインストルメンテーションの実行が容易になります。|  
|`COR_PRF_USE_PROFILE_IMAGES`|ネイティブ イメージを検索して、プロファイラーが拡張したイメージを見つけます。 特定のアセンブリでプロファイラーが拡張したイメージが見つからなかった場合、共通言語ランタイムはそのアセンブリの JIT に戻ります。 このフラグと `COR_PRF_DISABLE_ALL_NGEN_IMAGES` フラグが両方指定されている場合は、`COR_PRF_DISABLE_ALL_NGEN_IMAGES` が使用されます。|  
  
<a name="Composite"></a>   
### <a name="composite-flags"></a>複合フラグ  
  
|メンバー|説明|  
|------------|-----------------|  
|`COR_PRF_ALL`|`COR_PRF_MONITOR` のすべてのフラグ値を表します。|  
|`COR_PRF_ALLOWABLE_AFTER_ATTACH`|プロファイラーが実行中のアプリケーションに割り当てられた後に設定することが可能な、`COR_PRF_MONITOR` のすべてのフラグを表します。 構文セクションは、このビットマスク内に存在する個々のフラグを示します。|  
|`COR_PRF_MONITOR_ALL`|すべてのコールバック イベントを有効にします。|  
|`COR_PRF_MONITOR_IMMUTABLE`|初期化中にのみ設定可能な、`COR_PRF_MONITOR` のすべてのフラグを表します。 初期化の後にこれらのいずれかのフラグを変更しようとすると、処理が失敗したことを示す `HRESULT` 値が返されます。|  
|`COR_PRF_REQUIRE_PROFILE_IMAGE`|プロファイルが強化されたイメージを必要とするすべての `COR_PRF_MONITOR` フラグを表しています。|  
  
## <a name="remarks"></a>Remarks  
 [ICorProfilerInfo:: geteventmask](icorprofilerinfo-geteventmask-method.md)メソッドと[ICorProfilerInfo:: seteventmask](icorprofilerinfo-seteventmask-method.md)メソッドで `COR_PRF_MONITOR` 値を使用して、共通言語ランタイムがプロファイラーに対して行うイベント通知を定義します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [列挙型のプロファイリング](profiling-enumerations.md)
- [GetEventMask メソッド](icorprofilerinfo-geteventmask-method.md)
- [SetEventMask メソッド](icorprofilerinfo-seteventmask-method.md)
