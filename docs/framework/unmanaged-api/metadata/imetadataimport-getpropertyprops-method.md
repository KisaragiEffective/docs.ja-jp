---
title: IMetaDataImport::GetPropertyProps メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetPropertyProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetPropertyProps
helpviewer_keywords:
- GetPropertyProps method [.NET Framework metadata]
- IMetaDataImport::GetPropertyProps method [.NET Framework metadata]
ms.assetid: dc0ff3e6-7e7d-4f6c-948d-52b28f5cb78c
topic_type:
- apiref
ms.openlocfilehash: 247a2793bf3806f5ee38585d50b4535820dfcb69
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74437062"
---
# <a name="imetadataimportgetpropertyprops-method"></a>IMetaDataImport::GetPropertyProps メソッド
指定したトークンによって表されるプロパティのメタデータを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetPropertyProps (  
   [in]  mdProperty        prop,  
   [out] mdTypeDef         *pClass,   
   [out] LPCWSTR           szProperty,   
   [in]  ULONG             cchProperty,   
   [out] ULONG             *pchProperty,   
   [out] DWORD             *pdwPropFlags,   
   [out] PCCOR_SIGNATURE   *ppvSig,   
   [out] ULONG             *pbSig,   
   [out] DWORD             *pdwCPlusTypeFlag,   
   [out] UVCP_CONSTANT     *ppDefaultValue,  
   [out] ULONG             *pcchDefaultValue,  
   [out] mdMethodDef       *pmdSetter,   
   [out] mdMethodDef       *pmdGetter,   
   [out] mdMethodDef       rmdOtherMethod[],  
   [in]  ULONG             cMax,   
   [out] ULONG             *pcOtherMethod   
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `prop`  
 からメタデータを返すプロパティを表すトークン。  
  
 `pClass`  
 入出力プロパティを実装する型を表す TypeDef トークンへのポインター。  
  
 `szProperty`  
 入出力プロパティ名を保持するバッファー。  
  
 `cchProperty`  
 から`szProperty`のワイド文字単位のサイズ。  
  
 `pchProperty`  
 入出力`szProperty`に返されるワイド文字数。  
  
 `pdwPropFlags`  
 入出力プロパティに適用されている属性フラグへのポインター。 この値は、 [Corpropertyattr](../../../../docs/framework/unmanaged-api/metadata/corpropertyattr-enumeration.md)列挙子のビットマスクです。  
  
 `ppvSig`  
 入出力プロパティのメタデータシグネチャへのポインター。  
  
 `pbSig`  
 入出力`ppvSig`で返されたバイト数。  
  
 `pdwCPlusTypeFlag`  
 入出力プロパティの既定値である定数の型を指定するフラグ。 この値は CorElementType 列挙子からのものです。  
  
 `ppDefaultValue`  
 入出力このプロパティの既定値を格納するバイトへのポインター。  
  
 `pcchDefaultValue`  
 入出力`pdwCPlusTypeFlag` が ELEMENT_TYPE_STRING 場合、`ppDefaultValue`のワイド文字のサイズ。それ以外の場合、この値は関係ありません。 この場合、`ppDefaultValue` の長さは、`pdwCPlusTypeFlag`によって指定された型から推論されます。  
  
 `pmdSetter`  
 入出力プロパティの set アクセサーメソッドを表す MethodDef トークンへのポインター。  
  
 `pmdGetter`  
 入出力プロパティの get アクセサーメソッドを表す MethodDef トークンへのポインター。  
  
 `rmdOtherMethod`  
 入出力プロパティに関連付けられている他のメソッドを表す MethodDef トークンの配列。  
  
 `cMax`  
 [in] `rmdOtherMethod` 配列の最大サイズ。 すべてのメソッドを保持するのに十分な大きさの配列を指定しなかった場合、警告なしにスキップされます。  
  
 `pcOtherMethod`  
 入出力`rmdOtherMethod`で返される MethodDef トークンの数。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IMetaDataImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
