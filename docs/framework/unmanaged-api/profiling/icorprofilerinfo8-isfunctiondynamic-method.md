---
title: 'ICorProfilerInfo8:: IsFunctionDynamic'
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo8.IsFunctionDynamic
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: 50b4de2de3e74a5835ee5706999892735269d4c2
ms.sourcegitcommit: b11efd71c3d5ce3d9449c8d4345481b9f21392c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76861737"
---
# <a name="icorprofilerinfo8isfunctiondynamic-method"></a>ICorProfilerInfo8:: IsFunctionDynamic メソッド

関数にメタデータが関連付けられていないかどうかを判断します。

## <a name="syntax"></a>構文

```cpp
HRESULT IsFunctionDynamic( [in]  FunctionID  functionId,
                           [out] BOOL        *isDynamic);
```

## <a name="parameters"></a>パラメーター

- `functionId`

  \[] には、対象の関数を識別する `FunctionID` を指定します。

- `isDynamic`

  \[out] 関数にメタデータが含まれていないかどうかを示す値を格納する `BOOL` へのポインター。

## <a name="remarks"></a>コメント

関数は、メタデータがない場合は動的と見なされます。 IL スタブや LCG メソッドなどの特定のメソッドには、IMetaDataImport Api を使用して取得できるメタデータが関連付けられていません。 これらのメソッドは、命令ポインターを通じて、または[ICorProfilerCallback::D ynamicmethodjitcompilationstarted](icorprofilercallback8-dynamicmethodjitcompilationstarted-method.md)をリッスンすることによって、プロファイラーによって検出されます。

## <a name="requirements"></a>要件

**:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。

**ヘッダー** : CorProf.idl、CorProf.h

**ライブラリ:** CorGuids.lib

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [ICorProfilerInfo8 インターフェイス](icorprofilerinfo8-interface.md)
