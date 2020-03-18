---
title: IDefinitionIdentity インターフェイス
ms.date: 03/30/2017
api_name:
- IDefinitionIdentity
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IDefinitionIdentity
helpviewer_keywords:
- IDefinitionIdentity interface [.NET Framework fusion]
ms.assetid: ce5ba888-5fbe-4efd-91cf-f0ff94d8428b
topic_type:
- apiref
ms.openlocfilehash: 59578e1d3a66809c86f7daad1b208df2ae09568d
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73108039"
---
# <a name="idefinitionidentity-interface"></a>IDefinitionIdentity インターフェイス
現在のスコープ内のアプリケーションを定義するコードの一意の署名を表します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|`IDefinitionIdentity::Clone`|指定した属性の変更を除き、この `IDefinitionIdentity`と同一の新しい `IDefinitionIdentity` オブジェクトへのインターフェイスポインターを取得します。|  
|`IDefinitionIdentity::EnumAttributes`|この `IDefinitionIdentity`に関連付けられている属性を格納している[IEnumIDENTITY_ATTRIBUTE](ienumidentity-attribute-interface.md)オブジェクトへのインターフェイスポインターを取得します。|  
|`IDefinitionIdentity::GetAttribute`|指定した名前空間内の指定した名前の属性の値を取得します。|  
|`IDefinitionIdentity::SetAttribute`|指定した名前空間の指定した名前を持つ属性を、指定した値に設定します。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** 分離 .h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [Fusion インターフェイス](fusion-interfaces.md)
