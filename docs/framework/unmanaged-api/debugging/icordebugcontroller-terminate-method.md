---
title: ICorDebugController::Terminate メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugController.Terminate
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController::Terminate
helpviewer_keywords:
- Terminate method, ICorDebugController interface [.NET Framework debugging]
- ICorDebugController::Terminate method [.NET Framework debugging]
ms.assetid: 4275af0c-b5a7-4e4c-97c9-7e41f36b2dd8
topic_type:
- apiref
ms.openlocfilehash: 851a127c117b826c271dd021c41cfdb36045a1ff
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76783756"
---
# <a name="icordebugcontrollerterminate-method"></a>ICorDebugController::Terminate メソッド
指定された終了コードを使用してプロセスを終了します。  
  
> [!NOTE]
> このメソッドは、Win32 `TerminateProcess` 関数のラッパーです。 このため、`Terminate` は、Win32 `TerminateProcess` 関数が使用するのと同じ方法で終了コードを使用します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT Terminate (  
    [in] UINT exitCode  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `exitCode`  
 から終了コードを表す数値。 有効な数値は、Winbase. h で定義されています。  
  
## <a name="remarks"></a>コメント  
 `Terminate` が呼び出されたときにプロセスが停止された場合は、次のようにする必要があり[ます。このメソッドを](icordebugcontroller-continue-method.md)使用すると、デバッガーは、" [ExitProcess](icordebugmanagedcallback-exitprocess-method.md) " または ": [Exitmanagedcallback:: exitappdomain](icordebugmanagedcallback-exitappdomain-method.md)コールバック" を使用して終了の確認を受け取ることができます。  
  
> [!NOTE]
> このメソッドは、アプリケーションドメインによって実装されていません。 つまり、<xref:System.AppDomain> レベルでは実装されていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目
