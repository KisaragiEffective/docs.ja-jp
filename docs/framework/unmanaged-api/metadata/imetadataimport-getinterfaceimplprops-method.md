---
title: IMetaDataImport::GetInterfaceImplProps メソッド
ms.date: 02/25/2019
api_name:
- IMetaDataImport.GetInterfaceImplProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetInterfaceImplProps
helpviewer_keywords:
- IMetaDataImport::GetInterfaceImplProps method [.NET Framework metadata]
- GetInterfaceImpProps method [.NET Framework metadata]
ms.assetid: be3f5985-b1e4-4036-8602-c16e8508d4af
topic_type:
- apiref
ms.openlocfilehash: e5eb735acac73d694a0719c206bd22711a8c0333
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74437542"
---
# <a name="imetadataimportgetinterfaceimplprops-method"></a>IMetaDataImport::GetInterfaceImplProps メソッド
指定したメソッドを実装する <xref:System.Type> のメタデータトークンへのポインターと、そのメソッドを宣言するインターフェイスを取得します。
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetInterfaceImplProps (  
   [in]  mdInterfaceImpl        iiImpl,  
   [out] mdTypeDef              *pClass,  
   [out] mdToken                *ptkIface  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `iiImpl`  
 からクラスとインターフェイストークンを返すメソッドを表すメタデータトークン。  
  
 `pClass`  
 入出力メソッドを実装するクラスを表すメタデータトークン。  
  
 `ptkIface`  
 入出力実装されたメソッドを定義するインターフェイスを表すメタデータトークン。  

## <a name="remarks"></a>コメント

 `iImpl` の値を取得するには、 [EnumInterfaceImpls](imetadataimport-enuminterfaceimpls-method.md)メソッドを呼び出します。
 
 たとえば、クラスの `mdTypeDef` トークン値が0x02000007 で、型にトークンが含まれている3つのインターフェイスを実装しているとします。 

- 0x02000003 (TypeDef)
- 0x0100000A (TypeRef)
- 0x0200001C (TypeDef)

概念的には、この情報は次のようにインターフェイス実装テーブルに格納されます。

| 行番号 | クラストークン | インターフェイストークン |
|------------|-------------|-----------------|
| 4          |             |                 |
| 5          | 02000007    | 02000003        |
| 6          | 02000007    | 0100000A        |
| 7          |             |                 |
| 8          | 02000007    | 0200001C        |

これは、トークンが4バイトの値であることを思い出してください。

- 下位3バイトは、行番号 (RID) を保持します。
- 上位バイトは、`mdtInterfaceImpl`のトークンの種類 (0x09) を保持します。

`GetInterfaceImplProps` は、`iImpl` 引数で指定したトークンを持つ行に保持されている情報を返します。 
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IMetaDataImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
