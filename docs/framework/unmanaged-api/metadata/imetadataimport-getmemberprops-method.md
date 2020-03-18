---
title: IMetaDataImport::GetMemberProps メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetMemberProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetMemberProps
helpviewer_keywords:
- IMetaDataImport::GetMemberProps method [.NET Framework metadata]
- GetMemberProps method [.NET Framework metadata]
ms.assetid: 42790918-4142-4938-b8f4-a56979a55846
topic_type:
- apiref
ms.openlocfilehash: bc5bbba2fa4a95955e52a2e083a2097178b5d96a
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74437521"
---
# <a name="imetadataimportgetmemberprops-method"></a>IMetaDataImport::GetMemberProps メソッド
指定されたメタデータトークンによって参照される <xref:System.Type> メンバーの、名前、バイナリ署名、相対仮想アドレスなど、指定されたメンバー定義のメタデータに格納されている情報を取得します。 これは単純なヘルパーメソッドです。 *mb*が MethodDef の場合は、 **getmethodprops**が呼び出されます。*mb*が FieldDef の場合は、 **getfieldprops**が呼び出されます。 詳細については、これらの他の方法を参照してください。 
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetMemberProps (  
   [in]  mdToken           mb,   
   [out] mdTypeDef         *pClass,  
   [out] LPWSTR            szMember,   
   [in]  ULONG             cchMember,   
   [out] ULONG             *pchMember,   
   [out] DWORD             *pdwAttr,  
   [out] PCCOR_SIGNATURE   *ppvSigBlob,   
   [out] ULONG             *pcbSigBlob,   
   [out] ULONG             *pulCodeRVA,   
   [out] DWORD             *pdwImplFlags,   
   [out] DWORD             *pdwCPlusTypeFlag,   
   [out] UVCP_CONSTANT     *ppValue,  
   [out] ULONG             *pcchValue  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `mb`  
 から関連付けられているメタデータを取得するメンバーを参照するトークン。  
  
 `pClass`  
 入出力メンバーのクラスを表すメタデータトークンへのポインター。  
  
 `szMember`  
 入出力メンバーの名前。  
  
 `cchMember`  
 から`szMember` バッファーのワイド文字単位のサイズ。  
  
 `pchMember`  
 入出力返される名前のワイド文字単位のサイズ。  
  
 `pdwAttr`  
 入出力メンバーに適用されるフラグ値。  
  
 `ppvSigBlob`  
 入出力メンバーのバイナリメタデータシグネチャへのポインター。  
  
 `pcbSigBlob`  
 入出力`ppvSigBlob`のサイズ (バイト単位)。  
  
 `pulCodeRVA`  
 入出力メンバーの相対仮想アドレスへのポインター。  
  
 `pdwImplFlags`  
 入出力メンバーに関連付けられているメソッド実装フラグ。  
  
 `pdwCPlusTypeFlag`  
 入出力<xref:System.ValueType>をマークするフラグ。 これは `ELEMENT_TYPE_*` の値の1つです。
  
 `ppValue`  
 入出力このメンバーによって返される定数文字列値。  
  
 `pcchValue`  
 入出力`ppValue`の文字単位のサイズ。 `ppValue` が文字列を保持していない場合は0。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
