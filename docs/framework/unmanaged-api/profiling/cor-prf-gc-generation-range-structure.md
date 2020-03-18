---
title: COR_PRF_GC_GENERATION_RANGE 構造体
ms.date: 03/30/2017
api_name:
- COR_PRF_GC_GENERATION_RANGE
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_GC_GENERATION_RANGE
helpviewer_keywords:
- COR_PRF_GC_GENERATION_RANGE structure [.NET Framework profiling]
ms.assetid: e7e07273-8d10-4a68-807e-59634e3f8c5e
topic_type:
- apiref
ms.openlocfilehash: 682aac9729519e0861ae6e6f60df78a30063c278
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76867212"
---
# <a name="cor_prf_gc_generation_range-structure"></a>COR_PRF_GC_GENERATION_RANGE 構造体
ガーベッジ コレクションを実行中のメモリ範囲 (ブロック) を説明します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef struct COR_PRF_GC_GENERATION_RANGE {  
    COR_PRF_GC_GENERATION generation;  
    ObjectID rangeStart;  
    UINT_PTR rangeLength;  
    UINT_PTR rangeLengthReserved;  
} COR_PRF_GC_GENERATION_RANGE;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`generation`|メモリブロックが属するジェネレーションを指定する[COR_PRF_GC_GENERATION](cor-prf-gc-generation-enumeration.md)列挙体の値。|  
|`rangeStart`|メモリブロックの開始位置を指定するオブジェクトの ID。|  
|`rangeLength`|メモリブロックの使用されている部分のサイズ (つまり、ブロック内で使用されているメモリの量) を指定する整数へのポインター。|  
|`rangeLengthReserved`|メモリブロックのサイズ (つまり、ブロックに予約されているメモリの量) を指定する整数へのポインター。|  
  
## <a name="remarks"></a>コメント  
 `rangeLength` 値が正確であることが保証されるのは、 [ICorProfilerInfo2:: GetGenerationBounds](icorprofilerinfo2-getgenerationbounds-method.md)または[ICorProfilerInfo2:: getobjectgeneration](icorprofilerinfo2-getobjectgeneration-method.md)(両方が `COR_PRF_GC_GENERATION_RANGE` 構造体を使用している場合) が[ICorProfilerCallback2:: GarbageCollectionStarted](icorprofilercallback2-garbagecollectionstarted-method.md)メソッドまたは[ICorProfilerCallback2:: GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md)メソッドから呼び出された場合だけです。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Corprof.idl  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [構造体のプロファイリング](profiling-structures.md)
