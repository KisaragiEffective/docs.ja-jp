---
title: ImportTypes2 メソッド
ms.date: 03/30/2017
api_name:
- IALink2.ImportTypes2
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- ImportTypes2
helpviewer_keywords:
- ImportTypes2 method
ms.assetid: 32f3ba58-9695-41e9-ba58-fd19e45ed396
topic_type:
- apiref
ms.openlocfilehash: 8dae903ab76ab83ac0818c4bc4a76e81094bdf65
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74445669"
---
# <a name="importtypes2-method"></a>ImportTypes2 メソッド
型のインポートを開始します。 [Importfile メソッド](importfile-method.md)を使用してインポートされた各スコープから型のインポートを開始するには、このメソッドを呼び出します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT ImportTypes2(  
    mdAssembly AssemblyID,  
    mdToken FileToken,  
    DWORD dwScope,  
    HALINKENUM* phEnum,  
    IMetaDataImport2** ppImportScope,  
    DWORD* pdwCountOfTypes  
) PURE;  
```  
  
## <a name="parameters"></a>パラメーター  
 `AssemblyID`  
 インポート先のアセンブリの ID。  
  
 `FileToken`  
 インポート元のファイルの ID。  
  
 `dwScope`  
 インポート元の0から始まるスコープ。  
  
 `phEnum`  
 指定されたスコープ内の型の列挙子ハンドルを受け取ります。  
  
 `ppImportScope`  
 必要に応じて、 [IMetaDataImport2 インターフェイス](../metadata/imetadataimport2-interface.md)インターフェイスを受け取ります。  
  
 `pdwCountOfTypes`  
 必要に応じて、指定されたスコープ内の型の数を受け取ります。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK を返します。  
  
## <a name="requirements"></a>要件  
 Alink. h が必要です。  
  
## <a name="see-also"></a>関連項目

- [IALink2 インターフェイス](ialink2-interface.md)
- [IALink インターフェイス](ialink-interface.md)
- [ALink API](index.md)
