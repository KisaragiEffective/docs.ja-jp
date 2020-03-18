---
title: AddFile2 メソッド
ms.date: 03/30/2017
api_name:
- AddFile2
- IALink2.AddFile2
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- AddFile2
helpviewer_keywords:
- AddFile2 method
ms.assetid: 03bc49bf-a89b-4fb6-a88d-97482e061195
topic_type:
- apiref
ms.openlocfilehash: 8dadf9ec8f896b03e4918b21f5153c1b747010fd
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74446666"
---
# <a name="addfile2-method"></a>AddFile2 メソッド
アセンブリにファイルを追加します。 は、バインドされていないモジュールを作成するためにも使用できます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT AddFile2(  
    mdAssembly AssemblyID,  
    LPCWSTR pszFilename,  
    DWORD dwFlags,  
    IMetaDataEmit2* pEmitter,  
    mdFile* pFileToken  
) PURE;  
```  
  
## <a name="parameters"></a>パラメーター  
 `AssemblyID`  
 ファイルが追加されるアセンブリの ID。  
  
 `pszFilename`  
 追加するファイルの名前。  
  
 `dwFlags`  
 COM + は、`ffContainsNoMetaData` や `ffWriteable`などの `FileDef` フラグを持ちます。 `dwFlags` は、の[メソッド](../metadata/imetadataassemblyemit-definefile-method.md)に渡されます。  
  
 `pEmitter`  
 [IMetaDataEmit2 インターフェイス](../metadata/imetadataemit2-interface.md)インターフェイスへのインターフェイス。  
  
 `pFileToken`  
 追加するファイルの ID を受け取ります。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK を返します。  
  
## <a name="requirements"></a>要件  
 Alink. h が必要です。  
  
## <a name="see-also"></a>参照

- [IALink2 インターフェイス](ialink2-interface.md)
- [IALink インターフェイス](ialink-interface.md)
- [ALink API](index.md)
