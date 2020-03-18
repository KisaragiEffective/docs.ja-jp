---
title: ICorDebugProcess3::SetEnableCustomNotification メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugProcess3.SetEnableCustomNotification Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess3::SetEnableCustomNotification
helpviewer_keywords:
- ICorDebugProcess3::SetEnableCustomNotification method [.NET Framework debugging]
- SetEnableCustomNotification method [.NET Framework debugging]
ms.assetid: afd88ee9-2589-4461-a75a-9b6fe55a2525
topic_type:
- apiref
ms.openlocfilehash: f2f365f3fe1568f2dd3bad677dd77a13946002e1
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76792460"
---
# <a name="icordebugprocess3setenablecustomnotification-method"></a>ICorDebugProcess3::SetEnableCustomNotification メソッド
指定された型のカスタムデバッガー通知を有効または無効にします。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetEnableCustomNotification(ICorDebugClass * pClass,  
                                    BOOL fEnable);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pClass`  
 からカスタムデバッガー通知を指定する型。  
  
 `fEnable`  
 [in] カスタムデバッガー通知を有効にする `true`通知を無効にする `false` ます。 既定値は `false`です。  
  
## <a name="remarks"></a>コメント  
 `fEnable` が `true`に設定されている場合、<xref:System.Diagnostics.Debugger.NotifyOfCrossThreadDependency%2A?displayProperty=nameWithType> メソッドを呼び出すと、 [ICorDebugManagedCallback3:: CustomNotification](icordebugmanagedcallback3-customnotification-method.md)コールバックがトリガーされます。 既定では、通知は無効になっています。そのため、デバッガーは、認識している通知の種類を指定し、処理を希望する必要があります。 このクラス[のクラスはアプリケーション](icordebug-interface.md)ドメインによってスコープが設定されているため、プロセス全体で通知を受け取る必要がある場合は、プロセス内のすべてのアプリケーションドメインに対して `SetEnableCustomNotification` を呼び出す必要があります。  
  
 .NET Framework 4 以降、サポートされている通知は、スレッド間の依存関係通知だけです。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebugProcess3 インターフェイス](icordebugprocess3-interface.md)
- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
