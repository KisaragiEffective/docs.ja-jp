---
title: IMetaDataImport::EnumInterfaceImpls メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumInterfaceImpls
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumInterfaceImpls
helpviewer_keywords:
- IMetaDataImport::EnumInterfaceImpls method [.NET Framework metadata]
- EnumInterfaceImpls method [.NET Framework metadata]
ms.assetid: ba6e178f-128b-4e47-a13c-b4be73eb106c
topic_type:
- apiref
ms.openlocfilehash: ef7057ad19fd34750bd15d358e9c1ebb1289cd44
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75338061"
---
# <a name="imetadataimportenuminterfaceimpls-method"></a>IMetaDataImport::EnumInterfaceImpls メソッド
指定した `TypeDef`によって実装されているすべてのインターフェイスを列挙します。 
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EnumInterfaceImpls (  
   [in, out]  HCORENUM       *phEnum,   
   [in]   mdTypeDef          td,  
   [out]  mdInterfaceImpl    rImpls[],   
   [in]   ULONG              cMax,  
   [out]  ULONG*             pcImpls  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `phEnum`  
 [入力、出力]列挙子へのポインター。  
  
 `td`  
 からインターフェイスの実装を表す MethodDef トークンを列挙する TypeDef のトークン。  
  
 `rImpls`  
 入出力MethodDef トークンを格納するために使用される配列。  
  
 `cMax`  
 から`rImpls` 配列の最大長。  
  
 `pcImpls`  
 入出力`rImpls`で返されたトークンの実際の数。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|`S_OK`|`EnumInterfaceImpls` が正常に返されました。|  
|`S_FALSE`|列挙する MethodDef トークンがありません。 この場合、`pcImpls` は0に設定されます。|  

## <a name="remarks"></a>コメント

列挙体は、指定した `TypeDef`によって実装された各インターフェイスの `mdInterfaceImpl` トークンのコレクションを返します。 インターフェイストークンは、インターフェイスが指定された順序で返されます (`DefineTypeDef` または `SetTypeDefProps`)。 返された `mdInterfaceImpl` トークンのプロパティは、 [GetInterfaceImplProps](imetadataimport-getinterfaceimplprops-method.md)を使用して照会できます。
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IMetaDataImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
