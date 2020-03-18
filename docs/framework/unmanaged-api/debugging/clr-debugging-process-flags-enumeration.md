---
title: CLR_DEBUGGING_PROCESS_FLAGS 列挙体
ms.date: 03/30/2017
api_name:
- CLR_DEBUGGING_PROCESS_FLAGS
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CLR_DEBUGGING_PROCESS_FLAG
helpviewer_keywords:
- CLR_DEBUGGING_PROCESS_FLAGS enumeration [.NET Framework debugging]
ms.assetid: 85b85fde-1f87-490b-ba8d-d604670959c3
topic_type:
- apiref
ms.openlocfilehash: b9185600d9d8b2a33830d86642727ac54b87a9cf
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73099657"
---
# <a name="clr_debugging_process_flags-enumeration"></a>CLR_DEBUGGING_PROCESS_FLAGS 列挙体
[ICLRDebugging:: OpenVirtualProcess](iclrdebugging-openvirtualprocess-method.md)メソッドによって使用される値を提供します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CLR_DEBUGGING_PROCESS_FLAGS  
{  
   CLR_DEBUGGING_MANAGED_EVENT_PENDING = 1,  
   CLR_DEBUGGING_MANAGED_EVENT_DEBUGGER_LAUNCH = 2  
}  CLR_DEBUGGING_PROCESS_FLAGS;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`CLR_DEBUGGING_MANAGED_EVENT_PENDING`|このランタイムには、送信するキャッチアップマネージデバッガーイベントがありません。 キャッチアップイベントと非キャッチアップイベントの違いについては、「解説」を参照してください。|  
|`CLR_DEBUGGING_MANAGED_EVENT_DEBUGGER_LAUNCH`|保留中のマネージイベントは <xref:System.Diagnostics.Debugger.Launch%2A?displayProperty=nameWithType> 要求です。|  
  
## <a name="remarks"></a>Remarks  
 キャッチアップイベントには、プロセスにアタッチした後にデバッガーが現在の状態になるようにするプロセス、アプリケーションドメイン、アセンブリ、モジュール、スレッド作成通知が含まれます。 `CLR_DEBUGGING_MANAGED_EVENT_PENDING` フラグによって示されるキャッチアップ以外のイベントには、例外やマネージデバッグアシスタント (MDA) 通知など、他のすべてのデバッガーイベントが含まれます。  
  
 `CLR_DEBUGGING_MANAGED_EVENT_DEBUGGER_LAUNCH` フラグにより、ランタイムは、終了した例外と、キャンセル可能なマネージデバッガーをアタッチする要求を区別できます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** メタホスト .idl、メタホスト .h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [列挙型のデバッグ](debugging-enumerations.md)
- [デバッグ](index.md)
