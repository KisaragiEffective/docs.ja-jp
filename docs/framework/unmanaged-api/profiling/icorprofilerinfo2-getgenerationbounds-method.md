---
title: ICorProfilerInfo2::GetGenerationBounds メソッド
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetGenerationBounds
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetGenerationBounds
helpviewer_keywords:
- ICorProfilerInfo2::GetGenerationBounds method [.NET Framework profiling]
- GetGenerationBounds method [.NET Framework profiling]
ms.assetid: 9c37185f-d1e0-4a6e-8b99-707f7df61d88
topic_type:
- apiref
ms.openlocfilehash: 3cdc185408576f5679daacef4dde438d66e490ff
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76862751"
---
# <a name="icorprofilerinfo2getgenerationbounds-method"></a>ICorProfilerInfo2::GetGenerationBounds メソッド
各種ガベージ コレクション ジェネレーションを構成するメモリ領域 (ヒープのセグメント) を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetGenerationBounds(  
    [in]  ULONG cObjectRanges,  
    [out] ULONG *pcObjectRanges,  
    [out, size_is(cObjectRanges), length_is(*pcObjectRanges)] COR_PRF_GC_GENERATION_RANGE ranges[]);  
```  
  
## <a name="parameters"></a>パラメーター  
 `cObjectRanges`  
 [in] `ranges` 配列の呼び出し元によって割り当てられた要素の数。  
  
 `pcObjectRanges`  
 [out] その一部または全部が `ranges` 配列で返される範囲の総数を指定する整数へのポインター。  
  
 `ranges`  
 入出力[COR_PRF_GC_GENERATION_RANGE](cor-prf-gc-generation-range-structure.md)構造体の配列。それぞれがガベージコレクションを行っているジェネレーション内のメモリの範囲 (つまり、ブロック) を記述します。  
  
## <a name="remarks"></a>コメント  
 ガベージ コレクションを処理中でない場合、`GetGenerationBounds` メソッドは任意のプロファイラー コールバックから呼び出すことができます。

 通常、ジェネレーションの移動はガベージ コレクション中に行われます。 コレクションの間にジェネレーションが増大する可能性はありますが、一般的に移動はありません。 したがって、`GetGenerationBounds` を呼び出す上で最も注目すべき地点は `ICorProfilerCallback2::GarbageCollectionStarted` と `ICorProfilerCallback2::GarbageCollectionFinished` の間です。  
  
 プログラムの起動中に、いくつかのオブジェクトが共通言語ランタイム (CLR) 自体によって割り当てられます。これは、一般的にはジェネレーションの 3 と 0 で行われます。 したがって、マネージド コードが実行を開始するまでに、これらのジェネレーションには既にオブジェクトが含まれています。 通常、ジェネレーションの 1 と 2 は、ガベージ コレクターによって生成されたダミー オブジェクトを除き、空です。 (ダミーオブジェクトのサイズは、CLR の32ビット実装では12バイトです。64ビットの実装では、サイズは大きくなります)。また、ネイティブイメージジェネレーター (Ngen.exe) によって生成されたモジュール内にあるジェネレーション2の範囲が表示される場合もあります。 この場合、ジェネレーション2のオブジェクトは固定された*オブジェクト*であり、ガベージコレクターではなく ngen.exe が実行されるときに割り当てられます。  
  
 この関数は、呼び出し元が割り当てたバッファーを使用します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー** : CorProf.idl、CorProf.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorProfilerInfo インターフェイス](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 インターフェイス](icorprofilerinfo2-interface.md)
- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [プロファイル](index.md)
