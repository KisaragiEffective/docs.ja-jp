---
title: ICorDebugVariableSymbol インターフェイス
ms.date: 03/30/2017
ms.assetid: 0e58b85e-69bd-41ff-bedb-8cdc8be6a7a2
ms.openlocfilehash: 9d17bc92dcae9e906dfe18d7665fbf489d278234
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76790871"
---
# <a name="icordebugvariablesymbol-interface"></a>ICorDebugVariableSymbol インターフェイス
変数のデバッグ シンボル情報を取得します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド|説明|  
|------------|-----------------|  
|[GetName メソッド](icordebugvariablesymbol-getname-method.md)|変数の名前を取得します。|  
|[GetSize メソッド](icordebugvariablesymbol-getsize-method.md)|変数のサイズ (バイト単位) を取得します。|  
|[GetSlotIndex メソッド](icordebugvariablesymbol-getslotindex-method.md)|ローカル変数のマネージド スロット インデックスを取得します。|  
|[GetValue メソッド](icordebugvariablesymbol-getvalue-method.md)|変数の値をバイト配列として取得します。|  
|[SetValue メソッド](icordebugvariablesymbol-setvalue-method.md)|バイト配列の値を変数に代入します。|  
  
## <a name="remarks"></a>コメント  
  
> [!NOTE]
> このインターフェイスは .NET ネイティブでのみ使用可能です。 .NET ネイティブの外部で ICorDebug シナリオについてこのインターフェイスを実装する場合は、共通言語ランタイムはこのインターフェイスを無視します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
