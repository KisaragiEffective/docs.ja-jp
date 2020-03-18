---
title: ExportNestedType メソッド
ms.date: 03/30/2017
api_name:
- IALink.ExportNestedType
- ExportNestedType
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- ExportNestedType
helpviewer_keywords:
- ExportNestedType method
ms.assetid: dec7df60-4d30-47c8-99db-72e0419e5f76
topic_type:
- apiref
ms.openlocfilehash: fded6b95144d4088a2abc8dfcc4ef8eda331c34f
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74438424"
---
# <a name="exportnestedtype-method"></a>ExportNestedType メソッド
入れ子にされた型をエクスポート可能として指定します。 [Exporttype メソッド](exporttype-method.md)も入れ子になった型をエクスポートできますが、このメソッドの方が高速です。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT ExportNestedType(  
    mdAssembly      AssemblyID,  
    mdToken         FileToken,  
    mdTypeDef       TypeToken,  
    mdExportedType  ParentType,  
    LPCWSTR         pszTypename,  
    DWORD           dwFlags,  
    mdExportedType* pType  
) PURE;   
```  
  
## <a name="parameters"></a>パラメーター  
 `AssemblyID`  
 エクスポート元のアセンブリの ID。  
  
 `FileToken`  
 エクスポート可能にする型を定義するファイルのトークンまたはアセンブリ。  
  
 `TypeToken`  
 エクスポート可能にする型の型トークン。  
  
 `ParentType`  
 親の種類のトークン。  
  
 `pszTypename`  
 エクスポートする完全修飾型名。  
  
 `dwFlags`  
 `tdPublic` や `tdNested`などのフラグを `ComType` します。 この値は、この[メソッド](../metadata/imetadataassemblyemit-defineexportedtype-method.md)に渡すことができます。  
  
 `pType`  
 エクスポートされた型のトークンを受け取ります。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK を返します。  
  
## <a name="requirements"></a>要件  
 Alink. h が必要です。  
  
## <a name="see-also"></a>参照

- [IALink インターフェイス](ialink-interface.md)
- [IALink2 インターフェイス](ialink2-interface.md)
- [ALink API](index.md)
