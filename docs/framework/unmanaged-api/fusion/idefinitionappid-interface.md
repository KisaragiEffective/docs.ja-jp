---
title: IDefinitionAppId インターフェイス
ms.date: 03/30/2017
api_name:
- IDefinitionAppId
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IDefinitionAppId
helpviewer_keywords:
- IDefinitionAppId interface [.NET Framework fusion]
ms.assetid: e72f2550-bdec-4a20-a2f4-2e14847266c1
topic_type:
- apiref
ms.openlocfilehash: 5114f74e80da925c7a153b9e481c54067152eaec
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73108200"
---
# <a name="idefinitionappid-interface"></a>IDefinitionAppId インターフェイス
現在のスコープ内のアプリケーションを定義するコードの一意の識別子を表します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|`IDefinitionAppId::get_Codebase`|この `IDefinitionAppId` オブジェクトのコードを表す書式設定された文字列を取得します。|  
|`IDefinitionAppId::put_Codebase`|この `IDefinitionAppId` オブジェクトのコードを、指定した書式設定された文字列値に設定します。|  
|`IDefinitionAppId::EnumAppPath`|現在のアプリケーションパス内のアセンブリを格納する[IEnumDefinitionIdentity](ienumdefinitionidentity-interface.md)オブジェクトへのインターフェイスポインターを取得します。|  
|`IDefinitionAppId::SetAppPath`|現在のスコープ内のアセンブリのアプリケーションパスを、指定した[IDefinitionIdentity](idefinitionidentity-interface.md)オブジェクトによって参照される値に設定します。|  
|`IDefinitionAppId::get_SubscriptionId`|この `IDefinitionAppId` オブジェクトに対するサブスクリプションのトークン識別子の文字列形式へのポインターを取得します。|  
|`IDefinitionAppId::put_SubscriptionId`|この `IDefinitionAppId` オブジェクトに対するサブスクリプションのトークン識別子を、指定された文字列値に設定します。|  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** 分離 .h  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [Fusion インターフェイス](fusion-interfaces.md)
