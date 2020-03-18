---
title: ICorDebugProcess5::EnableNGENPolicy メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugProcess5::EnableNGenPolicy
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess5::EnableNGENPolicy
helpviewer_keywords:
- ICorDebugProcess5::EnableNGENPolicy method [.NET Framework debugging]
- EnableNGENPolicy method, ICorDebugProcess5 interface [.NET Framework debugging]
ms.assetid: 3b8e15ca-3c72-4685-a937-da4c739cb9e9
topic_type:
- apiref
ms.openlocfilehash: 9497bea9b7cc5eb98876c923858dbcbc6adf9d07
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76792448"
---
# <a name="icordebugprocess5enablengenpolicy-method"></a>ICorDebugProcess5::EnableNGENPolicy メソッド
マネージデバッガーで実行中にアプリケーションがネイティブイメージを読み込む方法を決定する値を設定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT EnableNGENPolicy(  
    [in] CorDebugNGENPolicy ePolicy  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `ePolicy`  
 からマネージデバッガーで実行中にアプリケーションがネイティブイメージを読み込む方法を決定する[Cordebugngenpolicy](cordebugngenpolicy-enumeration.md)定数。  
  
## <a name="remarks"></a>コメント  
 ポリシーが正常に設定されている場合、メソッドは `S_OK`を返します。 `ePolicy` が[Cordebugngenpolicy](cordebugngenpolicy-enumeration.md)によって定義された列挙値の範囲外にある場合、メソッドは `E_INVALIDARG` を返し、メソッドの呼び出しは無効になります。 ネイティブイメージジェネレーター (Ngen.exe) のポリシーを更新できない場合、メソッドは `E_FAIL`を返します。  
  
 `ICorDebugProcess5::EnableNGenPolicy` メソッドは、プロセスの有効期間中はいつでも呼び出すことができます。 ポリシーは、ポリシーが設定された後に読み込まれるすべてのモジュールに対して有効です。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebugProcess5 インターフェイス](icordebugprocess5-interface.md)
- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
