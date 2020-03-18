---
title: EnumImportTypes メソッド
ms.date: 03/30/2017
api_name:
- EnumImportTypes
- IALink.EnumImportTypes
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- EnumImportTypes
helpviewer_keywords:
- EnumImportTypes method
ms.assetid: 83a0e4e7-ec06-40cb-9b63-700b9695bb04
topic_type:
- apiref
ms.openlocfilehash: ca7c7570aff63aa328dddc0626648fa74397addc
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74448736"
---
# <a name="enumimporttypes-method"></a>EnumImportTypes メソッド

各スコープ内の各型を列挙します。

## <a name="syntax"></a>構文

```cpp
HRESULT EnumImportTypes(
    HALINKENUM   hEnum,
    DWORD        dwMax,
    mdTypeDef    aTypeDefs[],
    DWORD*       pdwCount
) PURE;
```

## <a name="parameters"></a>パラメーター

`hEnum`\
列挙子のハンドル。

`dwMax`\
取得する型の最大数。

`aTypeDefs`\
`dwMax`を超えないように、型トークンを受け取ります。

`pdwCount`\
`aTypeDefs`内の実際の型の数を受け取ります。

## <a name="return-value"></a>戻り値

メソッドが成功した場合は S_OK を返します。

## <a name="requirements"></a>要件

Alink. h が必要です。

## <a name="see-also"></a>関連項目

- [IALink インターフェイス](ialink-interface.md)
- [IALink2 インターフェイス](ialink2-interface.md)
- [ALink API](index.md)
