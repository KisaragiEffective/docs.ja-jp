---
title: ICorProfilerInfo9::GetNativeCodeStartAddresses
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo9.GetNativeCodeStartAddresses
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: 99706fdc3d60a5e1a7f85400c1184d5acc808e42
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77449731"
---
# <a name="icorprofilerinfo9getnativecodestartaddresses-method"></a>ICorProfilerInfo9:: GetNativeCodeStartAddresses メソッド

指定された functionId と rejitId は、現在存在する、このコードのすべての just-in-time バージョンのネイティブコードの開始アドレスを列挙します。

## <a name="syntax"></a>構文

```cpp
HRESULT GetNativeCodeStartAddresses( [in]  FunctionID functionID,
                                     [in]  ReJITID reJitId,
                                     [in]  ULONG32 cCodeStartAddresses,
                                     [out] ULONG32 *pcCodeStartAddresses,
                                     [out] UINT_PTR codeStartAddresses[]);
```

## <a name="parameters"></a>パラメーター

- `functionId`

  \[] には、ネイティブコードの開始アドレスを返す関数の ID を指定します。

- `reJitId`

  \[] JIT 再コンパイルされた関数の id。

- `cCodeStartAddresses`

  \[] `codeStartAddresses` 配列の最大サイズ。

- `pcCodeStartAddresses`

  \[out] 使用可能なアドレスの数。

- `codeStartAddresses`

  \[out] `UINT_PTR`の配列。それぞれが指定された関数のネイティブ本体の開始アドレスです。

## <a name="remarks"></a>コメント

階層化コンパイルが有効になっている場合、関数は複数のネイティブコード本体を持つことができます。

## <a name="requirements"></a>要件

**プラットフォーム:** 「 [.Net Core でサポートされるオペレーティングシステム](../../../core/install/dependencies.md?pivots=os-windows)」を参照してください。

**ヘッダー** : CorProf.idl、CorProf.h

**ライブラリ:** CorGuids.lib

**.Net のバージョン:** [!INCLUDE[net_core_22](../../../../includes/net-core-22-md.md)]

## <a name="see-also"></a>参照

- [ICorProfilerInfo9 インターフェイス](icorprofilerinfo9-interface.md)
