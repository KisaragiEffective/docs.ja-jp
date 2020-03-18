---
title: IMetaDataEmit::DefineImportMember メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.DefineImportMember
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::DefineImportMember
helpviewer_keywords:
- DefineImportMember method [.NET Framework metadata]
- IMetaDataEmit::DefineImportMember method [.NET Framework metadata]
ms.assetid: c7dd94c6-335b-46ff-9dfe-505056db5673
topic_type:
- apiref
ms.openlocfilehash: 75711b3d87699ff5db21a04351ff0acaccabb5aa
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74431861"
---
# <a name="imetadataemitdefineimportmember-method"></a>IMetaDataEmit::DefineImportMember メソッド
現在のスコープの外部で定義されている型またはモジュールの指定したメンバーへの参照を作成し、その参照のトークンを定義します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT DefineImportMember (   
    [in]  IMetaDataAssemblyImport  *pAssemImport,   
    [in]  const void               *pbHashValue,   
    [in]  ULONG                    cbHashValue,  
    [in]  IMetaDataImport          *pImport,   
    [in]  mdToken                  mbMember,   
    [in]  IMetaDataAssemblyEmit    *pAssemEmit,   
    [in]  mdToken                  tkParent,   
    [out] mdMemberRef              *pmr   
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pAssemImport`  
 からターゲットメンバーのインポート元のアセンブリを表す[IMetaDataAssemblyImport](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyimport-interface.md)インターフェイス。  
  
 `pbHashValue`  
 から`pAssemImport`によって指定されたアセンブリのハッシュを格納している配列。  
  
 `cbHashValue`  
 [in] `pbHashValue` 配列のバイト数。  
  
 `pImport`  
 からターゲットメンバーのインポート元のメタデータスコープを表す[IMetaDataImport](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)インターフェイス。  
  
 `mbMember`  
 からターゲットメンバーを指定するメタデータトークン。 トークンには、`mdMethodDef` (メンバーメソッドの場合)、`mdProperty` (メンバープロパティの場合)、または `mdFieldDef` (メンバーフィールドの場合) トークンを指定できます。  
  
 `pAssemEmit`  
 からターゲットメンバーがインポートされるアセンブリを表す[IMetaDataAssemblyEmit](../../../../docs/framework/unmanaged-api/metadata/imetadataassemblyemit-interface.md)インターフェイス。  
  
 `tkParent`  
 からターゲットメンバーを所有する型またはモジュールの `mdTypeRef` または `mdModuleRef` トークン。  
  
 `pmr`  
 入出力メンバー参照の現在のスコープで定義されている `mdMemberRef` トークン。  
  
## <a name="remarks"></a>コメント  
 `DefineImportMember` メソッドは、`mbMember`によって指定されたメンバーを検索し、`pImport`によって指定された別のスコープで定義され、そのプロパティを取得します。 この情報を使用して、現在のスコープで[IMetaDataEmit::D efinememberref](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definememberref-method.md)メソッドを呼び出し、メンバー参照を作成します。  
  
 一般に、`DefineImportMember` メソッドを使用する前に、現在のスコープで、ターゲットメンバーの親クラス、インターフェイス、またはモジュールの型参照またはモジュール参照を作成する必要があります。 この参照のメタデータトークンは、`tkParent` 引数で渡されます。 後でコンパイラまたはリンカーによって解決される場合は、ターゲットメンバーの親への参照を作成する必要はありません。 要約:  
  
- ターゲットメンバーがフィールドまたはメソッドの場合は、 [IMetaDataEmit::D efinetyperefbyname](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definetyperefbyname-method.md)または[IMetaDataEmit::D efineImportType](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-defineimporttype-method.md)メソッドを使用して、メンバーの親クラスまたは親インターフェイスの型参照を現在のスコープ内に作成します。  
  
- ターゲットメンバーがグローバル変数またはグローバル関数 (つまり、クラスまたはインターフェイスのメンバーではない) である場合は、 [IMetaDataEmit::D efinemoduleref](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definemoduleref-method.md)メソッドを使用して、メンバーの親モジュールに対して、現在のスコープでモジュール参照を作成します。  
  
- ターゲットメンバーの親がコンパイラまたはリンカーによって後で解決される場合は、`tkParent`で `mdTokenNil` を渡します。 これが適用される唯一のシナリオは、グローバル関数またはグローバル変数が、最終的に現在のモジュールにリンクされ、メタデータがマージされる .obj ファイルからインポートされる場合です。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IMetaDataEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)
- [IMetaDataEmit2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-interface.md)
