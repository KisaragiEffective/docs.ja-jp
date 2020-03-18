---
title: EnumCustomAttributes メソッド
ms.date: 03/30/2017
api_name:
- EnumCustomAttributes
- IALink.EnumCustomAttributes
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- EnumCustomAttributes
helpviewer_keywords:
- EnumCustomAttributes method
ms.assetid: 08dff60c-f01b-4050-8865-ea3f95361c9f
topic_type:
- apiref
ms.openlocfilehash: 6a5b3f1e9bf1444feb73949ef7133fbd9ae35134
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74446472"
---
# <a name="enumcustomattributes-method"></a>EnumCustomAttributes メソッド
アセンブリレベルのカスタム属性を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EnumCustomAttributes(  
    HALINKENUM hEnum,  
    mdToken tkType,  
    mdCustomAttribute rCustomValues[],  
    ULONG cMax,  
    ULONG* pcCustomValues  
) PURE;  
```  
  
## <a name="parameters"></a>パラメーター  
 `hEnum`  
 列挙子のハンドル。  
  
 `tkType`  
 列挙する属性の型。 すべての属性には `mdTokenNill` を使用します。  
  
 `rCustomValues`  
 カスタム属性トークンを受け取ります。  
  
 `cMax`  
 `rCustomValues` 配列のサイズを指定します。  
  
 `pcCustomValues`  
 必要に応じて、トークンの値の数を受け取ります。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK を返します。  
  
## <a name="requirements"></a>要件  
 Alink. h が必要です。  
  
## <a name="see-also"></a>参照

- [IALink インターフェイス](ialink-interface.md)
- [IALink2 インターフェイス](ialink2-interface.md)
- [ALink API](index.md)
