---
title: ICorDebugNativeFrame インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugNativeFrame
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugNativeFrame
helpviewer_keywords:
- ICorDebugNativeFrame interface [.NET Framework debugging]
ms.assetid: 04819c58-7246-4b32-befb-680cf1dbc436
topic_type:
- apiref
ms.openlocfilehash: 41ac0b29ade2f78b893df72e8a17624373f6dd78
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76792787"
---
# <a name="icordebugnativeframe-interface"></a>ICorDebugNativeFrame インターフェイス

ネイティブフレームに使用される、特殊な実装のテキストフレーム。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[CanSetIP メソッド](icordebugnativeframe-cansetip-method.md)|命令ポインターをネイティブコード内の指定したオフセット位置に安全に設定できるかどうかを示す値を取得します。|  
|[GetIP メソッド](icordebugnativeframe-getip-method.md)|ネイティブコードへのスタックフレームのオフセットを取得します。|  
|[GetLocalDoubleRegisterValue メソッド](icordebugnativeframe-getlocaldoubleregistervalue-method.md)|ネイティブフレームの2つのメモリレジスタに格納されている引数またはローカル変数の値を表す ICorDebugValue へのポインターを取得します。|  
|[GetLocalMemoryRegisterValue メソッド](icordebugnativeframe-getlocalmemoryregistervalue-method.md)|指定したレジスタに下位ビットが格納されているローカル変数の値を表す `ICorDebugValue` へのポインターを取得します。上位ビットは、指定したメモリアドレスに格納されます。|  
|[GetLocalMemoryValue メソッド](icordebugnativeframe-getlocalmemoryvalue-method.md)|指定したメモリアドレスに格納されているローカル変数の値を表す `ICorDebugValue` へのポインターを取得します。|  
|[GetLocalRegisterMemoryValue メソッド](icordebugnativeframe-getlocalregistermemoryvalue-method.md)|指定したレジスタに上位ビットが格納され、指定したメモリアドレスに下位ビットが格納されているローカル変数の値を表す `ICorDebugValue` へのポインターを取得します。|  
|[GetLocalRegisterValue メソッド](icordebugnativeframe-getlocalregistervalue-method.md)|引数の値または指定したネイティブレジスタに格納されているローカル変数を表す `ICorDebugValue` へのポインターを取得します。|  
|[GetRegisterSet メソッド](icordebugnativeframe-getregisterset-method.md)|この `ICorDebugNativeFrame`のレジスタセットを表す、[ツールセットへ](icordebugregisterset-interface.md)のポインターを取得します。|  
|[SetIP メソッド](icordebugnativeframe-setip-method.md)|命令ポインターをネイティブコード内の指定されたオフセット位置に設定します。|  
  
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
