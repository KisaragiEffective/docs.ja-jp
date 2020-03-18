---
title: IMetaDataImport::FindMethod メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataImport.FindMethod
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::FindMethod
helpviewer_keywords:
- FindMethod method [.NET Framework metadata]
- IMetaDataImport::FindMethod method [.NET Framework metadata]
ms.assetid: 0f9bde1d-e306-438d-941b-d0925b322304
topic_type:
- apiref
ms.openlocfilehash: 470b6511366cef1680eaf97f9ab376736add55c4
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74437898"
---
# <a name="imetadataimportfindmethod-method"></a>IMetaDataImport::FindMethod メソッド
指定した <xref:System.Type> で囲まれ、指定された名前とメタデータシグネチャを持つメソッドの MethodDef トークンへのポインターを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT FindMethod (  
   [in]  mdTypeDef          td,  
   [in]  LPCWSTR            szName,   
   [in]  PCCOR_SIGNATURE    pvSigBlob,   
   [in]  ULONG              cbSigBlob,   
   [out] mdMethodDef        *pmb  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `td`  
 から検索対象のメンバーを囲む型 (クラスまたはインターフェイス) の `mdTypeDef` トークン。 この値が `mdTokenNil`場合は、グローバル関数の参照が行われます。  
  
 `szName`  
 から検索するメソッドの名前。  
  
 `pvSigBlob`  
 からメソッドのバイナリメタデータシグネチャへのポインター。  
  
 `cbSigBlob`  
 から`pvSigBlob`のサイズ (バイト単位)。  
  
 `pmb`  
 入出力一致する MethodDef トークンへのポインター。  
  
## <a name="remarks"></a>コメント  
 メソッドは、外側のクラスまたはインターフェイス (`td`)、その名前 (`szName`)、および必要に応じてシグネチャ (`pvSigBlob`) を使用して指定します。 クラスまたはインターフェイスに同じ名前のメソッドが複数存在する可能性があります。 その場合は、メソッドのシグネチャを渡して、一意の一致を検索します。  
  
 シグネチャは特定のスコープにバインドされているため、`FindMethod` に渡される署名は、現在のスコープで生成される必要があります。 署名には、外側のクラスまたは値の型を識別するトークンを埋め込むことができます。 トークンは、ローカルの TypeDef テーブルのインデックスです。 現在のスコープのコンテキスト外でランタイムシグネチャを作成し、その署名を入力として使用して `FindMethod`することはできません。  
  
 `FindMethod` は、クラスまたはインターフェイスで直接定義されたメソッドのみを検索します。継承されたメソッドは見つかりません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Reflection.MethodInfo>
- [IMetaDataImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
