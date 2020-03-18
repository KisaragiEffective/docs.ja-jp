---
title: CorDebugPlatform 列挙型
ms.date: 03/30/2017
api_name:
- CorDebugPlatformEnum
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugPlatformEnum
helpviewer_keywords:
- CorDebugPlatformEnum enumeration [.NET Framework debugging]
ms.assetid: c5444816-7378-4521-afd3-bf5e4b5303d5
topic_type:
- apiref
ms.openlocfilehash: 2ed32449c4a133e6e72ec44f9cb57f49de22164a
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76778244"
---
# <a name="cordebugplatform-enumeration"></a>CorDebugPlatform 列挙型
は[、のターゲットプラットフォームの値](icordebugdatatarget-getplatform-method.md)を提供しています。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum CorDebugPlatform  
{  
    CORDB_PLATFORM_WINDOWS_X86,    // Windows on Intel x86  
    CORDB_PLATFORM_WINDOWS_AMD64,  // Windows x64 (AMD64, Intel EM64T)  
    CORDB_PLATFORM_WINDOWS_IA64,   // Windows on Intel IA-64  
    CORDB_PLATFORM_MAC_PPC,        // Macintosh OS on PowerPC  
    CORDB_PLATFORM_MAC_X86,        // Macintosh OS on Intel x86  
    CORDB_PLATFORM_WINDOWS_ARM,    // Windows on ARM  
    CORDB_PLATFORM_MAC_AMD64       // MacOS on Intel x64  
} CorDebugPlatform;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|CORDB_PLATFORM_WINDOWS_X86|ターゲット プラットフォームは、Intel x86 ハードウェア上で稼動する Windows です。|  
|CORDB_PLATFORM_WINDOWS_AMD64|ターゲット プラットフォームは、AMD64 または Intel EM64T ハードウェア上で稼動する 64 ビット Windows です。|  
|CORDB_PLATFORM_WINDOWS_IA64|ターゲット プラットフォームは、Intel IA-64 ハードウェア上で稼動する 32 ビット Windows です。|  
|CORDB_PLATFORM_MAC_PPC|ターゲットプラットフォームは、PowerPC ハードウェアで実行されている Macintosh オペレーティングシステムです。|  
|CORDB_PLATFORM_MAC_X86|ターゲットプラットフォームは、Intel x86 ハードウェア上で実行されている Macintosh オペレーティングシステムです。|  
|CORDB_PLATFORM_WINDOWS_ARM|ターゲットプラットフォームは、Windows ARM ハードウェアで実行されている Macintosh オペレーティングシステムです。|  
|CORDB_PLATFORM_MAC_AMD64|ターゲットプラットフォームは、AMD64 ハードウェアで実行されている Macintosh オペレーティングシステムです。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
 `CORDB_PLATFORM_WINDOWS_ARM` および `CORDB_PLATFORM_MAC_AMD64` メンバーは、.NET Framework 4.5.2 以降のバージョンでのみ使用できます。  
  
## <a name="see-also"></a>関連項目

- [列挙型のデバッグ](debugging-enumerations.md)
