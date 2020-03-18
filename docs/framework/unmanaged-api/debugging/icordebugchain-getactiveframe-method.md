---
title: ICorDebugChain::GetActiveFrame メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugChain.GetActiveFrame
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugChain::GetActiveFrame
helpviewer_keywords:
- ICorDebugChain::GetActiveFrame method [.NET Framework debugging]
- GetActiveFrame method, ICorDebugChain interface [.NET Framework debugging]
ms.assetid: 36887017-670b-4f21-b406-8fab956f84a3
topic_type:
- apiref
ms.openlocfilehash: 03cb1556ee971124ed4c591f38d9f892fc7df7b0
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73192153"
---
# <a name="icordebugchaingetactiveframe-method"></a>ICorDebugChain::GetActiveFrame メソッド
チェーンのアクティブな (つまり、最新の) フレームを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetActiveFrame (  
    [out] ICorDebugFrame   **ppFrame  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `ppFrame`  
 入出力チェーン上のアクティブな (つまり、最新の) フレームを表す、の各フレームオブジェクトのアドレスへのポインター。  
  
## <a name="remarks"></a>Remarks  
 使用できるマネージスタックフレームがない場合、`ppFrame` は null に設定されます。  
  
 アクティブなフレームが使用できない場合、呼び出しは成功し、`ppFrame` は null になります。 CHAIN_ENTER_UNMANAGED によって開始されるチェーンや、CHAIN_CLASS_INIT によって開始されるチェーンに対して、アクティブなフレームは使用できません。 CorDebugChainReason 列挙体を参照してください。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
