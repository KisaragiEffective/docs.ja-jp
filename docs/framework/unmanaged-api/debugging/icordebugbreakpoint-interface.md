---
title: ICorDebugBreakpoint インターフェイス
ms.date: 03/30/2017
api_name:
- ICorDebugBreakpoint
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugBreakpoint
helpviewer_keywords:
- ICorDebugBreakpoint interface [.NET Framework debugging]
ms.assetid: aa5ad3d7-e1bb-42af-99bc-471224e3bcaa
topic_type:
- apiref
ms.openlocfilehash: 53d8d219a13f2dade16a338efccf0837f8de0158
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76784374"
---
# <a name="icordebugbreakpoint-interface"></a>ICorDebugBreakpoint インターフェイス

関数内のブレークポイント、または値のウォッチポイントを表します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[Activate メソッド](icordebugbreakpoint-activate-method.md)|この `ICorDebugBreakpoint`のアクティブな状態を設定します。|  
|[IsActive メソッド](icordebugbreakpoint-isactive-method.md)|この `ICorDebugBreakpoint` がアクティブかどうかを示す値を取得します。|  
  
## <a name="remarks"></a>コメント  
 ブレークポイントは、条件式を直接サポートしません。 このような機能が必要な場合は、デバッガーが `ICorDebugBreakpoint`上に実装する必要があります。  
  
 は、関数内のブレークポイントをサポートするために `ICorDebugBreakpoint` を拡張します。  
  
> [!NOTE]
> このインターフェイスは、コンピューター間またはプロセス間でのリモート呼び出しをサポートしていません。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
