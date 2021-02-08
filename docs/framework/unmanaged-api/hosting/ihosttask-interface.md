---
description: ': IHostTask インターフェイスの詳細情報'
title: IHostTask インターフェイス
ms.date: 03/30/2017
api_name:
- IHostTask
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTask
helpviewer_keywords:
- IHostTask interface [.NET Framework hosting]
ms.assetid: a71dbbd5-64b8-47eb-9f03-8e8c85fbe2bc
topic_type:
- apiref
ms.openlocfilehash: c46bbdd2e881c20d1ffd634bec8ddfa3b70b0f82
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99784666"
---
# <a name="ihosttask-interface"></a>IHostTask インターフェイス

共通言語ランタイム (CLR) がホストと通信してタスクを管理できるようにするメソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[Alert メソッド](ihosttask-alert-method.md)|現在のインスタンスによって表されるタスクをホストがスリープ解除する `IHostTask` ように要求します。これにより、タスクを中止できます。|  
|[GetPriority メソッド](ihosttask-getpriority-method.md)|現在のインスタンスによって表されるタスクのスレッド優先度レベルを取得し `IHostTask` ます。|  
|[Join メソッド](ihosttask-join-method.md)|現在のインスタンスによって表されるタスクが `IHostTask` 完了するか、指定された時間が経過するか、または [IHostTask:: Alert](ihosttask-alert-method.md) が呼び出されるまで、呼び出し元のタスクをブロックします。|  
|[SetCLRTask メソッド](ihosttask-setclrtask-method.md)|[ICLRTask インターフェイス](iclrtask-interface.md)インスタンスを現在のインスタンスに関連付け `IHostTask` ます。|  
|[SetPriority メソッド](ihosttask-setpriority-method.md)|現在のインスタンスによって表されるタスクのスレッドの優先度レベルをホストが調整するように要求 `IHostTask` します。|  
|[Start メソッド](ihosttask-start-method.md)|現在のインスタンスによって表されるタスクを中断状態からライブ状態に移行するようホストに要求し `IHostTask` ます。このとき、コードを実行できます。|  
  
## <a name="remarks"></a>解説  

 CLR は、によって定義されたメソッドを呼び出して、 `IHostTask` タスクを開始したり、スレッドの優先度レベルを設定したりします。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** MSCorEE.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRTask インターフェイス](iclrtask-interface.md)
- [ICLRTaskManager インターフェイス](iclrtaskmanager-interface.md)
- [IHostTaskManager インターフェイス](ihosttaskmanager-interface.md)
- [ホスト インターフェイス](hosting-interfaces.md)
