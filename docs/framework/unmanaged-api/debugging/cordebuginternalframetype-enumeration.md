---
title: CorDebugInternalFrameType 列挙型
ms.date: 03/30/2017
api_name:
- CorDebugInternalFrameType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugInternalFrameType
helpviewer_keywords:
- CorDebugInternalFrameType enumeration [.NET Framework debugging]
ms.assetid: e4412dc2-c338-4cfb-94d8-f682095dd2b1
topic_type:
- apiref
ms.openlocfilehash: 2be827e12db765485ee889d6a4a19a982dad5d54
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76778369"
---
# <a name="cordebuginternalframetype-enumeration"></a>CorDebugInternalFrameType 列挙型
スタック フレームの型を示します。 この列挙体は、 [GetFrameType](icordebuginternalframe-getframetype-method.md)メソッドによって使用されます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorDebugInternalFrameType {  
  
    STUBFRAME_NONE                 = 0x00000000,  
    STUBFRAME_M2U                  = 0x00000001,  
    STUBFRAME_U2M                  = 0x00000002,  
    STUBFRAME_APPDOMAIN_TRANSITION = 0x00000003,  
    STUBFRAME_LIGHTWEIGHT_FUNCTION = 0x00000004,  
    STUBFRAME_FUNC_EVAL            = 0x00000005,  
    STUBFRAME_INTERNALCALL         = 0x00000006,  
    STUBFRAME_CLASS_INIT           = 0x00000007,  
    STUBFRAME_EXCEPTION            = 0x00000008,  
    STUBFRAME_SECURITY             = 0x00000009,  
    STUBFRAME_JIT_COMPILATION     = 0x0000000a,  
} CorDebugInternalFrameType;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`STUBFRAME_NONE`|null 値。 `ICorDebugInternalFrame::GetFrameType` メソッドは、この値を返しません。|  
|`STUBFRAME_M2U`|アンマネージスタブフレーム。|  
|`STUBFRAME_U2M`|アンマネージスタブフレーム。|  
|`STUBFRAME_APPDOMAIN_TRANSITION`|アプリケーションドメイン間の移行。|  
|`STUBFRAME_LIGHTWEIGHT_FUNCTION`|ライトウェイトメソッド呼び出し。|  
|`STUBFRAME_FUNC_EVAL`|関数の評価の開始。|  
|`STUBFRAME_INTERNALCALL`|共通言語ランタイムへの内部呼び出し。|  
|`STUBFRAME_CLASS_INIT`|クラスの初期化の開始。|  
|`STUBFRAME_EXCEPTION`|スローされる例外。|  
|`STUBFRAME_SECURITY`|コードアクセスセキュリティに使用されるフレーム。|  
|`STUBFRAME_JIT_COMPILATION`|ランタイムは、メソッドを JIT コンパイルしています。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [列挙型のデバッグ](debugging-enumerations.md)
