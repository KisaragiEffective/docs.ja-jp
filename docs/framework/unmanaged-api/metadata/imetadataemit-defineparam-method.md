---
description: '詳細について: IMetaDataEmit::D efineParam メソッド'
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
ms.openlocfilehash: 300de3183b329773a8e6813d6b92c6d049d63195
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99784094"
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
 からパラメーターのフラグ。 これは、値のビットマスクです `CorParamAttr` 。  
  
 `dwCPlusTypeFlag`  
 [入力] `ELEMENT_TYPE_` *\** 定数値の。  
  
 `pValue`  
 からパラメーターの定数値。  
  
 `cchValue`  
 からのサイズ (Unicode 文字) `pValue` 。  
  
 `ppd`  
 入出力 `mdParamDef` 割り当てられたトークン。  
  
## <a name="remarks"></a>解説  

 パラメーターの場合、のシーケンス値は `ulParamSeq` 1 から始まります。 戻り値のシーケンス番号は0です。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** MSCorEE.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IMetaDataEmit インターフェイス](imetadataemit-interface.md)
- [IMetaDataEmit2 インターフェイス](imetadataemit2-interface.md)
