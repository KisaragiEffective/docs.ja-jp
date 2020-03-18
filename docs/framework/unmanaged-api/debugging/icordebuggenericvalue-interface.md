---
title: ICorDebugGenericValue インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugGenericValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugGenericValue
helpviewer_keywords:
- ICorDebugGenericValue interface [.NET Framework debugging]
ms.assetid: bc14f408-b359-4c8c-ade2-888ccdf7261b
topic_type:
- apiref
ms.openlocfilehash: e60d4b128bf03ff81863e0c95815b2c204807583
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794468"
---
# <a name="icordebuggenericvalue-interface"></a>ICorDebugGenericValue インターフェイス

すべての値に適用される "ICorDebugValue" のサブクラス。 このインターフェイスは、値に対して Get メソッドと Set メソッドを提供します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetValue メソッド](icordebuggenericvalue-getvalue-method.md)|指定したバッファーに値をコピーします。|  
|[SetValue メソッド](icordebuggenericvalue-setvalue-method.md)|指定したバッファーから新しい値をコピーします。|  
  
## <a name="remarks"></a>コメント  
 `ICorDebugGenericValue` は、リモート処理が不可能なため、サブインターフェイスです。  
  
 参照型の場合、値は参照の内容ではなく参照です。  
  
 このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
