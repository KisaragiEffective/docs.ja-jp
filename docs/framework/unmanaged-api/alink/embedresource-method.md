---
title: EmbedResource メソッド
ms.date: 03/30/2017
api_name:
- IALink.EmbedResource
- EmbedResource
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- EmbedResource
helpviewer_keywords:
- EmbedResource method
ms.assetid: 667bd954-6dc6-4020-a3cb-0e8224179993
topic_type:
- apiref
ms.openlocfilehash: 24279870e7406de649df56e8aad31252513e95c7
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74446537"
---
# <a name="embedresource-method"></a>EmbedResource メソッド
埋め込みリソースを宣言します。 このメソッドは、実際にはリソースを埋め込みません。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EmbedResource(  
    mdAssembly  AssemblyID,  
    mdToken     FileToken,  
    LPCWSTR     pszResourceName,  
    DWORD       dwOffset,  
    DWORD       dwFlags  
) PURE;  
```  
  
## <a name="parameters"></a>パラメーター  
 `AssemblyID`  
 アセンブリの ID。  
  
 `FileToken`  
 リソースが含まれているファイルのファイルトークンまたはアセンブリ ID。  
  
 `pszResourceName`  
 リソースの名前。  
  
 `dwOffset`  
 RVA からのリソースのオフセット。  
  
 `dwFlags`  
 `mrPublic` や `mrPrivate`などのアクセシビリティフラグ。 これらのフラグは、を使用して、この[メソッド](../metadata/imetadataassemblyemit-defineexportedtype-method.md)に渡すことができます。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK を返します。  
  
## <a name="requirements"></a>要件  
 Alink. h が必要です。  
  
## <a name="see-also"></a>参照

- [IALink インターフェイス](ialink-interface.md)
- [IALink2 インターフェイス](ialink2-interface.md)
- [ALink API](index.md)
