---
title: IMetaDataEmit::DefineProperty メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.DefineProperty
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::DefineProperty
helpviewer_keywords:
- IMetaDataEmit::DefineProperty method [.NET Framework metadata]
- DefineProperty method [.NET Framework metadata]
ms.assetid: 5c4c1dc2-d40d-4173-bbe6-7058fb21c98f
topic_type:
- apiref
ms.openlocfilehash: f11b374ed0ecbfc137c43fb641ae691237604691
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74431525"
---
# <a name="imetadataemitdefineproperty-method"></a>IMetaDataEmit::DefineProperty メソッド
指定した `get` および `set` メソッドアクセサーを使用して、指定した型のプロパティ定義を作成し、そのプロパティ定義へのトークンを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT DefineProperty (   
    [in]  mdTypeDef          td,   
    [in]  LPCWSTR            szProperty,   
    [in]  DWORD              dwPropFlags,   
    [in]  PCCOR_SIGNATURE    pvSig,   
    [in]  ULONG              cbSig,   
    [in]  DWORD              dwCPlusTypeFlag,   
    [in]  void const         *pValue,   
    [in]  ULONG              cchValue,   
    [in]  mdMethodDef        mdSetter,   
    [in]  mdMethodDef        mdGetter,   
    [in]  mdMethodDef        rmdOtherMethods[],   
    [out] mdProperty         *pmdProp   
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `td`  
 からプロパティが定義されているクラスまたはインターフェイスのトークン。  
  
 `szProperty`  
 からプロパティの名前。  
  
 `dwPropFlags`  
 からプロパティフラグ。  
  
 `pvSig`  
 からプロパティシグネチャ。  
  
 `cbSig`  
 から`pvSig`内のバイト数。  
  
 `dwCPlusTypeFlag`  
 からプロパティの既定値の型。  
  
 `pValue`  
 からプロパティの既定値。  
  
 `cchValue`  
 から`pValue`内の (Unicode) 文字の数。  
  
 `mdSetter`  
 からプロパティ値を設定するメソッド。  
  
 `mdGetter`  
 からプロパティ値を取得するメソッド。  
  
 `rmdOtherMethods[]`  
 からプロパティに関連付けられている他のメソッドの配列。 `mdTokenNil`で配列を終了します。  
  
 `pmdProp`  
 入出力割り当てられた `mdProperty` トークン。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)
- [IMetaDataEmit2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-interface.md)
