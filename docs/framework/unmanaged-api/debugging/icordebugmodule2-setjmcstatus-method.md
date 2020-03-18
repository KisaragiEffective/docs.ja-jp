---
title: ICorDebugModule2::SetJMCStatus メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugModule2.SetJMCStatus
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule2::SetJMCStatus
helpviewer_keywords:
- SetJMCStatus method, ICorDebugModule2 interface [.NET Framework debugging]
- ICorDebugModule2::SetJMCStatus method [.NET Framework debugging]
ms.assetid: 8c6d2089-4dbb-4715-b9e9-2a4491c8c9ce
topic_type:
- apiref
ms.openlocfilehash: a0b70078dee88b270d8361aa9bddcb7d80df1db1
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73129471"
---
# <a name="icordebugmodule2setjmcstatus-method"></a>ICorDebugModule2::SetJMCStatus メソッド
この ICorDebugModule2 内のすべてのクラスのすべてのメソッドのマイコードのみ (JMC) の状態を、指定した値に設定します。ただし、逆の値に設定するのは、`pTokens` 配列内のすべてのメソッドです。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetJMCStatus (  
    [in] BOOL                        bIsJustMyCode,  
    [in] ULONG32                     cTokens,  
    [in, size_is(cTokens)] mdToken   pTokens[]  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `bIsJustMycode`  
 からコードをデバッグする場合は `true` に設定します。それ以外の場合は、を `false`に設定します。  
  
 `cTokens`  
 [in] `pTokens` 配列のサイズ。  
  
 `pTokens`  
 から`mdToken` 値の配列。各値は、JMC 状態がに設定されるメソッドを参照します。`bIsJustMycode`。  
  
## <a name="remarks"></a>Remarks  
 `pTokens` 配列に指定されている各メソッドの JMC の状態は、`bIsJustMycode` 値の逆に設定されます。 このモジュール内の他のすべてのメソッドの状態は、`bIsJustMycode` 値に設定されます。  
  
 `SetJMCStatus` メソッドは、このモジュール内の以前のすべての JMC 設定を消去します。  
  
 すべての関数が正常に設定されている場合、`SetJMCStatus` メソッドは S_OK HRESULT を返します。 `true` としてマークされている一部の関数がデバッグ可能でない場合は、CORDBG_E_FUNCTION_NOT_DEBUGGABLE HRESULT が返されます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
