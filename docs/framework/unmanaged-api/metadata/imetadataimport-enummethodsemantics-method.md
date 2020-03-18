---
title: IMetaDataImport::EnumMethodSemantics メソッド
ms.date: 03/30/2017
api_name:
- IMetaDataImport.EnumMethodSemantics
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::EnumMethodSemantics
helpviewer_keywords:
- EnumMethodSemantics method [.NET Framework metadata]
- IMetaDataImport::EnumMethodSemantics method [.NET Framework metadata]
ms.assetid: e7e3c630-9691-46d6-94df-b5593a7bb08a
topic_type:
- apiref
ms.openlocfilehash: ff6932b6040a19e0ccda2f8d2140fa131cdd9224
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74450076"
---
# <a name="imetadataimportenummethodsemantics-method"></a>IMetaDataImport::EnumMethodSemantics メソッド
指定したメソッドが関連付けられているプロパティおよびプロパティ変更イベントを列挙します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EnumMethodSemantics (  
   [in, out] HCORENUM    *phEnum,  
   [in]  mdMethodDef     mb,   
   [out] mdToken         rEventProp[],  
   [in]  ULONG           cMax,  
   [out] ULONG           *pcEventProp  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `phEnum`  
 [入力、出力]列挙子へのポインター。 このメソッドの最初の呼び出しでは、この値は NULL である必要があります。  
  
 `mb`  
 から列挙型のスコープを制限する MethodDef トークン。  
  
 `rEventProp`  
 入出力イベントまたはプロパティを格納するために使用される配列。  
  
 `cMax`  
 [in] `rEventProp` 配列の最大サイズ。  
  
 `pcEventProp`  
 入出力`rEventProp`で返されるイベントまたはプロパティの数。  
  
## <a name="return-value"></a>戻り値  
  
|HRESULT|説明|  
|-------------|-----------------|  
|`S_OK`|`EnumMethodSemantics` が正常に返されました。|  
|`S_FALSE`|列挙するイベントやプロパティはありません。 この場合、`pcEventProp` は0になります。|  
  
## <a name="remarks"></a>コメント  
 多くの共通言語ランタイム型では、プロパティ`Changed` イベント *、`On`プロパティ*に関連*するプロパティ`Changed`* メソッドが定義されています。 たとえば、<xref:System.Windows.Forms.Control?displayProperty=nameWithType> 型は、<xref:System.Windows.Forms.Control.Font%2A> プロパティ、<xref:System.Windows.Forms.Control.FontChanged> イベント、および <xref:System.Windows.Forms.Control.OnFontChanged%2A> メソッドを定義します。 <xref:System.Windows.Forms.Control.Font%2A> プロパティの set アクセサーメソッドは <xref:System.Windows.Forms.Control.OnFontChanged%2A> メソッドを呼び出します。このメソッドによって、<xref:System.Windows.Forms.Control.FontChanged> イベントが発生します。 <xref:System.Windows.Forms.Control.OnFontChanged%2A> に対して MethodDef を使用して `EnumMethodSemantics` を呼び出し、<xref:System.Windows.Forms.Control.Font%2A> プロパティと <xref:System.Windows.Forms.Control.FontChanged> イベントへの参照を取得します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Cor  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>参照

- [IMetaDataImport インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 インターフェイス](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
