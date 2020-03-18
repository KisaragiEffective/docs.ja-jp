---
title: ICorDebugStepper::StepRange メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugStepper.StepRange
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugStepper::StepRange
helpviewer_keywords:
- StepRange method [.NET Framework debugging]
- ICorDebugStepper::StepRange method [.NET Framework debugging]
ms.assetid: b9776112-6e6d-4708-892a-8873db02e16f
topic_type:
- apiref
ms.openlocfilehash: 21b8bf618e197372e301d5f56e7592c20710014d
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791709"
---
# <a name="icordebugsteppersteprange-method"></a>ICorDebugStepper::StepRange メソッド
この ICorDebugStepper は、含まれているスレッドを1ステップずつ実行し、指定した範囲の最後のコードに到達したときにを返します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT StepRange (  
    [in] BOOL     bStepIn,  
    [in, size_is(cRangeCount)] COR_DEBUG_STEP_RANGE ranges[],  
    [in] ULONG32  cRangeCount  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `bStepIn`  
 から`true` に設定すると、スレッド内で呼び出される関数にステップインします。 関数をステップオーバーするには、`false` に設定します。  
  
 `ranges`  
 からCOR_DEBUG_STEP_RANGE 構造体の配列。それぞれが範囲を指定します。  
  
 `cRangeCount`  
 [in] `ranges` 配列のサイズ。  
  
## <a name="remarks"></a>コメント  
 `StepRange` メソッドは[ICorDebugStepper:: Step](icordebugstepper-step-method.md)メソッドと同様に機能しますが、指定された範囲外のコードに到達するまでは完了しない点が異なります。  
  
 これは、一度に1つの命令をステップ実行するよりも効率的です。 範囲は、ステッパのフレームの先頭からのオフセットペアのリストとして指定されます。  
  
 範囲は、メソッドの MSIL (Microsoft 中間言語) コードに対する相対値です。 `false` を使用して[ICorDebugStepper:: SetRangeIL](icordebugstepper-setrangeil-method.md)を呼び出し、メソッドのネイティブコードを基準として範囲を設定します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
