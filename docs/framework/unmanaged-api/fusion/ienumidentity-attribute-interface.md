---
title: IEnumIDENTITY_ATTRIBUTE インターフェイス
ms.date: 03/30/2017
api_name:
- IEnumIDENTITY_ATTRIBUTE
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IEnumIDENTITY_ATTRIBUTE
helpviewer_keywords:
- IEnumIDENTITY_ATTRIBUTE interface [.NET Framework fusion]
ms.assetid: c2ec2748-e9ae-4e1b-80db-6fcec5cb81a1
topic_type:
- apiref
ms.openlocfilehash: 7e6bc57fb470a5c12549bb5f9445ecf1551425a4
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73107843"
---
# <a name="ienumidentity_attribute-interface"></a>IEnumIDENTITY_ATTRIBUTE インターフェイス
現在のスコープ内のコードオブジェクトの属性の列挙子として機能します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|`IEnumIDENTITY_ATTRIBUTE::Clone`|この `IEnumIDENTITY_ATTRIBUTE`と同じメンバーを含む新しい `IEnumIDENTITY_ATTRIBUTE` へのインターフェイスポインターを取得します。|  
|`IEnumIDENTITY_ATTRIBUTE::CurrentIntoBuffer`|この `IEnumIDENTITY_ATTRIBUTE` の要素に格納されているデータを、指定したデータバッファーに書き込みます。|  
|`IEnumIDENTITY_ATTRIBUTE::Next`|指定した数の属性を、現在の位置から開始して取得します。|  
|`IEnumIDENTITY_ATTRIBUTE::Reset`|命令ポインターをこの `IEnumIDENTITY_ATTRIBUTE`の先頭に移動します。|  
|`IEnumIDENTITY_ATTRIBUTE::Skip`|現在位置を開始位置として、指定した要素数だけ前方に命令ポインターを移動します。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** 分離 .h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [Fusion インターフェイス](fusion-interfaces.md)
