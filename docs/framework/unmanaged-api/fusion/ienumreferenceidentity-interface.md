---
title: IEnumReferenceIdentity インターフェイス
ms.date: 03/30/2017
api_name:
- IEnumReferenceIdentity
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IEnumReferenceIdentity
helpviewer_keywords:
- IEnumReferenceIdentity interface [.NET Framework fusion]
ms.assetid: a17b3155-7216-4e16-8c9f-abce21f549e7
topic_type:
- apiref
ms.openlocfilehash: 1305b9ebe3cd87ba002ee87610ff309d015a44e6
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73131748"
---
# <a name="ienumreferenceidentity-interface"></a>IEnumReferenceIdentity インターフェイス
`IReferenceIdentity` オブジェクトのコレクションの列挙子として機能します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|`IEnumReferenceIdentity::Clone`|この `IEnumReferenceIdentity`と同じメンバーを含む新しい `IEnumReferenceIdentity` へのインターフェイスポインターを取得します。|  
|`IEnumReferenceIdentity::Next`|現在の位置から開始して、指定した数の `IReferenceIdentity` オブジェクトを取得します。|  
|`IEnumReferenceIdentity::Reset`|命令ポインターをこの `IEnumReferenceIdentity`の先頭に移動します。|  
|`IEnumReferenceIdentity::Skip`|現在位置を開始位置として、指定した要素数だけ前方に命令ポインターを移動します。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** 分離 .h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [Fusion インターフェイス](fusion-interfaces.md)
- [IReferenceIdentity インターフェイス](ireferenceidentity-interface.md)
