---
title: IMetaDataEmit::DefineTypeDef メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.DefineTypeDef
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::DefineTypeDef
helpviewer_keywords:
- IMetaDataEmit::DefineTypeDef method [.NET Framework metadata]
- DefineTypeDef method [.NET Framework metadata]
ms.assetid: dd11c485-be95-4b97-9cd8-68679a4fb432
topic_type:
- apiref
ms.openlocfilehash: 031996813718a074eebab62ff54a2de52b898c22
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74450215"
---
# <a name="imetadataemitdefinetypedef-method"></a>IMetaDataEmit::DefineTypeDef メソッド
共通言語ランタイム型の型定義を作成し、その型定義のメタデータトークンを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT DefineTypeDef (   
    [in]  LPCWSTR     szTypeDef,   
    [in]  DWORD       dwTypeDefFlags,   
    [in]  mdToken     tkExtends,   
    [in]  mdToken     rtkImplements[],   
    [out] mdTypeDef   *ptd  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `szTypeDef`  
 からUnicode での型の名前。  
  
 `dwTypeDefFlags`  
 [in] 属性 `TypeDef` ます。 これは `CoreTypeAttr` 値のビットマスクです。  
  
 `tkExtends`  
 から基本クラスのトークン。 `mdTypeDef` または `mdTypeRef` トークンのいずれかである必要があります。  
  
 `rtkImplements`  
 からこのクラスまたはインターフェイスが実装するインターフェイスを指定するトークンの配列。  
  
 `ptd`  
 入出力割り当てられた `mdTypeDef` トークン。  
  
## <a name="remarks"></a>コメント  
 `dwTypeDefFlags` のフラグは、作成される型が共通型システム参照型 (クラスまたはインターフェイス) であるか、共通型システム値型であるかを指定します。  
  
 このメソッドは、指定されたパラメーターに応じて、この型によって継承または実装されるインターフェイスごとに `mdInterfaceImpl` レコードを作成する場合もあります。 ただし、このメソッドはこれらの `mdInterfaceImpl` トークンを返しません。 クライアントが後で `mdInterfaceImpl` トークンを追加または変更する場合は、`IMetaDataImport` インターフェイスを使用してそれらを列挙する必要があります。 `[default]` インターフェイスの COM セマンティクスを使用する場合は、`rtkImplements`の最初の要素として既定のインターフェイスを指定する必要があります。クラスに設定されたカスタム属性は、クラスに既定のインターフェイスがあることを示します (これは常に、クラスに対して宣言された最初の `mdInterfaceImpl` トークンと見なされます)。  
  
 `rtkImplements` 配列の各要素は、`mdTypeDef` または `mdTypeRef` トークンを保持します。 配列の最後の要素は `mdTokenNil`である必要があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)
- [IMetaDataEmit2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-interface.md)
