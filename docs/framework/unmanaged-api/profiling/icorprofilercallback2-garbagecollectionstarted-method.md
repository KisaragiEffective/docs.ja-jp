---
title: ICorProfilerCallback2::GarbageCollectionStarted メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback2.GarbageCollectionStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback2::GarbageCollectionStarted
helpviewer_keywords:
- GarbageCollectionStarted method [.NET Framework profiling]
- ICorProfilerCallback2::GarbageCollectionStarted method [.NET Framework profiling]
ms.assetid: 44eef087-f21f-4fe2-b481-f8a0ee022e7d
topic_type:
- apiref
ms.openlocfilehash: c90c790c519cc0c422657e6e2d8040a365fbf48c
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76865780"
---
# <a name="icorprofilercallback2garbagecollectionstarted-method"></a>ICorProfilerCallback2::GarbageCollectionStarted メソッド
ガベージコレクションが開始されたことをコードプロファイラーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GarbageCollectionStarted(  
    [in] int cGenerations,  
    [in, size_is(cGenerations), length_is(cGenerations)] BOOL generationCollected[],  
    [in] COR_PRF_GC_REASON reason);  
```  
  
## <a name="parameters"></a>パラメーター  
 `cGenerations`  
 から`generationCollected` 配列内のエントリの合計数。  
  
 `generationCollected`  
 からブール値の配列。配列インデックスに対応する世代がこのガベージコレクションによって収集されている場合に `true` ます。それ以外の場合は、`false`ます。  
  
 配列は、生成を示す[COR_PRF_GC_GENERATION](cor-prf-gc-generation-enumeration.md)列挙体の値によってインデックスが作成されます。  
  
 `reason`  
 からガベージコレクションが発生した理由を示す[COR_PRF_GC_REASON](cor-prf-gc-reason-enumeration.md)列挙体の値。  
  
## <a name="remarks"></a>コメント  
 このガベージコレクションに関連するすべてのコールバックは、`GarbageCollectionStarted` コールバックと、対応する[ICorProfilerCallback2:: GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md)コールバックの間で発生します。 これらのコールバックは、同じスレッドでは実行されません。  
  
 `GarbageCollectionStarted` コールバック中に、プロファイラーが元の場所のオブジェクトを検査するのは安全です。 `GarbageCollectionStarted`から戻った後、ガベージコレクターがオブジェクトの移動を開始します。 プロファイラーは、このコールバックから返された後、`ICorProfilerCallback2::GarbageCollectionFinished` コールバックを受け取るまで、すべてのオブジェクト Id が無効であると見なす必要があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
- [ICorProfilerCallback2 インターフェイス](icorprofilercallback2-interface.md)
