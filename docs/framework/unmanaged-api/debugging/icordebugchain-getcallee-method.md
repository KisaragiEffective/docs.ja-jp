---
title: ICorDebugChain::GetCallee メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugChain.GetCallee
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugChain::GetCallee method
helpviewer_keywords:
- ICorDebugChain::GetCallee method [.NET Framework debugging]
- GetCallee method, ICorDebugChain interface [.NET Framework debugging]
ms.assetid: 19560c79-abdc-4bdf-a5fe-eb362a59edc0
topic_type:
- apiref
ms.openlocfilehash: 5d28af09faae84b0482d438ae33f593f250490c1
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73196339"
---
# <a name="icordebugchaingetcallee-method"></a>ICorDebugChain::GetCallee メソッド
このチェーンによって呼び出されたチェーンを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetCallee (  
    [out] ICorDebugChain     **ppChain  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `ppChain`  
 入出力呼び出されたチェーンを表す、のオブジェクトのアドレスへのポインター。 このチェーンが現在実行されている場合 (つまり、このチェーンが呼び出し先チェーンの戻りを待機していない場合)、`ppChain` は null になります。  
  
## <a name="remarks"></a>Remarks  
 このチェーンは、呼び出されたチェーンが返されるのを待機してから、実行を再開します。 スレッド間でマーシャリングされた呼び出しの場合、呼び出されたチェーンは別のスレッド上にある可能性があります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
