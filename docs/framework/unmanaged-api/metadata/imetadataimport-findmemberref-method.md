---
title: IMetaDataImport::FindMemberRef メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataImport.FindMemberRef
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::FindMemberRef
helpviewer_keywords:
- IMetaDataImport::FindMemberRef method [.NET Framework metadata]
- FindMemberRef method [.NET Framework metadata]
ms.assetid: 1ccda329-d752-4d89-abe8-511af3c3f4c9
topic_type:
- apiref
ms.openlocfilehash: 59512cc1c1b280d7fe6deb2f9d721ad53547e356
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74437959"
---
# <a name="imetadataimportfindmemberref-method"></a>IMetaDataImport::FindMemberRef メソッド
指定した <xref:System.Type> で囲まれ、指定された名前とメタデータシグネチャを持つメンバー参照の MemberRef トークンへのポインターを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT FindMemberRef (  
   [in]  mdTypeRef          td,  
   [in]  LPCWSTR            szName,   
   [in]  PCCOR_SIGNATURE    pvSigBlob,   
   [in]  ULONG              cbSigBlob,   
   [out] mdMemberRef        *pmr  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `td`  
 から検索対象のメンバー参照を囲むクラスまたはインターフェイスの TypeRef トークン。 この値が `mdTokenNil`場合、グローバル変数またはグローバル関数参照に対して参照が行われます。  
  
 `szName`  
 から検索対象のメンバー参照の名前。  
  
 `pvSigBlob`  
 からメンバー参照のバイナリメタデータシグネチャへのポインター。  
  
 `cbSigBlob`  
 から`pvSigBlob`のサイズ (バイト単位)。  
  
 `pmr`  
 入出力一致する MemberRef トークンへのポインター。  
  
## <a name="remarks"></a>コメント  
 メンバーは、外側のクラスまたはインターフェイス (`td`)、その名前 (`szName`)、および必要に応じてシグネチャ (`pvSigBlob`) を使用して指定します。  
  
 シグネチャは特定のスコープにバインドされているため、`FindMemberRef` に渡される署名は、現在のスコープで生成される必要があります。 署名には、外側のクラスまたは値の型を識別するトークンを埋め込むことができます。 トークンは、ローカルの TypeDef テーブルのインデックスです。 現在のスコープのコンテキスト外でランタイムシグネチャを作成し、その署名を `FindMemberRef`の入力として使用することはできません。  
  
 `FindMemberRef` は、クラスまたはインターフェイスで直接定義されたメンバー参照だけを検索します。継承されたメンバー参照は見つかりません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
