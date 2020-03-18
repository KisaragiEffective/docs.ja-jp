---
title: Init メソッド
ms.date: 03/30/2017
api_name:
- IALink.Init
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- Init
helpviewer_keywords:
- Init method
ms.assetid: e48b5c64-049f-4f93-ad87-d07ae9cd5845
topic_type:
- apiref
ms.openlocfilehash: 986ae69e7ebb8f607be5d37fab426bcc787abb26
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74445640"
---
# <a name="init-method"></a>Init メソッド
使用する[Ialink インターフェイス](ialink-interface.md)を実装するオブジェクトを準備します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT Init(  
    IMetaDataDispenserEx* pDispenser,  
    IMetaDataError* pErrorHandler  
) PURE;  
```  
  
## <a name="parameters"></a>パラメーター  
 `pDispenser`  
 [IMetaDataDispenserEx インターフェイス](../metadata/imetadatadispenserex-interface.md)のメタデータディスペンサーへのポインター。  
  
 `pErrorHandler`  
 [IMetaDataError インターフェイス](../metadata/imetadataerror-interface.md)は、省略可能なエラー処理インターフェイスへのポインターです。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK を返します。  
  
## <a name="requirements"></a>要件  
 Alink. h が必要です。  
  
## <a name="see-also"></a>参照

- [IALink インターフェイス](ialink-interface.md)
- [IALink2 インターフェイス](ialink2-interface.md)
- [ALink API](index.md)
