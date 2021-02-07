---
description: '詳細については、次のページを参照してください: いいでしょう:: GetCaller メソッド'
title: ICorDebugChain::GetCaller メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugChain.GetCaller
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugChain::GetCaller
helpviewer_keywords:
- ICorDebugChain::GetCaller method [.NET Framework debugging]
- GetCaller method, ICorDebugChain interface [.NET Framework debugging]
ms.assetid: d0b8ab4b-d7d2-4fa0-945f-3d2b87e7e991
topic_type:
- apiref
ms.openlocfilehash: 5af2132b7fec9e70704db980b95221db6eb273f8
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99695022"
---
# <a name="icordebugchaingetcaller-method"></a>ICorDebugChain::GetCaller メソッド

このチェーンを呼び出したチェーンを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetCaller (  
    [out] ICorDebugChain      **ppChain  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `ppChain`  
 入出力呼び出し元チェーンを表す、のオブジェクトのアドレスへのポインター。  
  
 (このチェーンまたはデバッガーが呼び出し履歴を初期化した場合のように) このチェーンが自発的に呼び出された場合、 `ppChain` は null になります。  
  
## <a name="remarks"></a>解説  

 呼び出しがスレッド間でマーシャリングされている場合は、呼び出し元のチェーンが別のスレッドに存在する可能性があります。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
