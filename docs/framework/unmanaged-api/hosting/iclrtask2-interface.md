---
title: ICLRTask2 インターフェイス
ms.date: 03/30/2017
api_name:
- ICLRTask2
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRTask2
helpviewer_keywords:
- ICLRTask2 interface [.NET Framework hosting]
ms.assetid: b5a22ebc-0582-49de-91f9-97a3d9789290
topic_type:
- apiref
ms.openlocfilehash: 47c6dd9045636bcfbe07c909fec3fda515d28ee8
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73124526"
---
# <a name="iclrtask2-interface"></a>ICLRTask2 インターフェイス
には、 [ICLRTask](../../../../docs/framework/unmanaged-api/hosting/iclrtask-interface.md)インターフェイスのすべての機能が用意されています。また、には、現在のスレッドでスレッドの中止を遅延させることができるメソッドが用意されています。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[BeginPreventAsyncAbort メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask2-beginpreventasyncabort-method.md)|現在のスレッドの新しいスレッド中止要求を遅延します。|  
|[EndPreventAsyncAbort メソッド](../../../../docs/framework/unmanaged-api/hosting/iclrtask2-endpreventasyncabort-method.md)|新しいまたは保留中のスレッド中止要求に対して、現在のスレッドでのスレッドの中止を許可します。|  
  
## <a name="remarks"></a>Remarks  
 `ICLRTask2` インターフェイスは、`ICLRTask` インターフェイスを継承し、ホストがスレッドの中止を遅延させて、失敗しないコードの領域を保護できるようにするメソッドを追加します。 `BeginPreventAsyncAbort` を呼び出すと、現在のスレッドの遅延スレッド中止カウンターがインクリメントされ、`EndPreventAsyncAbort` を呼び出すとデクリメントされます。 `BeginPreventAsyncAbort` および `EndPreventAsyncAbort` の呼び出しは入れ子にすることができます。 カウンターがゼロより大きい限り、現在のスレッドのスレッド中止は遅延されます。  
  
 `BeginPreventAsyncAbort` と `EndPreventAsyncAbort` の呼び出しがペアになっていない場合、現在のスレッドにスレッド中止を配信できない状態になる可能性があります。  
  
 遅延は、それ自体を中止するスレッドには適用されません。  
  
 この機能によって公開されている機能は、仮想マシン (VM) によって内部的に使用されます。 これらの方法を誤用すると、VM で特定できない動作が発生する可能性があります。 たとえば、最初に `BeginPreventAsyncAbort` を呼び出さずに `EndPreventAsyncAbort` を呼び出すと、VM が以前にインクリメントしたときにカウンターを0に設定できます。 同様に、内部カウンターのオーバーフローはチェックされません。 ホストと VM の両方によってインクリメントされるために整数の制限を超えた場合、結果として得られる動作は指定されません。  
  
 `ICLRTask` から継承されたメンバーと、このインターフェイスの他の用途については、「 [ICLRTask](../../../../docs/framework/unmanaged-api/hosting/iclrtask-interface.md)インターフェイス」を参照してください。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** Mscoree.dll にリソースとして含まれています  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICLRTask インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtask-interface.md)
- [ICLRTaskManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/iclrtaskmanager-interface.md)
- [IHostTask インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihosttask-interface.md)
- [IHostTaskManager インターフェイス](../../../../docs/framework/unmanaged-api/hosting/ihosttaskmanager-interface.md)
- [ホスト インターフェイス](../../../../docs/framework/unmanaged-api/hosting/hosting-interfaces.md)
