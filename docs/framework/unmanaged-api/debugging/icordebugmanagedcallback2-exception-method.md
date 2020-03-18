---
title: ICorDebugManagedCallback2::Exception メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugManagedCallback2.Exception
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugManagedCallback2::Exception
helpviewer_keywords:
- ICorDebugManagedCallback2::Exception method [.NET Framework debugging]
- Exception method, ICorDebugManagedCallback2 interface [.NET Framework debugging]
ms.assetid: 78b0f14f-2fae-4e63-8412-4df119ee8468
topic_type:
- apiref
ms.openlocfilehash: e7125d923fb1d3757bb4ca53f5a7db806b241dd9
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76781527"
---
# <a name="icordebugmanagedcallback2exception-method"></a>ICorDebugManagedCallback2::Exception メソッド
例外ハンドラーの検索が開始されたことをデバッガーに通知します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT Exception (  
    [in] ICorDebugAppDomain   *pAppDomain,  
    [in] ICorDebugThread      *pThread,  
    [in] ICorDebugFrame       *pFrame,  
    [in] ULONG32              nOffset,  
    [in] CorDebugExceptionCallbackType dwEventType,  
    [in] DWORD                dwFlags  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pAppDomain`  
 から例外がスローされたスレッドを含むアプリケーションドメインを表す、のオブジェクトへのポインター。  
  
 `pThread`  
 から例外がスローされたスレッドを表す、スレッドオブジェクトへのポインター。  
  
 `pFrame`  
 から`dwEventType` パラメーターによって決定される、フレームを表す、テキストボックスオブジェクトへのポインター。 詳細については、「解説」の表を参照してください。  
  
 `nOffset`  
 から`dwEventType` パラメーターによって決定されるオフセットを指定する整数。 詳細については、「解説」の表を参照してください。  
  
 `dwEventType`  
 からこの例外コールバックの種類を指定する Cordebugexceptioncallback Type 列挙体の値。  
  
 `dwFlags`  
 から例外に関する追加情報を指定する[Cordebugexceptionflags](cordebugexceptionflags-enumeration.md)列挙値  
  
## <a name="remarks"></a>コメント  
 `Exception` コールバックは、例外処理プロセスの検索フェーズ中にさまざまなポイントで呼び出されます。 つまり、例外のアンワインド中に複数回呼び出すことができます。  
  
 処理されている例外は、`pThread` パラメーターによって参照される、のスレッドオブジェクトから取得できます。  
  
 特定のフレームとオフセットは、`dwEventType` パラメーターによって次のように決定されます。  
  
|`dwEventType` の値|`pFrame` の値|`nOffset` の値|  
|----------------------------|-----------------------|------------------------|  
|DEBUG_EXCEPTION_FIRST_CHANCE|例外をスローしたフレーム。|フレーム内の命令ポインター。|  
|DEBUG_EXCEPTION_USER_FIRST_CHANCE|スローされた例外のポイントに最も近いユーザーコードフレーム。|フレーム内の命令ポインター。|  
|DEBUG_EXCEPTION_CATCH_HANDLER_FOUND|Catch ハンドラーを格納しているフレーム。|Catch ハンドラーの先頭の MSIL (Microsoft 中間言語) オフセット。|  
|DEBUG_EXCEPTION_UNHANDLED|NULL|シンボル.|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebugManagedCallback2 インターフェイス](icordebugmanagedcallback2-interface.md)
- [ICorDebugManagedCallback インターフェイス](icordebugmanagedcallback-interface.md)
