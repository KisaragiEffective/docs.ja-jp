---
title: ICorDebugFrame インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugFrame
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugFrame
helpviewer_keywords:
- ICorDebugFrame interface [.NET Framework debugging]
ms.assetid: 0c48f764-3c64-4602-b2f4-4ffc60eb2c65
topic_type:
- apiref
ms.openlocfilehash: ba138e79e0d6fb6f9c5e9c3efe3466f3c88cccae
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76782615"
---
# <a name="icordebugframe-interface"></a>ICorDebugFrame インターフェイス

現在のスタックのフレームを表します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[CreateStepper メソッド](icordebugframe-createstepper-method.md)|この `ICorDebugFrame`に対して、ステップ実行操作を実行する ICorDebugStepper を取得します。|  
|[GetCallee メソッド](icordebugframe-getcallee-method.md)|このフレームが呼び出された現在のチェーン内の `ICorDebugFrame` へのポインターを取得します。または、このがチェーン内の最も内側のフレームである場合は null を返します。|  
|[GetCaller メソッド](icordebugframe-getcaller-method.md)|このフレームを呼び出した現在のチェーン内の `ICorDebugFrame` へのポインターを取得します。これがチェーンの最も外側のフレームである場合は null を返します。|  
|[GetChain メソッド](icordebugframe-getchain-method.md)|この `ICorDebugFrame` が含まれている、このチェーンへのポインターを取得します。|  
|[GetCode メソッド](icordebugframe-getcode-method.md)|このスタックフレームに関連付けられているテキストコードへのポインターを取得します。|  
|[GetFunction メソッド](icordebugframe-getfunction-method.md)|このスタックフレームに関連付けられているコードを格納しているコードを指すポインターを取得します。|  
|[GetFunctionToken メソッド](icordebugframe-getfunctiontoken-method.md)|このスタックフレームに関連付けられているコードを含む関数のメタデータトークンを取得します。|  
|[GetStackRange メソッド](icordebugframe-getstackrange-method.md)|この `ICorDebugFrame`によって表されるスタックフレームの絶対アドレス範囲を取得します。|  
  
## <a name="remarks"></a>コメント  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
