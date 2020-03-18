---
title: IMetaDataEmit::DefineField メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.DefineField
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::DefineField
helpviewer_keywords:
- IMetaDataEmit::DefineField method [.NET Framework metadata]
- DefineField method, IMetaDataEmit interface [.NET Framework metadata
ms.assetid: 6b5be4fc-2e86-499c-8b09-833160bca767
topic_type:
- apiref
ms.openlocfilehash: 40f24a4ea628ce92a27ab1bfe97fc87a57dfa4f0
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74432548"
---
# <a name="imetadataemitdefinefield-method"></a>IMetaDataEmit::DefineField メソッド
指定したメタデータシグネチャを持つフィールドの定義を作成し、そのフィールド定義へのトークンを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT DefineField (   
    [in]  mdTypeDef   td,   
    [in]  LPCWSTR     szName,   
    [in]  DWORD       dwFieldFlags,   
    [in]  PCCOR_SIGNATURE pvSigBlob,   
    [in]  ULONG       cbSigBlob,   
    [in]  DWORD       dwCPlusTypeFlag,   
    [in]  void const  *pValue,   
    [in]  ULONG       cchValue,   
    [out] mdFieldDef  *pmd   
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `td`  
 から外側のクラスまたはインターフェイスの `mdTypeDef` トークン。  
  
 `szName`  
 からUnicode でのフィールド名。  
  
 `dwFieldFlags`  
 からフィールド属性。 これは `CorFieldAttr` 値のビットマスクです。  
  
 `pvSigBlob`  
 からBLOB としてのフィールドシグネチャ。  
  
 `cbSigBlob`  
 から`pvSigBlob`内のバイト数。  
  
 `dwCPlusTypeFlag`  
 から定数値の `ELEMENT_TYPE_` *\** 。 これは `CorElementType` の値です。 フィールドの定数値を定義していない場合は、`ELEMENT_TYPE_END`を使用します。  
  
 `pValue`  
 からフィールドの定数値。  
  
 `cchValue`  
 から`pValue`の (Unicode) 文字のサイズ。  
  
 `pmd`  
 入出力割り当てられた `mdFieldDef` トークン。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IMetaDataEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)
- [IMetaDataEmit2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-interface.md)
