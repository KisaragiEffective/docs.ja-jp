---
title: ICorProfilerCallback2::RootReferences2 メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback2.RootReferences2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback2::RootReferences2
helpviewer_keywords:
- RootReferences2 method [.NET Framework profiling]
- ICorProfilerCallback2::RootReferences2 method [.NET Framework profiling]
ms.assetid: 55a2f907-d216-42eb-8f2f-e5d59c2eebd6
topic_type:
- apiref
ms.openlocfilehash: a9ce9a7a56847efcadf09924ffc56c41f20a1c58
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76865728"
---
# <a name="icorprofilercallback2rootreferences2-method"></a>ICorProfilerCallback2::RootReferences2 メソッド
ガベージコレクションが発生した後のルート参照をプロファイラーに通知します。 このメソッドは、 [ICorProfilerCallback:: RootReferences](icorprofilercallback-rootreferences-method.md)メソッドの拡張です。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT RootReferences2(  
    [in] ULONG  cRootRefs,  
    [in, size_is(cRootRefs)] ObjectID rootRefIds[],  
    [in, size_is(cRootRefs)] COR_PRF_GC_ROOT_KIND rootKinds[],  
    [in, size_is(cRootRefs)] COR_PRF_GC_ROOT_FLAGS rootFlags[],  
    [in, size_is(cRootRefs)] UINT_PTR rootIds[]);  
```  
  
## <a name="parameters"></a>パラメーター  
 `cRootRefs`  
 から`rootRefIds`、`rootKinds`、`rootFlags`、および `rootIds` の各配列内の要素の数。  
  
 `rootRefIds`  
 からオブジェクト Id の配列。それぞれが静的オブジェクトまたはスタック上のオブジェクトを参照します。 `rootKinds` 配列内の要素は、`rootRefIds` 配列内の対応する要素を分類するための情報を提供します。  
  
 `rootKinds`  
 からガベージコレクションのルートの型を示す[COR_PRF_GC_ROOT_KIND](cor-prf-gc-root-kind-enumeration.md)値の配列。  
  
 `rootFlags`  
 からガベージコレクションのルートのプロパティを記述する[COR_PRF_GC_ROOT_FLAGS](cor-prf-gc-root-flags-enumeration.md)値の配列。  
  
 `rootIds`  
 から`rootKinds` パラメーターの値に応じて、ガベージコレクションのルートに関する追加情報を格納している整数を指す UINT_PTR 値の配列。  
  
 ルートの型がスタックの場合、ルート ID は変数を含む関数を示します。 そのルート ID が0の場合、関数は CLR の内部にある名前のない関数です。 ルートの型がハンドルの場合、ルート ID はガベージコレクションハンドル用です。 その他のルート型では、ID は不透明な値であるため、無視する必要があります。  
  
## <a name="remarks"></a>コメント  
 `rootRefIds`、`rootKinds`、`rootFlags`、および `rootIds` 配列は並列配列です。 つまり、`rootRefIds[i]`、`rootKinds[i]`、`rootFlags[i]`、および `rootIds[i]` はすべて同じルートに関係しています。  
  
 `RootReferences` と `RootReferences2` の両方が、プロファイラーに通知するために呼び出されます。 通常、プロファイラーは1つのメソッドを実装しますが、両方は実装しません。 `RootReferences2` 渡される情報は `RootReferences`で渡されるのスーパーセットであるためです。  
  
 `rootRefIds` のエントリをゼロにすることができます。これは、対応するルート参照が null で、マネージヒープ上のオブジェクトを参照しないことを意味します。  
  
 `RootReferences2` によって返されるオブジェクト Id は、コールバック自体では無効です。これは、ガベージコレクションが古いアドレスから新しいアドレスにオブジェクトを移動する途中である可能性があるためです。 このため、`RootReferences2` 呼び出しの間、プロファイラーはオブジェクトを検査するべきではありません。 [ICorProfilerCallback2:: GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md)が呼び出されると、すべてのオブジェクトが新しい場所に移動され、安全に検査できるようになります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
- [ICorProfilerCallback2 インターフェイス](icorprofilercallback2-interface.md)
