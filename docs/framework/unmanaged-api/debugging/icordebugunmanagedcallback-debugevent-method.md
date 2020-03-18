---
title: ICorDebugUnmanagedCallback::DebugEvent メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugUnmanagedCallback.DebugEvent
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugUnmanagedCallback::DebugEvent
helpviewer_keywords:
- DebugEvent method [.NET Framework debugging]
- ICorDebugUnmanagedCallback::DebugEvent method [.NET Framework debugging]
ms.assetid: be9cab04-65ec-44d5-a39a-f90709fdd043
topic_type:
- apiref
ms.openlocfilehash: cb52150a17c9ec8f4bbc25c13b85bce56b221eeb
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791192"
---
# <a name="icordebugunmanagedcallbackdebugevent-method"></a>ICorDebugUnmanagedCallback::DebugEvent メソッド
ネイティブイベントが発生したことをデバッガーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT DebugEvent (  
    [in] LPDEBUG_EVENT  pDebugEvent,  
    [in] BOOL           fOutOfBand  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pDebugEvent`  
 からネイティブイベントへのポインター。  
  
 `fOutOfBand`  
 [in] `true`、アンマネージイベントが発生した後にマネージプロセスの状態との対話が不可能な場合は、デバッガーがコード「: Continue」を呼び出す[よう](icordebugcontroller-continue-method.md)にします。それ以外の場合は、`false`ます。  
  
## <a name="remarks"></a>コメント  
 デバッグ中のスレッドが Win32 スレッドである場合は、Win32 デバッグインターフェイスのメンバーを使用しないでください。 `ICorDebugController::Continue` を呼び出すことができるのは、Win32 スレッドだけで、帯域外イベントを継続している場合のみです。  
  
 `DebugEvent` コールバックは、コールバックの標準規則に従っていません。 `DebugEvent`を呼び出すと、プロセスは、"生の OS-デバッグが停止しました" 状態になります。 プロセスは同期されません。 マネージコードに関する情報の要求を満たすために必要に応じて、同期状態が自動的に入力されます。これにより、他の入れ子になった `DebugEvent` コールバックが発生する可能性があります。  
  
 プロセス[を続行](icordebugprocess-clearcurrentexception-method.md)する前に例外イベントを無視するには、プロセスでを実行します。 このメソッドを呼び出すと、CONTINUE 要求で DBG_EXCEPTION_NOT_HANDLED ではなく DBG_CONTINUE が送信され、帯域外のブレークポイントとシングルステップの例外が自動的にクリアされます。 帯域外イベントは、デバッグ中のアプリケーションが停止した場合や、未処理の帯域内イベントが既に存在する場合でも、いつでも発生する可能性があります。  
  
 .NET Framework バージョン2.0 では、デバッガーはすぐに帯域外のブレークポイントイベントを終了する必要があります。 デバッガーでは、 [ICorDebugProcess2:: SetUnmanagedBreakpoint](icordebugprocess2-setunmanagedbreakpoint-method.md)メソッドと[ICorDebugProcess2:: ClearUnmanagedBreakpoint](icordebugprocess2-clearunmanagedbreakpoint-method.md)メソッドを使用して、ブレークポイントの追加と削除を行う必要があります。 これらのメソッドは、帯域外のブレークポイントを自動的にスキップします。 したがって、ディスパッチされるアウトオブバンドブレークポイントは、Win32 `DebugBreak` 関数の呼び出しなど、既に命令ストリームにある未加工のブレークポイントである必要があります。 [デバッグ API](index.md)のその他のメンバーである、`ICorDebugProcess::ClearCurrentException`、の[getthreadcontext](icordebugprocess-getthreadcontext-method.md)、テキスト[処理:: setthreadcontext](icordebugprocess-setthreadcontext-method.md)、またはその他のメンバーを使用しないようにしてください。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebugUnmanagedCallback インターフェイス](icordebugunmanagedcallback-interface.md)
