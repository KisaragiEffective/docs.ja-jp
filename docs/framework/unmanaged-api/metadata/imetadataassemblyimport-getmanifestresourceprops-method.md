---
title: IMetaDataAssemblyImport::GetManifestResourceProps メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.GetManifestResourceProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::GetManifestResourceProps
helpviewer_keywords:
- GetManifestResourceProps method [.NET Framework metadata]
- IMetaDataAssemblyImport::GetManifestResourceProps method [.NET Framework metadata]
ms.assetid: 00be4789-ac63-4397-b2ec-1629a5c5a585
topic_type:
- apiref
ms.openlocfilehash: c1792ed0f15f8cfb62567593c9694453650f0bb9
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74436323"
---
# <a name="imetadataassemblyimportgetmanifestresourceprops-method"></a>IMetaDataAssemblyImport::GetManifestResourceProps メソッド
指定されたメタデータシグネチャを持つマニフェストリソースのプロパティのセットを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetManifestResourceProps (  
    [in]  mdManifestResource   mdmr,   
    [out] LPWSTR               szName,   
    [in]  ULONG                cchName,   
    [out] ULONG                *pchName,   
    [out] mdToken              *ptkImplementation,   
    [out] DWORD                *pdwOffset,   
    [out] DWORD                *pdwResourceFlags  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `mdmr`  
 からプロパティを取得する対象のリソースを表す `mdManifestResource` トークン。  
  
 `szName`  
 入出力リソースの名前。  
  
 `cchName`  
 から`szName`のサイズ (ワイド文字単位)。  
  
 `pchName`  
 入出力`szName`に実際に返されるワイド文字数へのポインター。  
  
 `ptkImplementation`  
 入出力リソースを格納しているファイルまたはアセンブリを表す、`mdFile` トークンまたは `mdAssemblyRef` トークンへのポインター。  
  
 `pdwOffset`  
 入出力ファイル内のリソースの先頭へのオフセットを指定する値へのポインター。  
  
 `pdwResourceFlags`  
 入出力リソースに適用されるメタデータを記述するフラグへのポインター。 Flags 値は、1つ以上の[Cormanifestresourceflags](../../../../docs/framework/unmanaged-api/metadata/cormanifestresourceflags-enumeration.md)値を組み合わせたものです。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataAssemblyImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-interface.md)
