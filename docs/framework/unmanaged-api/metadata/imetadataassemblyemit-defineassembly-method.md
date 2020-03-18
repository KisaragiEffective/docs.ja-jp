---
title: IMetaDataAssemblyEmit::DefineAssembly メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyEmit.DefineAssembly
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyEmit::DefineAssembly
helpviewer_keywords:
- IMetaDataAssemblyEmit::DefineAssembly method [.NET Framework metadata]
- DefineAssembly method [.NET Framework metadata]
ms.assetid: a0637d66-74bf-4f2d-8137-9ff838bccece
topic_type:
- apiref
ms.openlocfilehash: 20628e708261076c6e172ff30c366a0d69c2e0f2
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74432127"
---
# <a name="imetadataassemblyemitdefineassembly-method"></a>IMetaDataAssemblyEmit::DefineAssembly メソッド
指定したアセンブリのメタデータを含む `Assembly` 構造体を作成し、関連付けられているメタデータトークンを返します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT DefineAssembly (  
    [in]  void                 *pbPublicKey,  
    [in]  ULONG                cbPublicKey,  
    [in]  ULONG                uHashAlgId,  
    [in]  LPCWSTR              szName,   
    [in]  ASSEMBLYMETADATA     *pMetaData,  
    [in]  DWORD                dwAssemblyFlags,  
    [out] mdAssembly           *pmda  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pbPublicKey`  
 からアセンブリの発行元を識別する公開キー。アセンブリに厳密な名前が付けられていない場合は NULL。  
  
 `cbPublicKey`  
 から`pbPublicKey`のサイズ (バイト単位)。  
  
 `uHashAlgId`  
 からアセンブリ内のファイルを暗号化するために使用するハッシュアルゴリズムの識別子。または、SHA-1 アルゴリズムを指定する場合は NULL。  
  
 `szName`  
 からユーザーが判読できる、アセンブリのテキスト名。 この値は、1024文字を超えないようにする必要があります。  
  
 `pMetaData`  
 からアセンブリのバージョン、プラットフォーム、およびロケール情報を格納している ASSEMBLYMETADATA インスタンスへのポインター。  
  
 `dwAssemblyFlags`  
 からアセンブリの機能を記述する[Corassemblyflags](../../../../docs/framework/unmanaged-api/metadata/corassemblyflags-enumeration.md)値の組み合わせ。  
  
 `pmda`  
 入出力メタデータトークンへのポインター。  
  
## <a name="remarks"></a>コメント  
 マニフェスト内で定義できる `Assembly` メタデータ構造は1つだけです。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataAssemblyEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyemit-interface.md)
