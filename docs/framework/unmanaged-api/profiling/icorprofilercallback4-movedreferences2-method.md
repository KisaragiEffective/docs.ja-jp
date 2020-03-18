---
title: ICorProfilerCallback4::MovedReferences2 メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback4.MovedReferences2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback4::MovedReferences2
helpviewer_keywords:
- MovedReferences2 method, ICorProfilerCallback4 interface [.NET Framework profiling]
- ICorProfilerCallback4::MovedReferences2 method [.NET Framework profiling]
ms.assetid: d17a065b-5bc6-4817-b3e1-1e413fcb33a8
topic_type:
- apiref
ms.openlocfilehash: 2f305852ae218417aa1f4d4fe9d2076c0163fd60
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76865273"
---
# <a name="icorprofilercallback4movedreferences2-method"></a>ICorProfilerCallback4::MovedReferences2 メソッド
圧縮ガベージ コレクションを実行した後の、ヒープ内のオブジェクトの新しいレイアウトを報告するために呼び出されます。 このメソッドは、プロファイラーが[ICorProfilerCallback4](icorprofilercallback4-interface.md)インターフェイスを実装した場合に呼び出されます。 このコールバックでは、 [ICorProfilerCallback:: MovedReferences](icorprofilercallback-movedreferences-method.md)メソッドが置き換えられます。これは、長さが ULONG で表現できる値を超えるオブジェクトの範囲をより多く報告できるためです。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT MovedReferences2(  
    [in]  ULONG  cMovedObjectIDRanges,  
    [in, size_is(cMovedObjectIDRanges)] ObjectID oldObjectIDRangeStart[] ,  
    [in, size_is(cMovedObjectIDRanges)] ObjectID newObjectIDRangeStart[] ,  
    [in, size_is(cMovedObjectIDRanges)] SIZE_T    cObjectIDRangeLength[] );  
```  
  
## <a name="parameters"></a>パラメーター  
 `cMovedObjectIDRanges`  
 [in] 圧縮ガベージ コレクションを実行した後に移動される、隣接したオブジェクトのブロック数。 つまり、`cMovedObjectIDRanges` の値は `oldObjectIDRangeStart`、`newObjectIDRangeStart`、および `cObjectIDRangeLength` 配列の合計サイズです。  
  
 `MovedReferences2` の次の 3 つの引数は並列配列です。 つまり、`oldObjectIDRangeStart[i]`、`newObjectIDRangeStart[i]`、`cObjectIDRangeLength[i]` はすべて、隣接するオブジェクトの同じブロックを対象としています。  
  
 `oldObjectIDRangeStart`  
 [in] それぞれがメモリ内の有効な隣接オブジェクト ブロックの古い (ガベージ コレクション実行前の) 開始アドレスを表す、`ObjectID` 値の配列。  
  
 `newObjectIDRangeStart`  
 [in] それぞれがメモリ内の有効な隣接オブジェクト ブロックの新しい (ガベージ コレクション実行後の) 開始アドレスを表す、`ObjectID` 値の配列。  
  
 `cObjectIDRangeLength`  
 [in] それぞれがメモリ内の隣接オブジェクト ブロックのサイズを表す、整数の配列。  
  
 サイズは、`oldObjectIDRangeStart` および `newObjectIDRangeStart` 配列内の参照される各ブロックに対して指定します。  
  
## <a name="remarks"></a>Remarks  
 圧縮ガベージ コレクターは、無効なオブジェクトによって占有されているメモリを解放し、解放された領域を圧縮します。 その結果、ヒープ内で有効なオブジェクトが移動され、以前の通知で配布された `ObjectID` の値が変更されることがあります。  
  
 既存の `ObjectID` の値 (`oldObjectID`) が次の範囲内にあるとします。  
  
 `oldObjectIDRangeStart[i]` <= `oldObjectID` < `oldObjectIDRangeStart[i]` + `cObjectIDRangeLength[i]`  
  
 この場合、範囲の開始からオブジェクトの開始までのオフセットは次のとおりです。  
  
 `oldObjectID` - `oldObjectRangeStart[i]`  
  
 `i` の値が次の範囲内にあるとします。  
  
 0 <= `i` < `cMovedObjectIDRanges`  
  
 この場合、新しい `ObjectID` は次のように計算できます。  
  
 `newObjectID` = `newObjectIDRangeStart[i]` + (`oldObjectID` – `oldObjectIDRangeStart[i]`)  
  
 ガーベッジ コレクションでオブジェクトが古い場所から新しい場所へ移動中の可能性があるため、コールバックの間は `MovedReferences2` によって渡される `ObjectID` 値はすべて無効です。 このため、`MovedReferences2` 呼び出しの間、プロファイラーはオブジェクトを検査するべきではありません。 [ICorProfilerCallback2:: GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md)コールバックは、すべてのオブジェクトが新しい場所に移動され、検査を実行できることを示します。  
  
 プロファイラーが[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスと[ICorProfilerCallback4](icorprofilercallback4-interface.md)インターフェイスの両方を実装している場合、`MovedReferences2` メソッドは、 [ICorProfilerCallback:: movedreferences](icorprofilercallback-movedreferences-method.md)メソッドの前に呼び出されますが、`MovedReferences2` メソッドが正常に返された場合にのみ呼び出されます。 プロファイラーは HRESULT を返すことがあり、これは `MovedReferences2` メソッドの失敗を示し、2 番目のメソッドが呼び出されません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
- [MovedReferences メソッド](icorprofilercallback-movedreferences-method.md)
- [ICorProfilerCallback4 インターフェイス](icorprofilercallback4-interface.md)
- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [プロファイル](index.md)
