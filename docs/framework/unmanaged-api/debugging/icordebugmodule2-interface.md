---
title: ICorDebugModule2 インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugModule2
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule2
helpviewer_keywords:
- ICorDebugModule2 interface [.NET Framework debugging]
ms.assetid: 0847e64f-fdbe-4c96-8168-da20fdc84d80
topic_type:
- apiref
ms.openlocfilehash: 32774aacecf3e56510bc6f0670538a44fde794c9
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76792986"
---
# <a name="icordebugmodule2-interface"></a>ICorDebugModule2 インターフェイス

モジュールインターフェイスの論理拡張機能として機能します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[ApplyChanges メソッド](icordebugmodule2-applychanges-method.md)|メタデータの変更と、Microsoft 中間言語 (MSIL) コードの変更を実行中のプロセスに適用します。|  
|[GetJITCompilerFlags メソッド](icordebugmodule2-getjitcompilerflags-method.md)|この `ICorDebugModule2`の just-in-time (JIT) コンパイルを制御するフラグを取得します。|  
|[ResolveAssembly メソッド](icordebugmodule2-resolveassembly-method.md)|指定したメタデータトークンによって参照されるアセンブリを解決します。|  
|[SetJITCompilerFlags メソッド](icordebugmodule2-setjitcompilerflags-method.md)|この `ICorDebugModule2`の JIT コンパイルを制御するフラグを設定します。|  
|[SetJMCStatus メソッド](icordebugmodule2-setjmcstatus-method.md)|この `ICorDebugModule2` 内のすべてのクラスのすべてのメソッドのマイコードのみ (JMC) 状態を、指定した値に設定します。ただし、逆の値に設定するのは、`pTokens` 配列内のすべてのメソッドです。|  
  
## <a name="remarks"></a>コメント  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
