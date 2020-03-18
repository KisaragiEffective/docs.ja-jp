---
title: ICLRErrorReportingManager インターフェイス
ms.date: 03/30/2017
api_name:
- ICLRErrorReportingManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRErrorReportingManager
helpviewer_keywords:
- ICLRErrorReportingManager interface [.NET Framework hosting]
ms.assetid: ea8af0d5-4133-4472-8a1f-50570d7e85fa
topic_type:
- apiref
ms.openlocfilehash: 49a60b6b9b076138d8ff1f8a15041e9a6bacfede
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73129252"
---
# <a name="iclrerrorreportingmanager-interface"></a>ICLRErrorReportingManager インターフェイス
エラー報告のためにホストがカスタムスタックダンプを構成できるようにするメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[BeginCustomDump メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrerrorreportingmanager-begincustomdump-method.md)|エラー報告用のカスタムスタックダンプの構成を指定します。|  
|[EndCustomDump メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrerrorreportingmanager-endcustomdump-method.md)|`BeginCustomDump`の以前の呼び出しで設定されたカスタムスタックダンプ構成をクリアします。|  
|[GetBucketParametersForCurrentException メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrerrorreportingmanager-getbucketparametersforcurrentexception-method.md)|呼び出し元のスレッドで現在の例外の Watson バケットを取得します。|  
  
## <a name="remarks"></a>Remarks  
 `BeginCustomDump` メソッドは、カスタムスタックダンプ構成を設定します。 `EndCustomDump` メソッドは、カスタムスタックダンプ構成をクリアし、関連付けられているすべての状態を解放します。 カスタムダンプの完了後に呼び出す必要があります。  
  
> [!IMPORTANT]
> `EndCustomDump` を呼び出さないと、メモリがリークします。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ECustomDumpItemKind 列挙型](../../../../docs/framework/unmanaged-api/hosting/ecustomdumpitemkind-enumeration.md)
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
