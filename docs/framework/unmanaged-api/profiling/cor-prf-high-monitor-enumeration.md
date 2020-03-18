---
title: COR_PRF_HIGH_MONITOR 列挙型
ms.date: 04/10/2018
ms.assetid: 3ba543d8-15e5-4322-b6e7-1ebfc92ed7dd
ms.openlocfilehash: 72324fd434f3f37f723988345514c8aaada07ca9
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76867153"
---
# <a name="cor_prf_high_monitor-enumeration"></a>COR_PRF_HIGH_MONITOR 列挙型

[.NET Framework 4.5.2 以降のバージョンでのみでサポート]  
  
プロファイラーが読み込み時に[ICorProfilerInfo5:: SetEventMask2](icorprofilerinfo5-seteventmask2-method.md)メソッドに対して指定できる、 [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md)列挙に含まれるフラグだけでなく、フラグも提供します。  
  
## <a name="syntax"></a>構文  
  
```cpp
typedef enum {  
    COR_PRF_HIGH_MONITOR_NONE                     = 0x00000000,  
    COR_PRF_HIGH_ADD_ASSEMBLY_REFERENCES          = 0x00000001,  
    COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED        = 0x00000002,
    COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS = 0x00000004,
    COR_PRF_HIGH_DISABLE_TIERED_COMPILATION       = 0x00000008,
    COR_PRF_HIGH_BASIC_GC                         = 0x00000010,
    COR_PRF_HIGH_MONITOR_GC_MOVED_OBJECTS         = 0x00000020,
    COR_PRF_HIGH_MONITOR_LARGEOBJECT_ALLOCATED    = 0x00000040,
    COR_PRF_HIGH_REQUIRE_PROFILE_IMAGE            = 0,  
    COR_PRF_HIGH_ALLOWABLE_AFTER_ATTACH           = COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED | 
                                                    COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS |
                                                    COR_PRF_HIGH_BASIC_GC |
                                                    COR_PRF_HIGH_MONITOR_GC_MOVED_OBJECTS |
                                                    COR_PRF_HIGH_MONITOR_LARGEOBJECT_ALLOCATED,  
    COR_PRF_HIGH_MONITOR_IMMUTABLE                = COR_PRF_HIGH_DISABLE_TIERED_COMPILATION  
} COR_PRF_HIGH_MONITOR;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`COR_PRF_HIGH_MONITOR_NONE`|フラグが設定されていません。|  
|`COR_PRF_HIGH_ADD_ASSEMBLY_REFERENCES`|CLR アセンブリ参照クロージャウォーク中にアセンブリ参照を追加するための[ICorProfilerCallback6:: GetAssemblyReference](icorprofilercallback6-getassemblyreferences-method.md)コールバックを制御します。|  
|`COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED`|メモリ内モジュールに関連付けられているシンボルストリームに更新するための[ICorProfilerCallback7:: Moduleinmemorysymbol supcallback](icorprofilercallback7-moduleinmemorysymbolsupdated-method.md)を制御します。|  
|`COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS`|動的メソッドがガベージコレクションおよびアンロードされたことを示す[ICorProfilerCallback9::D ynamicmethodunloaded](icorprofilercallback9-dynamicmethodunloaded-method.md)コールバックを制御します。 <br/> [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]|
|`COR_PRF_HIGH_DISABLE_TIERED_COMPILATION`|.NET Core 3.0 以降のバージョンのみ: プロファイラーの階層化された[コンパイル](../../../core/whats-new/dotnet-core-3-0.md)を無効にします。|
|`COR_PRF_HIGH_BASIC_GC`|.NET Core 3.0 以降のバージョンのみ: [`COR_PRF_MONITOR_GC`](cor-prf-monitor-enumeration.md)と比較して、軽量の GC プロファイルオプションを提供します。 [GarbageCollectionStarted](icorprofilercallback2-garbagecollectionstarted-method.md)、 [GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md)、および[GetGenerationBounds](icorprofilerinfo2-getgenerationbounds-method.md)の各コールバックのみを制御します。 `COR_PRF_MONITOR_GC` フラグとは異なり、`COR_PRF_HIGH_BASIC_GC` は同時実行ガベージコレクションを無効にしません。|
|`COR_PRF_HIGH_MONITOR_GC_MOVED_OBJECTS`|.NET Core 3.0 以降のバージョンのみ: Gc を圧縮するために、 [Movedreferences](icorprofilercallback-movedreferences-method.md)と[MovedReferences2](icorprofilercallback4-movedreferences2-method.md)コールバックを有効にします。|
|`COR_PRF_HIGH_MONITOR_LARGEOBJECT_ALLOCATED`|.NET Core 3.0 以降のバージョンのみ: [`COR_PRF_MONITOR_OBJECT_ALLOCATED`](cor-prf-monitor-enumeration.md)に似ていますが、ラージオブジェクトヒープ (LOH) に対してのみオブジェクト割り当てに関する情報を提供します。|
|`COR_PRF_HIGH_REQUIRE_PROFILE_IMAGE`|プロファイルが強化されたイメージを必要とするすべての `COR_PRF_HIGH_MONITOR` フラグを表しています。 これは、 [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md)列挙の `COR_PRF_REQUIRE_PROFILE_IMAGE` フラグに対応します。|  
|`COR_PRF_HIGH_ALLOWABLE_AFTER_ATTACH`|プロファイラーが実行中のアプリケーションに割り当てられた後に設定することが可能な、`COR_PRF_HIGH_MONITOR` のすべてのフラグを表します。|  
|`COR_PRF_HIGH_MONITOR_IMMUTABLE`|初期化中にのみ設定可能な、`COR_PRF_HIGH_MONITOR` のすべてのフラグを表します。 他の場所でこれらのフラグを変更しようとすると、エラーを表す `HRESULT` 値が生じます。|  
  
## <a name="remarks"></a>コメント

`COR_PRF_HIGH_MONITOR` フラグは、 [ICorProfilerInfo5:: GetEventMask2](icorprofilerinfo5-geteventmask2-method.md)メソッドと[ICorProfilerInfo5:: SetEventMask2](icorprofilerinfo5-seteventmask2-method.md)メソッドの `pdwEventsHigh` パラメーターと共に使用されます。  
  
.NET Framework 4.6.1 以降では、`COR_PRF_HIGH_ALLOWABLE_AFTER_ATTACH` の値が0から `COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED` (0x00000002) に変更されました。 .NET Framework 4.7.2 以降では、その値が `COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED` から `COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED | COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS`に変更されました。   

`COR_PRF_HIGH_MONITOR_IMMUTABLE` は、初期化中にのみ設定できるすべてのフラグを表すビットマスクとして使用することを目的としています。 これらのフラグを他の場所で変更しようとすると、失敗した `HRESULT`になります。

## <a name="requirements"></a>要件

**:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
**ヘッダー** : CorProf.idl、CorProf.h  
  
**ライブラリ:** CorGuids.lib  
  
**.NET Framework のバージョン:** [!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [列挙型のプロファイリング](profiling-enumerations.md)
- [COR_PRF_MONITOR 列挙型](cor-prf-monitor-enumeration.md)
- [ICorProfilerInfo5 インターフェイス](icorprofilerinfo5-interface.md)
