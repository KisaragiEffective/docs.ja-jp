---
title: IMetaDataAssemblyEmit::DefineAssemblyRef メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataAssemblyEmit.DefineAssemblyRef
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataAssemblyEmit::DefineAssemblyRef
helpviewer_keywords:
- DefineAssemblyRef method [.NET Framework metadata]
- IMetaDataAssemblyEmit::DefineAssemblyRef method [.NET Framework metadata]
ms.assetid: 0b284b18-0084-4b3a-912a-5ebe9f29c88b
topic_type:
- apiref
ms.openlocfilehash: c88b7a401a19b1bd0e02edab7ef7bbee1372199e
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74432083"
---
# <a name="imetadataassemblyemitdefineassemblyref-method"></a>IMetaDataAssemblyEmit::DefineAssemblyRef メソッド
このアセンブリが参照するアセンブリのメタデータを含む `AssemblyRef` 構造体を作成し、関連付けられたメタデータ トークンを返します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT DefineAssemblyRef (  
    [in]  void                *pbPublicKeyOrToken,  
    [in]  ULONG               cbPublicKeyOrToken,  
    [in]  LPCWSTR             szName,  
    [in]  ASSEMBLYMETADATA    pMetaData,  
    [in]  void                *pbHashValue,  
    [in]  ULONG               cbHashValue,  
    [in]  DWORD               dwAssemblyRefFlags,  
    [out] mdAssemblyRef       *pmdar  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pbPublicKeyOrToken`  
 から参照アセンブリの発行元の公開キー。 ヘルパー関数[StrongNameTokenFromAssembly](../../../../docs/framework/unmanaged-api/strong-naming/strongnametokenfromassembly-function.md)を使用して、このパラメーターとして渡す公開キーのハッシュを取得できます。  
  
 `cbPublicKeyOrToken`  
 から`pbPublicKeyOrToken`のサイズ (バイト単位)。  
  
 `szName`  
 からユーザーが判読できる、アセンブリのテキスト名。 この値は、1024文字を超えないようにする必要があります。  
  
 `pMetaData`  
 から参照されたアセンブリのバージョン、プラットフォーム、およびロケール情報を格納している ASSEMBLYMETADATA インスタンス。  
  
 `pbHashValue`  
 から参照アセンブリに関連付けられているハッシュデータ。 省略可。  
  
 `cbHashValue`  
 から`pbHashValue`のサイズ (バイト単位)。  
  
 `dwAssemblyRefFlags`  
 から実行エンジンの動作に影響を与える[Corassemblyflags](../../../../docs/framework/unmanaged-api/metadata/corassemblyflags-enumeration.md)値のビットごとの組み合わせ。  
  
 `pmdar`  
 入出力返された `AssemblyRef` メタデータトークンへのポインター。  
  
## <a name="remarks"></a>コメント  
 このアセンブリが参照するアセンブリごとに1つの `AssemblyRef` メタデータ構造を定義する必要があります。  
  
 実行時には、参照されたアセンブリの詳細がアセンブリリゾルバーに渡され、"ビルド済み" の情報を表すことが示されます。 次に、アセンブリリゾルバーがポリシーを適用します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataAssemblyEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyemit-interface.md)
