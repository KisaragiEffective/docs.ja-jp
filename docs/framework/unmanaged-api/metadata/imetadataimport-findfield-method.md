---
title: IMetaDataImport::FindField メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataImport.FindField
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::FindField
helpviewer_keywords:
- IMetaDataImport::FindField method [.NET Framework metadata]
- FindField method [.NET Framework metadata]
ms.assetid: 38cd4e16-fbb2-471c-aa73-ac51a1931ad2
topic_type:
- apiref
ms.openlocfilehash: 842d6c0deb90bc45cb59454fb30fcc3544d742f1
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74437941"
---
# <a name="imetadataimportfindfield-method"></a>IMetaDataImport::FindField メソッド
指定した <xref:System.Type> で囲まれ、指定された名前とメタデータシグネチャを持つフィールドの FieldDef トークンへのポインターを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT FindField (  
   [in]  mdTypeDef         td,  
   [in]  LPCWSTR           szName,  
   [in]  PCCOR_SIGNATURE   pvSigBlob,  
   [in]  ULONG             cbSigBlob,  
   [out] mdFieldDef        *pmb  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `td`  
 から検索対象のフィールドを囲むクラスまたはインターフェイスの TypeDef トークン。 この値が `mdTokenNil`場合は、グローバル変数に対して参照が行われます。  
  
 `szName`  
 から検索するフィールドの名前。  
  
 `pvSigBlob`  
 からフィールドのバイナリメタデータシグネチャへのポインター。  
  
 `cbSigBlob`  
 から`pvSigBlob`のサイズ (バイト単位)。  
  
 `pmb`  
 入出力一致する FieldDef トークンへのポインター。  
  
## <a name="remarks"></a>コメント  
 フィールドは、外側のクラスまたはインターフェイス (`td`)、その名前 (`szName`)、および必要に応じて署名 (`pvSigBlob`) を使用して指定します。  
  
 シグネチャは特定のスコープにバインドされているため、`FindField` に渡される署名は、現在のスコープで生成される必要があります。 署名には、外側のクラスまたは値の型を識別するトークンを埋め込むことができます。 (トークンは、ローカルの TypeDef テーブルのインデックスです)。 現在のスコープのコンテキスト外でランタイムシグネチャを作成し、その署名を `FindField`の入力として使用することはできません。  
  
 `FindField` は、クラスまたはインターフェイスで直接定義されたフィールドのみを検索します。継承されたフィールドは見つかりません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
