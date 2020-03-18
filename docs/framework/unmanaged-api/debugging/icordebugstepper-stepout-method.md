---
title: ICorDebugStepper::StepOut メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugStepper.StepOut
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugStepper::StepOut
helpviewer_keywords:
- ICorDebugStepper::StepOut method [.NET Framework debugging]
- StepOut method [.NET Framework debugging]
ms.assetid: aae0f48c-4ede-4256-9251-a7fc85a229dc
topic_type:
- apiref
ms.openlocfilehash: 08cf2d0bb09080296fc1fcc69b5817f4d6118765
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791722"
---
# <a name="icordebugstepperstepout-method"></a>ICorDebugStepper::StepOut メソッド
この ICorDebugStepper は、それを含むスレッドを1ステップずつ実行し、現在のフレームが呼び出し元のフレームに制御を返すときに完了します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT StepOut ();  
```  
  
## <a name="remarks"></a>コメント  
 `StepOut` 操作は、現在のフレームから呼び出し元のフレームに正常に戻った後に完了します。  
  
 アンマネージコード内で `StepOut` が呼び出された場合、現在のフレームがそれを呼び出したマネージコードに戻ると、手順が完了します。  
  
 .NET Framework バージョン2.0 では、STOP_UNMANAGED フラグが設定された `StepOut` を使用しないでください。これは失敗するためです。 (ステップ実行のフラグを設定するには、 [ICorDebugStepper:: SetUnmappedStopMask](icordebugstepper-setunmappedstopmask-method.md)を使用します。)相互運用デバッガーは、ネイティブコード自体にステップアウトする必要があります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
