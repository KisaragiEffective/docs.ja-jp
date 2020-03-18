---
title: 'いい変数 Home:: GetArgumentIndex メソッド'
ms.date: 03/30/2017
api_name:
- ICorDebugVariableHome.GetArgumentIndex
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugVariableHome::GetArgumentIndex
helpviewer_keywords:
- ICorDebugVariableHome::GetArgumentIndex method [.NET Framework debugging]
- GetArgumentIndex method, ICorDebugVariableHome interface [.NET Framework debugging]
ms.assetid: e86fcc72-388d-4009-ab21-8f9c3323e9a3
topic_type:
- apiref
ms.openlocfilehash: 12d4e63480f03bfad613f30362ddaeaf12b57a88
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791047"
---
# <a name="icordebugvariablehomegetargumentindex-method"></a>いい変数 Home:: GetArgumentIndex メソッド

関数の引数のインデックスを取得します。

## <a name="syntax"></a>構文

```cpp
HRESULT GetArgumentIndex(
    [out] ULONG32* pArgumentIndex
);
```

## <a name="parameters"></a>パラメーター

`pArgumentIndex`\
入出力引数インデックスへのポインター。

## <a name="return-value"></a>戻り値

メソッドは、次の値を返します。

|Value|説明|
|-----------|-----------------|
|`S_OK`|メソッド呼び出しによって有効な引数インデックスが返されました。|
|`E_FAIL`|現在の[ページ](icordebugvariablehome-interface.md)は、ローカル変数を表します。|

## <a name="remarks"></a>コメント

引数インデックスは、この引数のメタデータを取得するために使用できます。

## <a name="requirements"></a>要件

**:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。

**ヘッダー:** CorDebug.idl、CorDebug.h

**ライブラリ:** CorGuids.lib

**.NET Framework のバージョン:** [!INCLUDE[net_current_v462plus](../../../../includes/net-current-v462plus-md.md)]

## <a name="see-also"></a>関連項目

- [ICorDebugVariableHome インターフェイス](icordebugvariablehome-interface.md)
