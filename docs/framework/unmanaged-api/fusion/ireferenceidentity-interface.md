---
title: IReferenceIdentity インターフェイス
ms.date: 03/30/2017
api_name:
- IReferenceIdentity
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IReferenceIdentity
helpviewer_keywords:
- IReferenceIdentity interface [.NET Framework fusion]
ms.assetid: 9180ac5a-7019-4716-9f83-8a91d157239a
topic_type:
- apiref
ms.openlocfilehash: 8f6a117d1e2fe76c271b0b014e6079370c8b4fe4
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127060"
---
# <a name="ireferenceidentity-interface"></a>IReferenceIdentity インターフェイス
コードオブジェクトの一意の署名への参照を表します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|`IReferenceIdentity::Clone`|指定した属性の変更を除き、この `IReferenceIdentity`と同一の新しい `IReferenceIdentity` インスタンスへのインターフェイスポインターを取得します。|  
|`IReferenceIdentity::EnumAttributes`|この `IReferenceIdentity`に関連付けられている属性を格納している `IEnumIDENTITY_ATTRIBUTE` インスタンスへのインターフェイスポインターを取得します。|  
|`IReferenceIdentity::GetAttribute`|指定した名前を使用して、指定した名前空間の属性の値を取得します。|  
|`IReferenceIdentity::SetAttribute`|指定した名前空間と指定した名前を持つ属性を、指定した値に設定します。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** 分離 .h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [Fusion インターフェイス](fusion-interfaces.md)
- [IEnumIDENTITY_ATTRIBUTE インターフェイス](ienumidentity-attribute-interface.md)
