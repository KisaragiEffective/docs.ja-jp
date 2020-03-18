---
title: ICorDebugVirtualUnwinder::GetContext メソッド
ms.date: 03/30/2017
ms.assetid: fe502a76-3068-47e5-a0a0-85ccb72dfac3
ms.openlocfilehash: ff5e5bdd66ec44a0931b51212f07485718507576
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76790838"
---
# <a name="icordebugvirtualunwindergetcontext-method"></a>ICorDebugVirtualUnwinder::GetContext メソッド
このアンワインダーの現在のコンテキストを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetContext(  
   [in] ULONG32 contextFlags,  
   [in] ULONG32 cbContextBuf,  
   [out] ULONG32* contextSize,  
   [out, size_is(cbContextBuf)] BYTE contextBuf[]  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `contextFlags`  
 [in] (WinNT.h で定義されている) コンテキストのどの部分を返すかを指定するフラグ。  
  
 `cbContextBuf`  
 [in] `contextBuf` のバイト数。  
  
 `contextSize`  
 [out] `contextBuf` に実際に書き込まれたバイト数へのポインター。  
  
 `contextBuf`  
 [out] このアンワインダーの現在のコンテキストが格納されているバイト配列。  
  
## <a name="return-value"></a>戻り値  
 mscordbi によって受信された失敗を示す HRESULT 値は致命的と見なされ、ICorDebug API によって `CORDBG_E_DATA_TARGET_ERROR` が返されます。  
  
## <a name="remarks"></a>コメント  
 `contextBuf` 引数の初期値を、 [「GetContext](icordebugstackwalk-getcontext-method.md) 」メソッドを呼び出すことによって返されるコンテキストバッファーに設定します。  
  
> [!NOTE]
> このメソッドは .NET ネイティブでのみ使用できます。  
  
 アンワインドではレジスタのサブセット (例: 不揮発性レジスタのみ) だけが復元されるため、コンテキストが、実際のメソッド呼び出し時点でのレジスタの状態と正確には一致しないことがあります。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebugMemoryBuffer インターフェイス](icordebugmemorybuffer-interface.md)
- [デバッグ インターフェイス](debugging-interfaces.md)
