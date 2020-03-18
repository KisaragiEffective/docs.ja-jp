---
title: ICorDebugHeapValue3::GetThreadOwningMonitorLock メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugHeapValue3.GetThreadOwningMonitorLock
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugHeapValue3::GetThreadOwningMonitorLock
helpviewer_keywords:
- GetThreadOwningMonitorLock method [.NET Framework debugging]
- ICorDebugHeapValue3::GetThreadOwningMonitorLock method [.NET Framework debugging]
ms.assetid: e06fc19d-2cf4-4cad-81a3-137a68af8969
topic_type:
- apiref
ms.openlocfilehash: 8be7c0e32f6183deb354d8b3936ef55c2520fe9f
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76788617"
---
# <a name="icordebugheapvalue3getthreadowningmonitorlock-method"></a>ICorDebugHeapValue3::GetThreadOwningMonitorLock メソッド
このオブジェクトのモニターロックを所有するマネージスレッドを返します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetThreadOwningMonitorLock (  
    [out] ICorDebugThread   **ppThread,  
    [out] DWORD              *pAcquisitionCount  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `ppThread`  
 入出力このオブジェクトのモニターロックを所有するマネージスレッド。  
  
 `pAcquisitionCount`  
 入出力このスレッドがロックを解除してから、所有が解除されるまでの回数。  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、次の特定の HRESULT と、メソッドの失敗を示す HRESULT エラーも返します。  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|メソッドは正常に終了しました。|  
|S_FALSE|このオブジェクトのモニターロックを所有しているマネージスレッドはありません。|  
  
## <a name="exceptions"></a>例外  
  
## <a name="remarks"></a>コメント  
 マネージスレッドがこのオブジェクトのモニターロックを所有している場合は、次のようになります。  
  
- メソッドは S_OK を返します。  
  
- スレッドオブジェクトは、スレッドが終了するまで有効です。  
  
 このオブジェクトのモニターロックを所有しているマネージスレッドが存在しない場合、`ppThread` と `pAcquisitionCount` は変更されず、メソッドは S_FALSE を返します。  
  
 `ppThread` または `pAcquisitionCount` が有効なポインターでない場合、結果は未定義になります。  
  
 このオブジェクトのモニターロックを所有しているスレッドが特定できない場合にエラーが発生すると、メソッドはエラーを示す HRESULT を返します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
