---
title: ICorDebugThread インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugThread
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread
helpviewer_keywords:
- ICorDebugThread interface [.NET Framework debugging]
ms.assetid: 3930fd9b-2bc3-4b72-80a0-b6eeb94d60c6
topic_type:
- apiref
ms.openlocfilehash: b227b374021136e78f7f061d235eb18eca5b9515
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791478"
---
# <a name="icordebugthread-interface"></a>ICorDebugThread インターフェイス
プロセス内のスレッドを表します。 `ICorDebugThread` インスタンスの有効期間は、それが表しているスレッドの有効期間と同じです。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[ClearCurrentException メソッド](icordebugthread-clearcurrentexception-method.md)|このメソッドは実装されていません。 使用しないでください。|  
|[CreateEval メソッド](icordebugthread-createeval-method.md)|この `ICorDebugThread`を操作する、のオブジェクトを作成します。|  
|[CreateStepper メソッド](icordebugthread-createstepper-method.md)|この `ICorDebugThread`内のアクティブなフレームをステップ実行できるようにする、ICorDebugStepper オブジェクトを作成します。|  
|[EnumerateChains メソッド](icordebugthread-enumeratechains-method.md)|この `ICorDebugThread`内のすべてのスタックチェーンを格納している ICorDebugChainEnum 列挙子へのインターフェイスポインターを取得します。|  
|[GetActiveChain メソッド](icordebugthread-getactivechain-method.md)|この `ICorDebugThread`上のアクティブなについてのオブジェクトチェーンへのインターフェイスポインターを取得します。|  
|[GetActiveFrame メソッド](icordebugthread-getactiveframe-method.md)|この `ICorDebugThread`のアクティブなテキストフレームへのインターフェイスポインターを取得します。|  
|[GetAppDomain メソッド](icordebugthread-getappdomain-method.md)|この `ICorDebugThread` が現在実行されているアプリケーションドメインへのインターフェイスポインターを取得します。|  
|[GetCurrentException メソッド](icordebugthread-getcurrentexception-method.md)|現在マネージコードによってスローされている例外を表す、ICorDebugValue オブジェクトへのインターフェイスポインターを取得します。|  
|[GetDebugState メソッド](icordebugthread-getdebugstate-method.md)|この `ICorDebugThread`の現在のデバッグ状態を示す CorDebugThreadState 値を取得します。|  
|[GetHandle メソッド](icordebugthread-gethandle-method.md)|この `ICorDebugThread`のアクティブな部分の現在のハンドルを取得します。|  
|[GetID メソッド](icordebugthread-getid-method.md)|この `ICorDebugThread`のアクティブな部分の現在のオペレーティングシステム識別子を取得します。|  
|[GetObject メソッド](icordebugthread-getobject-method.md)|共通言語ランタイム (CLR) スレッドへのインターフェイスポインターを取得します。|  
|[GetProcess メソッド](icordebugthread-getprocess-method.md)|この `ICorDebugThread` がパートを形成するプロセスへのインターフェイスポインターを取得します。|  
|[GetRegisterSet メソッド](icordebugthread-getregisterset-method.md)|この `ICorDebugThread`に関連付けられているレジスタセットへのインターフェイスポインターを取得します。|  
|[GetUserState メソッド](icordebugthread-getuserstate-method.md)|この `ICorDebugThread`の現在の状態を示す CorDebugUserState 値のビットごとの組み合わせを取得します。|  
|[SetDebugState メソッド](icordebugthread-setdebugstate-method.md)|この `ICorDebugThread`のデバッグ状態を記述する `CorDebugThreadState` 値のビットごとの組み合わせを設定します。|  
  
## <a name="remarks"></a>コメント  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
