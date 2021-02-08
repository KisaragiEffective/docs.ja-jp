---
description: '詳細については、次を参照してください: CorDebugChainReason 列挙型'
title: CorDebugChainReason 列挙型
ms.date: 03/30/2017
api_name:
- CorDebugChainReason
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugChainReason
helpviewer_keywords:
- CorDebugChainReason enumeration [.NET Framework debugging]
ms.assetid: c915da51-50b2-41df-841a-2b971f4d0975
topic_type:
- apiref
ms.openlocfilehash: 18b6ac9c50c3a44a77a0f63a680b84c70e667e9f
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99801749"
---
# <a name="cordebugchainreason-enumeration"></a>CorDebugChainReason 列挙型

呼び出しチェーンが開始する理由を示します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorDebugChainReason {  
    CHAIN_NONE              = 0x000,  
    CHAIN_CLASS_INIT        = 0x001,  
    CHAIN_EXCEPTION_FILTER  = 0x002,  
    CHAIN_SECURITY          = 0x004,  
    CHAIN_CONTEXT_POLICY    = 0x008,  
    CHAIN_INTERCEPTION      = 0x010,  
    CHAIN_PROCESS_START     = 0x020,  
    CHAIN_THREAD_START      = 0x040,  
    CHAIN_ENTER_MANAGED     = 0x080,  
    CHAIN_ENTER_UNMANAGED   = 0x100,  
    CHAIN_DEBUGGER_EVAL     = 0x200,  
    CHAIN_CONTEXT_SWITCH    = 0x400,  
    CHAIN_FUNC_EVAL         = 0x800  
} CorDebugChainReason;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`CHAIN_NONE`|呼び出しチェーンが開始されていません。|  
|`CHAIN_CLASS_INIT`|コンストラクターによって、チェーンが開始されました。|  
|`CHAIN_EXCEPTION_FILTER`|例外フィルターによって、チェーンが開始されました。|  
|`CHAIN_SECURITY`|セキュリティを適用するコードによって、チェーンが開始されました。|  
|`CHAIN_CONTEXT_POLICY`|コンテキスト ポリシーによって、チェーンが開始されました。|  
|`CHAIN_INTERCEPTION`|使用しません。|  
|`CHAIN_PROCESS_START`|使用しません。|  
|`CHAIN_THREAD_START`|スレッド実行の開始によって、チェーンが開始されました。|  
|`CHAIN_ENTER_MANAGED`|マネージド コードへのエントリによって、チェーンが開始されました。|  
|`CHAIN_ENTER_UNMANAGED`|アンマネージ コードへのエントリによって、チェーンが開始されました。|  
|`CHAIN_DEBUGGER_EVAL`|使用しません。|  
|`CHAIN_CONTEXT_SWITCH`|使用しません。|  
|`CHAIN_FUNC_EVAL`|関数の評価によって、チェーンが開始されました。|  
  
## <a name="remarks"></a>解説  

 呼び出しチェーンが開始された理由を確認するには、と [いう方法を使用します](icordebugchain-getreason-method.md) 。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [列挙体のデバッグ](debugging-enumerations.md)
