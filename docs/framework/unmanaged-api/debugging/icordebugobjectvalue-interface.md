---
title: ICorDebugObjectValue インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugObjectValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugObjectValue
helpviewer_keywords:
- ICorDebugObjectValue interface [.NET Framework debugging]
ms.assetid: 937de6a0-6fbf-4ddc-80ea-a6217b73e62b
topic_type:
- apiref
ms.openlocfilehash: e104f8c522af2ee4cd42332b7459f4a2fd185511
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76792683"
---
# <a name="icordebugobjectvalue-interface"></a>ICorDebugObjectValue インターフェイス

オブジェクトを含む値を表す "ICorDebugValue" のサブクラス。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetClass メソッド](icordebugobjectvalue-getclass-method.md)|この `ICorDebugObjectValue` が参照するオブジェクトの共通言語ランタイム (CLR) <xref:System.Type> へのインターフェイスポインターを取得します。|  
|[GetContext メソッド](icordebugobjectvalue-getcontext-method.md)|実装されていません。|  
|[GetFieldValue メソッド](icordebugobjectvalue-getfieldvalue-method.md)|指定したクラスの指定したフィールドの値を表す、 [ICorDebugValue](icordebugvalue-interface.md)へのインターフェイスポインターを取得します。|  
|[GetManagedCopy メソッド](icordebugobjectvalue-getmanagedcopy-method.md)|互換性のために残されています。 このメソッドを呼び出さないでください。|  
|[GetVirtualMethod メソッド](icordebugobjectvalue-getvirtualmethod-method.md)|実装されていません。|  
|[IsValueClass メソッド](icordebugobjectvalue-isvalueclass-method.md)|この `ICorDebugObjectValue` によって参照されるオブジェクトが値型かどうかを示す値を取得します。|  
|[SetFromManagedCopy メソッド](icordebugobjectvalue-setfrommanagedcopy-method.md)|互換性のために残されています。 このメソッドを呼び出さないでください。|  
  
## <a name="remarks"></a>コメント  
 `ICorDebugObjectValue` は、デバッグ中のプロセスが続行されるまで有効なままです。  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
