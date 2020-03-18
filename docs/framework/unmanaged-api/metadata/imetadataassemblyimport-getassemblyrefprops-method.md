---
title: IMetaDataAssemblyImport::GetAssemblyRefProps メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyImport.GetAssemblyRefProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyImport::GetAssemblyRefProps
helpviewer_keywords:
- IMetaDataAssemblyImport::GetAssemblyRefProps method [.NET Framework metadata]
- GetAssemblyRefProps method [.NET Framework metadata]
ms.assetid: 5c6b7fb4-cbca-4479-b650-ab9a99732ea0
topic_type:
- apiref
ms.openlocfilehash: 4149db74adfa26df221eed5c590766a023bb105e
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74448229"
---
# <a name="imetadataassemblyimportgetassemblyrefprops-method"></a>IMetaDataAssemblyImport::GetAssemblyRefProps メソッド
指定されたメタデータシグネチャを持つアセンブリ参照のプロパティのセットを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetAssemblyRefProps (  
    [in]  mdAssemblyRef        mdar,   
    [out] const void          **ppbPublicKeyOrToken,   
    [out] ULONG                *pcbPublicKeyOrToken,   
    [out] LPWSTR               szName,   
    [in]  ULONG                cchName,   
    [out] ULONG                *pchName,   
    [out] ASSEMBLYMETADATA     *pMetaData,   
    [out] const void           **ppbHashValue,   
    [out] ULONG                *pcbHashValue,   
    [out] DWORD                *pdwAssemblyRefFlags  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `mdar`  
 からプロパティを取得する対象のアセンブリ参照を表す `mdAssemblyRef` メタデータトークン。  
  
 `ppbPublicKeyOrToken`  
 入出力公開キーまたはメタデータトークンへのポインター。  
  
 `pcbPublicKeyOrToken`  
 入出力返される公開キーまたはトークン内のバイト数。  
  
 `szName`  
 入出力アセンブリの簡易名。  
  
 `cchName`  
 から`szName`のサイズ (ワイド文字単位)。  
  
 `pchName`  
 入出力`szName`に実際に返されるワイド文字数へのポインター。  
  
 `pMetaData`  
 入出力アセンブリメタデータを格納している ASSEMBLYMETADATA 構造体へのポインター。  
  
 `ppbHashValue`  
 入出力ハッシュ値へのポインター。 これは、参照されるアセンブリの `PublicKey` プロパティの SHA-1 アルゴリズムを使用したハッシュです。 [Assemblyrefflags](../../../../docs/framework/unmanaged-api/metadata/assemblyrefflags-enumeration.md)の列挙の arfFullOriginator フラグが設定されている場合を除きます。  
  
 `pcbHashValue`  
 入出力返されたハッシュ値のワイド文字の数。  
  
 `pdwAssemblyRefFlags`  
 入出力アセンブリに適用されるメタデータを記述するフラグへのポインター。 Flags 値は、1つまたは複数の[Corassemblyflags](../../../../docs/framework/unmanaged-api/metadata/corassemblyflags-enumeration.md)値を組み合わせたものです。  
  
## <a name="return-value"></a>戻り値  
 成功した場合、このメソッドは S_OK を返します。それ以外の場合は、Winerror.h ヘッダーファイルで定義されているエラーコードの1つを返します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataAssemblyImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-interface.md)
