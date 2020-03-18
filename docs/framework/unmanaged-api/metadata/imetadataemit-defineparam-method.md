---
title: IMetaDataEmit::DefineParam メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.DefineParam
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::DefineParam
helpviewer_keywords:
- IMetaDataEmit::DefineParam method [.NET Framework metadata]
- DefineParam method [.NET Framework metadata]
ms.assetid: d86a3d14-4796-4909-9591-dfafe3de5ce4
topic_type:
- apiref
ms.openlocfilehash: 5c81bc82e19bce658336e4860a61f2721e17423d
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74431692"
---
# <a name="imetadataemitdefineparam-method"></a>IMetaDataEmit::DefineParam メソッド
指定したトークンによって参照されるメソッドに対して、指定したシグネチャを持つパラメーター定義を作成し、そのパラメーター定義のトークンを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT DefineParam (  
    [in]  mdMethodDef md,   
    [in]  ULONG       ulParamSeq,   
    [in]  LPCWSTR     szName,   
    [in]  DWORD       dwParamFlags,   
    [in]  DWORD       dwCPlusTypeFlag,   
    [in]  void const  *pValue,  
    [in]  ULONG       cchValue,   
    [out] mdParamDef  *ppd   
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `md`  
 からパラメーターが定義されているメソッドのトークン。  
  
 `ulParamSeq`  
 からパラメーターのシーケンス番号。  
  
 `szName`  
 からUnicode でのパラメーターの名前。  
  
 `dwParamFlags`  
 からパラメーターのフラグ。 これは `CorParamAttr` 値のビットマスクです。  
  
 `dwCPlusTypeFlag`  
 [in] 定数値の *\** を `ELEMENT_TYPE_`します。  
  
 `pValue`  
 からパラメーターの定数値。  
  
 `cchValue`  
 から`pValue`のサイズ (Unicode 文字)。  
  
 `ppd`  
 入出力割り当てられた `mdParamDef` トークン。  
  
## <a name="remarks"></a>コメント  
 `ulParamSeq` のシーケンス値は、パラメーターに対して1から始まります。 戻り値のシーケンス番号は0です。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IMetaDataEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)
- [IMetaDataEmit2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-interface.md)
