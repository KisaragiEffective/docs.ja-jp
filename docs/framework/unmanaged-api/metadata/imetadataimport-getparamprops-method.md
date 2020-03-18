---
title: IMetaDataImport::GetParamProps メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetParamProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetParamProps
helpviewer_keywords:
- IMetaDataImport::GetParamProps method [.NET Framework metadata]
- GetParamProps method [.NET Framework metadata]
ms.assetid: 4d5e5f00-bcab-4f41-b191-176511a186a7
topic_type:
- apiref
ms.openlocfilehash: bb73ccdd9eee4b5a655a56b5d6757e0c6003fbc9
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74437125"
---
# <a name="imetadataimportgetparamprops-method"></a>IMetaDataImport::GetParamProps メソッド
指定した ParamDef トークンによって参照されるパラメーターのメタデータ値を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetParamProps (  
   [in]  mdParamDef      tk,  
   [out] mdMethodDef     *pmd,  
   [out] ULONG           *pulSequence,  
   [out] LPWSTR          szName,  
   [in]  ULONG           cchName,  
   [out] ULONG           *pchName,  
   [out] DWORD           *pdwAttr,  
   [out] DWORD           *pdwCPlusTypeFlag,  
   [out] UVCP_CONSTANT   *ppValue,  
   [out] ULONG           *pcchValue  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `tk`  
 からメタデータを返すパラメーターを表す ParamDef トークン。  
  
 `pmd`  
 入出力パラメーターを受け取るメソッドを表す MethodDef トークンへのポインター。  
  
 `pulSequence`  
 入出力メソッド引数リスト内のパラメーターの序数位置。  
  
 `szName`  
 入出力パラメーターの名前を保持するバッファー。  
  
 `cchName`  
 から`szName`のワイド文字で要求されたサイズ。  
  
 `pchName`  
 入出力`szName`のワイド文字で返されたサイズ。  
  
 `pdwAttr`  
 入出力パラメーターに関連付けられているすべての属性フラグへのポインター。 これは `CorParamAttr` 値のビットマスクです。  
  
 `pdwCPlusTypeFlag`  
 入出力パラメーターが <xref:System.ValueType>であることを示すフラグへのポインター。  
  
 `ppValue`  
 入出力パラメーターによって返される定数文字列へのポインター。  
  
 `pcchValue`  
 入出力ワイド文字の `ppValue` のサイズ。 `ppValue` が文字列を保持していない場合は0。  
  
## <a name="remarks"></a>コメント

`pulSequence` のシーケンス値は、パラメーターに対して1から始まります。 戻り値のシーケンス番号は0です。

## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
