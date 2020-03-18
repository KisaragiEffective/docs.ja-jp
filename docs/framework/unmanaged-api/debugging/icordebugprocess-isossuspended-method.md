---
title: ICorDebugProcess::IsOSSuspended メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugProcess.IsOSSuspended
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess::IsOSSuspended
helpviewer_keywords:
- IsOSSuspended method [.NET Framework debugging]
- ICorDebugProcess::IsOSSuspended method [.NET Framework debugging]
ms.assetid: 83406cb2-5797-4402-872d-89c9516aefec
topic_type:
- apiref
ms.openlocfilehash: 21da69d4bff0f17eb607dda45fb7dbafea8c59f7
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128767"
---
# <a name="icordebugprocessisossuspended-method"></a>ICorDebugProcess::IsOSSuspended メソッド
デバッガーがこのプロセスを停止した結果として、指定されたスレッドが中断されたかどうかを示す値を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT IsOSSuspended(  
    [in]  DWORD threadID,  
    [out] BOOL  *pbSuspended);  
```  
  
## <a name="parameters"></a>パラメーター  
 `threadID`  
 から対象のスレッドの ID。  
  
 `pbSuspended`  
 入出力指定したスレッドが中断された場合に `true` されるブール値へのポインター。それ以外の場合、*`pbSuspended` は `false`です。  
  
## <a name="remarks"></a>Remarks  
 デバッガーがこのプロセスを停止した結果として、指定されたスレッドが中断された場合、指定されたスレッドの Win32 中断カウントは1だけインクリメントされます。 デバッガーのユーザーインターフェイス (UI) では、ユーザーに対してスレッドのオペレーティングシステム (OS) の中断カウントを表示すると、この情報を考慮に入れることができます。  
  
 `IsOSSuspended` メソッドは、アンマネージデバッグのコンテキストでのみ意味があります。 マネージデバッグ中に、スレッドは、OS が中断するのではなく協調的に中断されます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
