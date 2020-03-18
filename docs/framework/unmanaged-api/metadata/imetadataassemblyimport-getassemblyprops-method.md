---
title: IMetaDataAssemblyImport::GetAssemblyProps メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.GetAssemblyProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::GetAssemblyProps
helpviewer_keywords:
- GetAssemblyProps method [.NET Framework metadata]
- IMetaDataAssemblyImport::GetAssemblyProps method [.NET Framework metadata]
ms.assetid: 0eaa4aa9-9441-444a-920c-e4b2a2db899e
topic_type:
- apiref
ms.openlocfilehash: c3c57074ae53e2e1d8d41aa04cb6eb6089db58b5
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74449431"
---
# <a name="imetadataassemblyimportgetassemblyprops-method"></a>IMetaDataAssemblyImport::GetAssemblyProps メソッド
指定したメタデータシグネチャを持つアセンブリのプロパティのセットを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetAssemblyProps (  
    [in]  mdAssembly          mda,  
    [out] const void          **ppbPublicKey,   
    [out] ULONG               *pcbPublicKey,  
    [out] ULONG               *pulHashAlgId,  
    [out] LPWSTR              szName,  
    [in] ULONG                cchName,  
    [out] ULONG               *pchName,  
    [out] ASSEMBLYMETADATA    *pMetaData,  
    [out] DWORD               *pdwAssemblyFlags  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `mda`  
 [入力]。 プロパティを取得する対象のアセンブリを表す `mdAssembly` メタデータトークン。  
  
 `ppbPublicKey`  
 入出力公開キーまたはメタデータトークンへのポインター。  
  
 `pcbPublicKey`  
 入出力返される公開キーのバイト数。  
  
 `pulHashAlgId`  
 入出力アセンブリ内のファイルのハッシュに使用されるアルゴリズムへのポインター。  
  
 `szName`  
 入出力アセンブリの簡易名。  
  
 `cchName`  
 から`szName`のサイズ (ワイド文字単位)。  
  
 `pchName`  
 入出力`szName`に実際に返されるワイド文字数。  
  
 `pMetaData`  
 入出力アセンブリメタデータを格納している ASSEMBLYMETADATA 構造体へのポインター。  
  
 `pdwAssemblyFlags`  
 入出力アセンブリに適用されるメタデータを記述するフラグ。 この値は、1つまたは複数の[Corassemblyflags](../../../../docs/framework/unmanaged-api/metadata/corassemblyflags-enumeration.md)値を組み合わせたものです。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataAssemblyImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-interface.md)
