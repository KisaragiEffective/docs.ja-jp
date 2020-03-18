---
title: ICorProfilerCallback4::SurvivingReferences2 メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback4.SurvivingReferences2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback4::SurvivingReferences2
helpviewer_keywords:
- ICorProfilerCallback4::SurvivingReferences2 method [.NET Framework profiling]
- SurvivingReferences2 method, ICorProfilerCallback4 interface [.NET Framework profiling]
ms.assetid: 02b51888-5d89-4e50-a915-45b7e329aad9
topic_type:
- apiref
ms.openlocfilehash: bec50183e6a8690cb02f3dc06d32b7449e055cea
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76865169"
---
# <a name="icorprofilercallback4survivingreferences2-method"></a>ICorProfilerCallback4::SurvivingReferences2 メソッド
非圧縮ガベージ コレクションを実行した後の、ヒープ内のオブジェクトのレイアウトを報告します。 このメソッドは、プロファイラーが[ICorProfilerCallback4](icorprofilercallback4-interface.md)インターフェイスを実装した場合に呼び出されます。 このコールバックでは、 [ICorProfilerCallback2:: SurvivingReferences](icorprofilercallback2-survivingreferences-method.md)メソッドが置き換えられます。これは、長さが ULONG で表現できる値を超えるオブジェクトの範囲を超える可能性があるためです。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SurvivingReferences2(  
    [in] ULONG  cSurvivingObjectIDRanges,  
    [in, size_is(cSurvivingObjectIDRanges)] ObjectID  
                objectIDRangeStart[] ,  
    [in, size_is(cSurvivingObjectIDRanges)] SIZE_T  
                cObjectIDRangeLength[] );  
```  
  
## <a name="parameters"></a>パラメーター  
 `cSurvivingObjectIDRanges`  
 [in] 非圧縮ガベージ コレクションを実行した後に存続する、隣接したオブジェクトのブロック数。 つまり、`cSurvivingObjectIDRanges` の値は、`objectIDRangeStart` 配列と `cObjectIDRangeLength` 配列のサイズを表します。これらの配列にはそれぞれ、オブジェクトの各ブロックの `ObjectID` と長さが格納されます。  
  
 `SurvivingReferences2` の次の2個の引数は並列配列です。 つまり、`objectIDRangeStart` と `cObjectIDRangeLength` は隣接するオブジェクトの同じブロックを対象としています。  
  
 `objectIDRangeStart`  
 [in] それぞれがメモリ内の有効な隣接オブジェクト ブロックの開始アドレスを表す、`ObjectID` 値の配列。  
  
 `cObjectIDRangeLength`  
 [in] それぞれがメモリ内に存続する隣接オブジェクト ブロックのサイズを表す、整数の配列。  
  
 サイズは、`objectIDRangeStart` 配列内の参照される各ブロックに対して指定します。  
  
## <a name="remarks"></a>Remarks  
 `objectIDRangeStart` 配列と `cObjectIDRangeLength` 配列の要素は、次のように解釈されて、ガベージ コレクションでオブジェクトが存続したかどうかを判断する必要があります。 `ObjectID` 値 (`ObjectID`) が次の範囲内にあるとします。  
  
 `ObjectIDRangeStart[i]` <= `ObjectID` < `ObjectIDRangeStart[i]` + `cObjectIDRangeLength[i]`  
  
 次の範囲内にある `i` のすべての値について、オブジェクトはガベージ コレクションの実行後に存続しています。  
  
 0 <= `i` < `cSurvivingObjectIDRanges`  
  
 非圧縮ガベージ コレクションは、"無効な" オブジェクトによって占有されているメモリをクリアしますが、解放された領域は圧縮しません。 そのため、メモリはヒープに返されますが、"有効な" オブジェクトは移動されません。  
  
 共通言語ランタイム (CLR: Common Language Runtime) は、非圧縮ガベージ コレクションに対して `SurvivingReferences2` を呼び出します。 ガベージコレクションを圧縮する場合は、代わりに[MovedReferences2](icorprofilercallback4-movedreferences2-method.md)が呼び出されます。 単一のガベージ コレクションで 1 つのジェネレーションを圧縮できますが、その他のジェネレーションは非圧縮になります。 特定のジェネレーションのガベージコレクションの場合、プロファイラーは `SurvivingReferences2` コールバックまたは[MovedReferences2](icorprofilercallback4-movedreferences2-method.md)コールバックのいずれかを受け取りますが、両方を受け取ることはありません。  
  
 特定のガベージ コレクションで複数の `SurvivingReferences2` コールバックを受け取ることがあります。この原因としては、内部バッファリングの制限、サーバーのガベージ コレクション中の複数のコールバックなどが考えられます。 あるガベージ コレクションで複数のコールバックが生じる場合、情報が累積されます。つまり、`SurvivingReferences2` コールバックで報告されるすべての参照は対象のガベージ コレクション後も存続します。  
  
 プロファイラーが[ICorProfilerCallback](icorprofilercallback-interface.md)インターフェイスと[ICorProfilerCallback4](icorprofilercallback4-interface.md)インターフェイスの両方を実装している場合、`SurvivingReferences2` メソッドは、 [ICorProfilerCallback2:: SurvivingReferences](icorprofilercallback2-survivingreferences-method.md)メソッドの前に呼び出されますが、`SurvivingReferences2` が正常に返された場合に限ります。 プロファイラーは HRESULT を返すことがあり、これは `SurvivingReferences2` メソッドの失敗を示し、2 番目のメソッドが呼び出されません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerCallback インターフェイス](icorprofilercallback-interface.md)
- [ICorProfilerCallback2 インターフェイス](icorprofilercallback2-interface.md)
- [ICorProfilerCallback4 インターフェイス](icorprofilercallback4-interface.md)
