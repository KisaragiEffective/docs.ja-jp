---
title: IMetaDataEmit::SetClassLayout メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.SetClassLayout
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::SetClassLayout
helpviewer_keywords:
- IMetaDataEmit::SetClassLayout method [.NET Framework metadata]
- SetClassLayout method [.NET Framework metadata]
ms.assetid: 2576c449-388d-4434-a0e1-9f53991e11b6
topic_type:
- apiref
ms.openlocfilehash: 5214298c6ad9594548ab45ed583cb5b14ce1f30d
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74441767"
---
# <a name="imetadataemitsetclasslayout-method"></a>IMetaDataEmit::SetClassLayout メソッド
以前に呼び出した[Typedef メソッド](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definetypedef-method.md)の呼び出しで定義されたクラスのフィールドのレイアウトを完了します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetClassLayout (  
    [in]  mdTypeDef           td,   
    [in]  DWORD               dwPackSize,   
    [in]  COR_FIELD_OFFSET    rFieldOffsets[],   
    [in]  ULONG               ulClassSize   
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `td`  
 からレイアウトするクラスを指定する `mdTypeDef` トークン。  
  
 `dwPackSize`  
 からパッキングサイズは1、2、4、8、または16バイトです。 パッキングサイズは、隣接するフィールド間のバイト数です。  
  
 `rFieldOffsets`  
 から[COR_FIELD_OFFSET](../../../../docs/framework/unmanaged-api/metadata/cor-field-offset-structure.md)構造体の配列。各構造体は、クラスのフィールドと、クラス内のフィールドのオフセットを指定します。 `mdTokenNil`で配列を終了します。  
  
 `ulClassSize`  
 からクラスのサイズ (バイト単位)。  
  
## <a name="remarks"></a>コメント  
 クラスを最初に定義するには、 [IMetaDataEmit::D efineTypeDef](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-definetypedef-method.md)メソッドを呼び出し、クラスのフィールドに対して3つのレイアウト (automatic、シーケンシャル、explicit) のいずれかを指定します。 通常は、自動レイアウトを使用し、フィールドをレイアウトする最適な方法をランタイムが選択できるようにします。  
  
 ただし、アンマネージコードが使用する配置に従って、フィールドをレイアウトすることが必要になる場合があります。 この場合は、シーケンシャルまたは明示的なレイアウトを選択し `SetClassLayout` を呼び出して、フィールドのレイアウトを完成させます。  
  
- シーケンシャルレイアウト: パッキングサイズを指定します。 フィールドは、自然サイズまたはパッキングサイズのいずれかに従って整列されます。どちらの場合も、フィールドのオフセットは小さくなります。 `rFieldOffsets` と `ulClassSize` を0に設定します。  
  
- 明示的なレイアウト: 各フィールドのオフセットを指定するか、クラスのサイズとパッキングサイズを指定します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll のリソースとして使用されます。  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataEmit インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)
- [IMetaDataEmit2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-interface.md)
