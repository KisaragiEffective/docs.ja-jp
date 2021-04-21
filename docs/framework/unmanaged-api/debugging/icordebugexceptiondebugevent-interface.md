---
description: '詳細情報: ICorDebugExceptionDebugEvent インターフェイス'
title: ICorDebugExceptionDebugEvent インターフェイス
ms.date: 03/30/2017
ms.assetid: f9ba60d8-b54d-417e-bb3e-fde4b41ca44c
ms.openlocfilehash: eacaa344763fb77faef5f66282809d741f017b37
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99693422"
---
# <a name="icordebugexceptiondebugevent-interface"></a>ICorDebugExceptionDebugEvent インターフェイス

[ICorDebugDebugEvent](icordebugdebugevent-interface.md) インターフェイスを拡張し、例外イベントをサポートします。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetFlags メソッド](icordebugexceptiondebugevent-getflags-method.md)|例外をインターセプトできるかどうかを示すフラグを取得します。|  
|[GetNativeIP メソッド](icordebugexceptiondebugevent-getnativeip-method.md)|この例外デバッグ イベントのネイティブ インターフェイス ポインターを取得します。|  
|[GetStackPointer メソッド](icordebugexceptiondebugevent-getstackpointer-method.md)|この例外デバッグ イベントのスタック ポインターを取得します。|  
  
## <a name="remarks"></a>解説  

 `ICorDebugExceptionDebugEvent` インターフェイスは、次のイベントの種類によって実装されます。  
  
- [MANAGED_EXCEPTION_FIRST_CHANCE](cordebugrecordformat-enumeration.md)  
  
- [MANAGED_EXCEPTION_USER_FIRST_CHANCE](cordebugrecordformat-enumeration.md)  
  
- [MANAGED_EXCEPTION_CATCH_HANDLER_FOUND](cordebugrecordformat-enumeration.md)  
  
- [MANAGED_EXCEPTION_UNHANDLED](cordebugrecordformat-enumeration.md)  
  
> [!NOTE]
> このインターフェイスは .NET ネイティブでのみ使用可能です。 インターフェイス ポインターを取得するために `QueryInterface` を呼び出そうとすると、.NET ネイティブ外の ICorDebug シナリオに対して `E_NOINTERFACE` が返されます。  
  
## <a name="requirements"></a>必要条件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグのインターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
