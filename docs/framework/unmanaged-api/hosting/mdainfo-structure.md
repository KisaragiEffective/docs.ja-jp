---
title: MDAInfo 構造体
ms.date: 03/30/2017
api_name:
- MDAInfo
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- MDAInfo
helpviewer_keywords:
- MDAInfo structure [.NET Framework hosting]
ms.assetid: fb8c14f7-d461-43d1-8b47-adb6723b9b93
topic_type:
- apiref
ms.openlocfilehash: 9a2f513d40d722f1b0aad823ac7c0d93bda5615f
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73123259"
---
# <a name="mdainfo-structure"></a>MDAInfo 構造体
マネージデバッグアシスタント (MDA) の作成をトリガーする `Event_MDAFired` イベントについて詳しく説明します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef struct _MDAInfo {  
    LPCWSTR  lpMDACaption;  
    LPCWSTR  lpMDAMessage  
} MDAInfo;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`lpMDACaption`|現在の MDA のタイトル。 タイトルには、`Event_MDAFired` イベントをトリガーしたエラーの種類が示されます。|  
|`lpMDAMessage`|現在の MDA によって提供される出力メッセージ。|  
  
## <a name="remarks"></a>Remarks  
 マネージデバッグアシスタント (Mda) は、共通言語ランタイム (CLR) と連携して動作し、ランタイム実行エンジンで無効な条件を特定したり、の状態に関する追加情報をダンプしたりするなどのタスクを実行します。双発. Mda は、トラップが困難なイベントに関する XML メッセージを生成します。 これらは、マネージコードとアンマネージコードの間の遷移をデバッグする場合に特に便利です。  
  
 MDA の作成をトリガーするイベントが発生した場合、ランタイムは次の手順を実行します。  
  
- ホストが[ICLROnEventManager:: RegisterActionOnEvent](../../../../docs/framework/unmanaged-api/hosting/iclroneventmanager-registeractiononevent-method.md)を呼び出して[Iactiononclrevent](../../../../docs/framework/unmanaged-api/hosting/iactiononclrevent-interface.md)インスタンスを登録していない場合に、`Event_MDAFired` イベントが通知されるようにするには、ランタイムは既定のホストされていない動作に進みます。  
  
- ホストがこのイベントのハンドラーを登録している場合、ランタイムはデバッガーがプロセスにアタッチされているかどうかを確認します。 存在する場合、ランタイムはデバッガーに中断します。 デバッガーが続行されると、ホストが呼び出されます。 デバッガーがアタッチされていない場合、ランタイムは `IActionOnCLREvent::OnEvent` を呼び出し、`data` パラメーターとして `MDAInfo` インスタンスへのポインターを渡します。  
  
 ホストは mda をアクティブ化し、MDA がアクティブになったときに通知を受け取ることができます。 これにより、ホストは、既定の動作をオーバーライドし、イベントを発生させたマネージスレッドを中止して、プロセスの状態が破損するのを防ぐことができます。 Mda の使用方法の詳細については、「[マネージデバッグアシスタントによるエラーの診断](../../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)」を参照してください。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ホスト構造体](../../../../docs/framework/unmanaged-api/hosting/hosting-structures.md)
- [マネージド デバッグ アシスタントによるエラーの診断](../../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)
