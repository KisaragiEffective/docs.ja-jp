---
title: IMethodMalloc::Alloc メソッド
ms.date: 03/30/2017
api_name:
- IMethodMalloc.Alloc
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- IMethodMalloc::Alloc
helpviewer_keywords:
- IMethodMalloc::Alloc method [.NET Framework profiling]
- Alloc method, IMethodMalloc interface [.NET Framework profiling]
ms.assetid: 8653bd4c-2290-43d2-a3e1-cbbd50033f4f
topic_type:
- apiref
ms.openlocfilehash: af881d23ff77f05dadbbc745b973979e35ebe9f7
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74447570"
---
# <a name="imethodmallocalloc-method"></a>IMethodMalloc::Alloc メソッド

新しい Microsoft 中間言語 (MSIL) 関数本体に対して、指定された量のメモリを割り当てようとします。

## <a name="syntax"></a>構文

```cpp
PVOID Alloc (
    [in] ULONG   cb
);
```

## <a name="parameters"></a>パラメーター

`cb`\
からメソッド本体に割り当てるバイト数。

## <a name="remarks"></a>コメント

 割り当てられたメモリは、このアロケーターに関連付けられているモジュールのベースアドレスよりも大きいアドレスから開始されます。 つまり、各アロケーターは特定のモジュールに対して作成され、そのベースアドレスからの正のオフセットでメモリの割り当てが試行されます。 `Alloc` が、モジュールのベースアドレスよりも大きいアドレスで、要求されたバイト数を割り当てることができなかった場合は、使用可能なメモリ領域の実際の量に関係なく、E_OUTOFMEMORY を返します。

 `Alloc` メソッドは、 [ICorProfilerInfo:: SetILFunctionBody](icorprofilerinfo-setilfunctionbody-method.md)メソッドと組み合わせて使用する必要があります。

## <a name="requirements"></a>要件
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。

 **ヘッダー** : CorProf.idl、CorProf.h

 **ライブラリ:** CorGuids.lib

 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]

## <a name="see-also"></a>参照

- [IMethodMalloc インターフェイス](imethodmalloc-interface.md)
