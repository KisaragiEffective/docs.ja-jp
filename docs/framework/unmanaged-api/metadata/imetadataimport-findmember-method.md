---
title: IMetaDataImport::FindMember メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataImport.FindMember
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::FindMember
helpviewer_keywords:
- IMetaDataImport::FindMember method [.NET Framework metadata]
- FindMember method [.NET Framework metadata]
ms.assetid: ad32fb84-c2b6-41cd-888d-787ff3a90449
topic_type:
- apiref
ms.openlocfilehash: 7a46fa5319a1badc0cf28dcdbf535a6ed017c9c9
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74437917"
---
# <a name="imetadataimportfindmember-method"></a>IMetaDataImport::FindMember メソッド
指定した <xref:System.Type> で囲まれ、指定された名前とメタデータシグネチャを持つフィールドまたはメソッドの MemberDef トークンへのポインターを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT FindMember (  
   [in]  mdTypeDef         td,  
   [in]  LPCWSTR           szName,   
   [in]  PCCOR_SIGNATURE   pvSigBlob,   
   [in]  ULONG             cbSigBlob,   
   [out] mdToken           *pmb  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `td`  
 から検索対象のメンバーを囲むクラスまたはインターフェイスの TypeDef トークン。 この値が `mdTokenNil`場合は、グローバル変数またはグローバル関数の参照が行われます。  
  
 `szName`  
 から検索対象のメンバーの名前。  
  
 `pvSigBlob`  
 からメンバーのバイナリメタデータシグネチャへのポインター。  
  
 `cbSigBlob`  
 から`pvSigBlob`のサイズ (バイト単位)。  
  
 `pmb`  
 入出力一致する MemberDef トークンへのポインター。  
  
## <a name="remarks"></a>コメント  
 メンバーは、外側のクラスまたはインターフェイス (`td`)、その名前 (`szName`)、および必要に応じてシグネチャ (`pvSigBlob`) を使用して指定します。 クラスまたはインターフェイスに同じ名前のメンバーが複数存在する可能性があります。 その場合は、メンバーのシグネチャを渡して、一意の一致を検索します。  
  
 シグネチャは特定のスコープにバインドされているため、`FindMember` に渡される署名は、現在のスコープで生成される必要があります。 署名には、外側のクラスまたは値の型を識別するトークンを埋め込むことができます。 トークンは、ローカルの TypeDef テーブルのインデックスです。 現在のスコープのコンテキスト外でランタイムシグネチャを作成し、その署名を入力として使用して `FindMember`することはできません。  
  
 `FindMember` は、クラスまたはインターフェイスで直接定義されたメンバーのみを検索します。継承されたメンバーは見つかりません。  
  
> [!NOTE]
> `FindMember` はヘルパーメソッドです。 [IMetaDataImport:: FindMethod](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-findmethod-method.md); を呼び出します。この呼び出しで一致するものが見つからない場合は、`FindMember` [IMetaDataImport:: FindField](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-findfield-method.md)を呼び出します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
