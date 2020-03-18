---
title: LoggingLevelEnum 列挙型
ms.date: 03/30/2017
api_name:
- LoggingLevelEnum
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- LoggingLevelEnum
helpviewer_keywords:
- LoggingLevelEnum enumeration [.NET Framework debugging]
ms.assetid: 09daac08-005a-46b2-beab-408d0820c5e5
topic_type:
- apiref
ms.openlocfilehash: 1677798abdb8994d34c82a71e97a2c858209c18e
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76790386"
---
# <a name="logginglevelenum-enumeration"></a>LoggingLevelEnum 列挙型
マネージド スレッドがイベントを記録する際にイベント ログに書き込まれる説明メッセージの重大度レベルを示します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
typedef enum LoggingLevelEnum {  
    LTraceLevel0 = 0,  
    LTraceLevel1,  
    LTraceLevel2,  
    LTraceLevel3,  
    LTraceLevel4,  
    LStatusLevel0 = 20,  
    LStatusLevel1,  
    LStatusLevel2,  
    LStatusLevel3,  
    LStatusLevel4,  
    LWarningLevel = 40,  
    LErrorLevel = 50,  
    LPanicLevel = 100  
} LoggingLevelEnum;  
```  
  
## <a name="members"></a>メンバー  
  
|メンバー|説明|  
|------------|-----------------|  
|`LTraceLevel0`|メッセージはトレースレベル0です。|  
|`LTraceLevel1`|メッセージはトレースレベル1です。|  
|`LTraceLevel2`|メッセージはトレースレベル2です。|  
|`LTraceLevel3`|メッセージはトレースレベル3です。|  
|`LTraceLevel4`|メッセージはトレースレベル4です。|  
|`LStatusLevel0`|メッセージはステータスレベル0です。|  
|`LStatusLevel1`|メッセージはステータスレベル1です。|  
|`LStatusLevel2`|メッセージはステータスレベル2です。|  
|`LStatusLevel3`|メッセージはステータスレベル3です。|  
|`LStatusLevel4`|メッセージはステータスレベル4です。|  
|`LWarningLevel`|メッセージは警告レベルです。|  
|`LErrorLevel`|メッセージはエラーレベルです。|  
|`LPanicLevel`|メッセージはパニックレベルです。|  
  
## <a name="remarks"></a>コメント  
 共通言語ランタイム (CLR: common language runtime) は、コンポーネントのマネージ[コールバック:: LogMessage](icordebugmanagedcallback-logmessage-method.md)メソッドを呼び出して、マネージスレッドがイベントを記録したことをデバッガーに通知します。 CLR は `LoggingLevelEnum` 列挙値を渡して、マネージスレッドがイベントログに書き込んだメッセージの重大度レベルを示します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Diagnostics.EventLog>
- [列挙型のデバッグ](debugging-enumerations.md)
