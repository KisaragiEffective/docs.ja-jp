---
title: ICorDebugProcess インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugProcess
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess
helpviewer_keywords:
- ICorDebugProcess interface [.NET Framework debugging]
ms.assetid: be86f4b5-418a-4c5c-a67c-97148c65ed8c
topic_type:
- apiref
ms.openlocfilehash: b2429052173a187297b67c756213e5d27a79298b
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76792601"
---
# <a name="icordebugprocess-interface"></a>ICorDebugProcess インターフェイス
マネージド コードを実行しているプロセスを表します。 このインターフェイスは、というコントロールのサブクラスです。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[ClearCurrentException メソッド](icordebugprocess-clearcurrentexception-method.md)|指定されたスレッドで現在のアンマネージ例外をクリアします。|  
|[EnableLogMessages メソッド](icordebugprocess-enablelogmessages-method.md)|デバッガーへのログメッセージの送信を有効または無効にします。|  
|[EnumerateAppDomains メソッド](icordebugprocess-enumerateappdomains-method.md)|プロセス内のすべてのアプリケーションドメインを列挙します。|  
|[EnumerateObjects メソッド](icordebugprocess-enumerateobjects-method.md)|実装されていません。|  
|[GetHandle メソッド](icordebugprocess-gethandle-method.md)|プロセスを処理するハンドルを取得します。|  
|[GetHelperThreadID メソッド](icordebugprocess-gethelperthreadid-method.md)|デバッガーの内部ヘルパースレッドのオペレーティングシステム (OS) スレッド ID を取得します。|  
|[GetID メソッド](icordebugprocess-getid-method.md)|プロセスのオペレーティングシステム (OS) ID を取得します。|  
|[GetObject メソッド](icordebugprocess-getobject-method.md)|実装されていません。|  
|[GetThread メソッド](icordebugprocess-getthread-method.md)|指定された OS スレッド ID を持つコードスレッドインスタンスを取得します。|  
|[GetThreadContext メソッド](icordebugprocess-getthreadcontext-method.md)|指定されたスレッドのコンテキストを取得します。|  
|[IsOSSuspended メソッド](icordebugprocess-isossuspended-method.md)|デバッガーがプロセスを停止した結果としてスレッドが中断されたかどうかを判断します。|  
|[IsTransitionStub メソッド](icordebugprocess-istransitionstub-method.md)|マネージコードへの遷移を発生させるスタブ内にアドレスがあるかどうかを判断します。|  
|[ModifyLogSwitch メソッド](icordebugprocess-modifylogswitch-method.md)|指定されたログスイッチの重大度レベルを設定します。|  
|[ReadMemory メソッド](icordebugprocess-readmemory-method.md)|プロセスからメモリを読み取ります。|  
|[SetThreadContext メソッド](icordebugprocess-setthreadcontext-method.md)|指定されたスレッドのコンテキストを設定します。|  
|[ThreadForFiberCookie メソッド](icordebugprocess-threadforfibercookie-method.md)|非推奨。|  
|[WriteMemory メソッド](icordebugprocess-writememory-method.md)|プロセスのメモリ領域にデータを書き込みます。|  
  
## <a name="remarks"></a>コメント  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebug インターフェイス](icordebug-interface.md)
- [デバッグ インターフェイス](debugging-interfaces.md)
