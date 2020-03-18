---
title: ICorProfilerInfo9::GetCodeInfo4
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo9.GetCodeInfo4
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: f65cebff912adeb7afc34434467cf7be72f9be32
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77449765"
---
# <a name="icorprofilerinfo9getcodeinfo4-method"></a>ICorProfilerInfo9:: GetCodeInfo4 メソッド

ネイティブコードの開始アドレスを指定すると、このコードを格納する仮想メモリのブロックが返されます。

## <a name="syntax"></a>構文

```cpp
HRESULT GetCodeInfo4( [in]  UINT_PTR pNativeCodeStartAddress,
                      [in]  ULONG32 cCodeInfos,
                      [out] ULONG32* pcCodeInfos,
                      [out] COR_PRF_CODE_INFO codeInfos[]);
```

## <a name="parameters"></a>パラメーター

- `pNativeCodeStartAddress`

  \[] のネイティブ関数の先頭へのポインター。

- `cCodeInfos`

  \[] `codeInfos` 配列のサイズ。

- `pcCodeInfos`

  \[out] 使用可能な[COR_PRF_CODE_INFO](cor-prf-code-info-structure.md)構造の総数へのポインター。

- `codeInfos`

  \[呼び出し元によって指定されたバッファー。 メソッドから制御が戻った後で、それぞれがネイティブ コードのブロックを記述する `COR_PRF_CODE_INFO` の構造体の配列が含まれます。

## <a name="remarks"></a>コメント

`GetCodeInfo4` メソッドは[GetCodeInfo3](icorprofilerinfo4-getcodeinfo3-method.md)に似ていますが、メソッドのさまざまなネイティブバージョンのコード情報を検索できます。

> [!NOTE]
> `GetCodeInfo4` は、ガベージコレクションをトリガーできます。

エクステントは共通中間言語 (CIL) オフセットの昇順に並べ替えられます。

`GetCodeInfo4` が返された後、`codeInfos` バッファーがすべての[COR_PRF_CODE_INFO](cor-prf-code-info-structure.md)構造体を格納するのに十分な大きさであったことを確認する必要があります。 これを行うには、`cCodeInfos` の値を `cchName` パラメーターの値と比較します。 [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md)構造体のサイズで除算された `cCodeInfos` が `pcCodeInfos`よりも小さい場合は、より大きな `codeInfos` バッファーを割り当て、新しいサイズを持つ `cCodeInfos` を更新して `GetCodeInfo4` を再度呼び出します。

別の方法として、最初に `GetCodeInfo4` を長さゼロの `codeInfos` バッファーで呼び出して、適切なバッファーのサイズを取得します。 次に、`codeInfos` のバッファーサイズを `pcCodeInfos`で返される値に設定し、 [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md)構造体のサイズを乗算して、もう一度 `GetCodeInfo4` を呼び出します。

## <a name="requirements"></a>要件

**プラットフォーム:** 「 [.Net Core でサポートされるオペレーティングシステム](../../../core/install/dependencies.md?pivots=os-windows)」を参照してください。

**ヘッダー** : CorProf.idl、CorProf.h

**ライブラリ:** CorGuids.lib

**.Net のバージョン:** [!INCLUDE[net_core_22](../../../../includes/net-core-22-md.md)]

## <a name="see-also"></a>参照

- [ICorProfilerInfo9 インターフェイス](ICorProfilerInfo9-interface.md)
