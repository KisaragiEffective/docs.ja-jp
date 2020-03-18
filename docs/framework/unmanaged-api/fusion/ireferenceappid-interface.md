---
title: IReferenceAppId インターフェイス
ms.date: 03/30/2017
api_name:
- IReferenceAppId
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IReferenceAppId
helpviewer_keywords:
- IReferenceAppId interface [.NET Framework fusion]
ms.assetid: 8eb9e565-f358-43ce-900e-a8f8a5aa6cfb
topic_type:
- apiref
ms.openlocfilehash: 6f20fb2e9e026253fb02b47dfcd63cf655acc4ee
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73131657"
---
# <a name="ireferenceappid-interface"></a>IReferenceAppId インターフェイス
現在のスコープ内のアプリケーションの一意の識別子への参照を表します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|`IReferenceAppId::get_CodeBase`|この `IReferenceAppId`によって参照されるアプリケーションのコード識別子の文字列形式へのポインターを取得します。|  
|`IReferenceAppId::put_CodeBase`|この `IReferenceAppId`によって参照されるアプリケーションのコード識別子を設定します。|  
|`IReferenceAppId::EnumAppPath`|この `IReferenceAppId`のメンバーを表す `IReferenceIdentity` インスタンスを格納している `IEnumReferenceIdentity` インスタンスへのインターフェイスポインターを取得します。|  
|`IReferenceAppId::get_SubscriptionId`|この `IReferenceAppId`へのサブスクリプションのトークン識別子の文字列形式へのポインターを取得します。|  
|`IReferenceAppId::put_SubscriptionId`|この `IReferenceAppId` に対するサブスクリプションのトークン識別子を、指定された文字列値に設定します。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** 分離 .h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [Fusion インターフェイス](fusion-interfaces.md)
- [IEnumReferenceIdentity インターフェイス](ienumreferenceidentity-interface.md)
- [IReferenceIdentity インターフェイス](ireferenceidentity-interface.md)
