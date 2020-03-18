---
title: EClrEvent 列挙型
ms.date: 03/30/2017
api_name:
- EClrEvent
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- EClrEvent
helpviewer_keywords:
- EClrEvent enumeration [.NET Framework hosting]
ms.assetid: 7c36a7c2-75a2-4971-bc23-abf54c812154
topic_type:
- apiref
ms.openlocfilehash: ee749fd40f440e92f1d1b09c2ea5e7bdd51f1cbe
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73131131"
---
# <a name="eclrevent-enumeration"></a>EClrEvent 列挙型
ホストがコールバックを登録できる共通言語ランタイム (CLR) イベントについて説明します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum {  
    Event_ClrDisabled,  
    Event_DomainUnload,  
    Event_MDAFired,  
    Event_StackOverflow  
} EClrEvent;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`Event_ClrDisabled`|致命的な CLR エラーを指定します。|  
|`Event_DomainUnload`|特定の <xref:System.AppDomain>のアンロードを指定します。|  
|`Event_MDAFired`|マネージデバッグアシスタント (MDA) メッセージが生成されたことを示します。|  
|`Event_StackOverflow`|スタックオーバーフローエラーが発生したことを示します。|  
  
## <a name="remarks"></a>Remarks  
 ホストは、 [ICLROnEventManager](../../../../docs/framework/unmanaged-api/hosting/iclroneventmanager-interface.md)インターフェイスのメソッドを呼び出すことによって、`EClrEvent` によって記述されたイベントの種類のコールバックを登録できます。 ホストは、 [ICLRControl:: GetCLRManager](../../../../docs/framework/unmanaged-api/hosting/iclrcontrol-getclrmanager-method.md)メソッドを呼び出すことによって、このインターフェイスへのポインターを取得します。  
  
 `Event_CLRDisabled` イベントと `Event_DomainUnload` イベントを複数回発生させることができ、さまざまなスレッドから、アンロードまたは CLR の無効化を通知することができます。  
  
 `Event_MDAFired` イベントは、MDA メッセージの詳細を含む[MDAInfo](../../../../docs/framework/unmanaged-api/hosting/mdainfo-structure.md)インスタンスの作成を発生させます。 Mda の詳細については、「[マネージデバッグアシスタントによるエラーの診断](../../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)」を参照してください。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [IActionOnCLREvent インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iactiononclrevent-interface.md)
- [ICLRControl インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrcontrol-interface.md)
- [ホスティングの列挙型](../../../../docs/framework/unmanaged-api/hosting/hosting-enumerations.md)
