---
description: '詳細情報: AddFile メソッド'
title: AddFile メソッド
ms.date: 03/30/2017
api_name:
- IALink.AddFile
- AddFile
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- AddFile
helpviewer_keywords:
- AddFile method
ms.assetid: 9e707abb-f905-4568-9356-12aa21d1b11c
topic_type:
- apiref
ms.openlocfilehash: 5e1253587298b2c1559c72dced43ec70dc169090
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99638627"
---
# <a name="addfile-method"></a>AddFile メソッド

アセンブリにファイルを追加します。 は、バインドされていないモジュールを作成するためにも使用できます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT AddFile(  
    mdAssembly      AssemblyID,  
    LPCWSTR         pszFilename,  
    DWORD           dwFlags,  
    IMetaDataEmit*  pEmitter,  
    mdFile*         pFileToken  
) PURE;  
```  
  
## <a name="parameters"></a>パラメーター  

 `AssemblyID`  
 補強するアセンブリの一意の ID。  
  
 `pszFilename`  
 追加するファイルの完全修飾名。  
  
 `dwFlags`  
 やなどの COM + FileDef フラグ `ffContainsNoMetaData` `ffWriteable` 。 `dwFlags` は、 [メソッド](../metadata/imetadataassemblyemit-definefile-method.md)に渡されます。  
  
 `pEmitter`  
 必要に応じて、メタデータを出力するために使用される[IMetaDataEmit インターフェイス](../metadata/imetadataemit-interface.md)インターフェイス。  
  
 `pFileToken`  
 追加されたファイルの一意の ID が格納される場所へのポインター。  
  
## <a name="return-value"></a>戻り値  

 メソッドが成功した場合は S_OK を返します。  
  
## <a name="requirements"></a>要件  

 Alink. h が必要です。  
  
## <a name="see-also"></a>関連項目

- [IALink インターフェイス](ialink-interface.md)
- [IALink2 インターフェイス](ialink2-interface.md)
- [ALink API](index.md)
