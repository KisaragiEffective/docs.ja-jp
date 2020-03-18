---
title: IMetaDataAssemblyEmit::DefineExportedType メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyEmit.DefineExportedType
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyEmit::DefineExportedType
helpviewer_keywords:
- IMetaDataAssemblyEmit::DefineExportedType method [.NET Framework metadata]
- DefineExportedType method [.NET Framework metadata]
ms.assetid: fad01d7a-3178-4c8c-9f0a-4641e3701c9b
topic_type:
- apiref
ms.openlocfilehash: 44f97ef498eb8e64c55fc86b9f290b9e088293f6
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74432072"
---
# <a name="imetadataassemblyemitdefineexportedtype-method"></a>IMetaDataAssemblyEmit::DefineExportedType メソッド
指定してエクスポートした型のメタデータが含まれる `ExportedType` 構造体を作成し、関連付けられたメタデータ トークンを返します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT DefineExportedType (  
    [in]  LPCWSTR             szName,  
    [in]  mdToken             tkImplementation,   
    [in]  mdTypeDef           tkTypeDef,  
    [in]  DWORD               dwExportedTypeFlags,  
    [out] mdExportedType      *pmdct  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `szName`  
 からエクスポートする型の名前。 共通言語ランタイムのバージョン1.1 では、エクスポートされた型の名前は、型の `TypeDef` で指定された名前と完全に一致する必要があります。  
  
 `tkImplementation`  
 からエクスポートされた型を実装する場所を指定するトークン。 有効な値とそれに関連付けられている意味は次のとおりです。  
  
- 型 `mdFile`、このアセンブリ内の別のファイルに実装されています。  
  
- 型が別のアセンブリに実装されて `mdAssemblyRef`。  
  
- 型が他の型の中で入れ子になって `mdExportedTYpe`。  
  
- `mdFileNil` 型がマニフェストと同じファイル内にあり、入れ子にされた型ではありません。  
  
 `tkTypeDef`  
 からエクスポートする型を指定するメタデータのトークン。 この値は、型を実装するファイルの `TypeDef` テーブルに入力され、そのファイルがこのアセンブリ内にある場合にのみ関連します。  
  
 `dwExportedTypeFlags`  
 からエクスポートされた型のプロパティ設定を定義する[Cortypeattr](../../../../docs/framework/unmanaged-api/metadata/cortypeattr-enumeration.md)列挙値のビットごとの組み合わせ。  
  
 `pmdct`  
 入出力エクスポートされた型を示す、返されたメタデータトークンへのポインター。  
  
## <a name="remarks"></a>コメント  
 このアセンブリによって公開され、マニフェストを含むモジュール以外のモジュールで実装される型ごとに、`ExportedType` メタデータ構造を定義する必要があります。  
  
## <a name="requirements"></a>要件  
 **プラットフォーム:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IMetaDataAssemblyEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyemit-interface.md)
