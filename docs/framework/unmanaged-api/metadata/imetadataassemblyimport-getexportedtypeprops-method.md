---
title: IMetaDataAssemblyImport::GetExportedTypeProps メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.GetExportedTypeProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::GetExportedTypeProps
helpviewer_keywords:
- GetExportedTypeProps method [.NET Framework metadata]
- IMetaDataAssemblyImport::GetExportedTypeProps method [.NET Framework metadata]
ms.assetid: 25ca7623-5a55-4f09-b44a-36b03d142278
topic_type:
- apiref
ms.openlocfilehash: 82302124828a2dab73b445128d7d847e112edd36
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74448213"
---
# <a name="imetadataassemblyimportgetexportedtypeprops-method"></a>IMetaDataAssemblyImport::GetExportedTypeProps メソッド
指定したメタデータシグネチャを持つ、エクスポートされた型のプロパティのセットを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetExportedTypeProps (  
    [in]  mdExportedType    mdct,   
    [out] LPWSTR            szName,   
    [in]  ULONG             cchName,   
    [out] ULONG             *pchName,   
    [out] mdToken           *ptkImplementation,   
    [out] mdTypeDef         *ptkTypeDef,   
    [out] DWORD             *pdwExportedTypeFlags  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `mdct`  
 からエクスポートされた型を表す `mdExportedType` メタデータトークン。  
  
 `szName`  
 入出力エクスポートされた型の名前。  
  
 `cchName`  
 から`szName`のサイズ (ワイド文字単位)。  
  
 `pchName`  
 入出力実際に返されるワイド文字の数 `szName`  
  
 `ptkImplementation`  
 入出力エクスポートされた型のプロパティへのアクセスを格納または許可する、`mdFile`、`mdAssemblyRef`、または `mdExportedType` メタデータトークン。  
  
 `ptkTypeDef`  
 入出力ファイル内の型を表す `mdTypeDef` トークンへのポインター。  
  
 `pdwExportedTypeFlags`  
 入出力エクスポートされた型に適用されるメタデータを記述するフラグへのポインター。 Flags 値には、1つまたは複数の[Cortypeattr](../../../../docs/framework/unmanaged-api/metadata/cortypeattr-enumeration.md)値を指定できます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataAssemblyImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-interface.md)
