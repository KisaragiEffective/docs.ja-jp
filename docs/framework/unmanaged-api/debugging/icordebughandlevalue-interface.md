---
title: ICorDebugHandleValue インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugHandleValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugHandleValue
helpviewer_keywords:
- ICorDebugHandleValue interface [.NET Framework debugging]
ms.assetid: 66fcd2b8-ac66-414b-83a8-75a925e17772
topic_type:
- apiref
ms.openlocfilehash: 406468fc6e2b68e8c8e1dfbd0f0f18cce3f013ab
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794454"
---
# <a name="icordebughandlevalue-interface"></a>ICorDebugHandleValue インターフェイス

デバッガーがガベージコレクションのハンドルを作成した参照値を表す、値のサブクラス。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[Dispose メソッド](icordebughandlevalue-dispose-method.md)|インターフェイスポインターを明示的に解放せずに、この `ICorDebugHandleValue` オブジェクトが参照するハンドルを解放します。|  
|[GetHandleType メソッド](icordebughandlevalue-gethandletype-method.md)|この `ICorDebugHandleValue`によって参照されるハンドルの種類を示す CorDebugHandleType 値を取得します。|  
  
## <a name="remarks"></a>コメント  
 デバッグ対象のコードの実行が中断された場合、`ICorDebugReferenceValue` オブジェクトは無効になります。 `ICorDebugHandleValue` は、明示的に解放されるまで、中断と継続を通じて参照を保持します。  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
