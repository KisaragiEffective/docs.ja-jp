---
title: ICorDebugController::Continue メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugController.Continue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController::Continue
helpviewer_keywords:
- Continue method [.NET Framework debugging]
- ICorDebugController::Continue method [.NET Framework debugging]
ms.assetid: 8684cd06-ad3e-48ef-832e-15320e1f43a2
topic_type:
- apiref
ms.openlocfilehash: 14356a12c944ef93dba5e7b818d3ee5cf5adc607
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73125418"
---
# <a name="icordebugcontrollercontinue-method"></a>ICorDebugController::Continue メソッド

[Stop メソッド](icordebugcontroller-stop-method.md)の呼び出し後に、マネージスレッドの実行を再開します。

## <a name="syntax"></a>構文

```cpp
HRESULT Continue (
    [in] BOOL fIsOutOfBand
);
```

## <a name="parameters"></a>パラメーター

`fIsOutOfBand`  
から帯域外イベントから続行する場合は `true` に設定します。それ以外の場合は、を `false`に設定します。

## <a name="remarks"></a>Remarks

`Continue` は、`ICorDebugController::Stop` メソッドの呼び出し後にプロセスを続行します。

混合モードのデバッグを実行する場合は、アウトオブバンドイベントから続行する場合を除き、Win32 イベントスレッドで `Continue` を呼び出さないでください。

*帯域内イベント*は、マネージイベント、またはデバッガーがプロセスのマネージ状態との対話をサポートする通常のアンマネージイベントのいずれかです。 この場合、デバッガーは、`fOutOfBand` パラメーターが `false` に設定された状態で[ICorDebugUnmanagedCallback::D Eのイベント](icordebugunmanagedcallback-debugevent-method.md)コールバックを受け取ります。

*アウトオブバンドイベント*は、イベントによってプロセスが停止されている間に、プロセスのマネージ状態との相互作用が不可能なアンマネージイベントです。 この場合、デバッガーは、`fOutOfBand` パラメーターが `true`に設定された `ICorDebugUnmanagedCallback::DebugEvent` コールバックを受け取ります。

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。

**ヘッダー:** CorDebug.idl、CorDebug.h

**ライブラリ:** CorGuids.lib

**.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
