---
title: ICorDebugThread3::GetActiveInternalFrames メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugThread3.GetActiveInternalFrames Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread3::GetActiveInternalFrames
helpviewer_keywords:
- ICorDebugThread3::GetActiveInternalFrames method [.NET Framework debugging]
- GetActiveInternalFrames method [.NET Framework debugging]
ms.assetid: d69796b4-5b6d-457c-85f6-2cf42e8a8773
topic_type:
- apiref
ms.openlocfilehash: 25cd3e05bc80dd39d2ca558bb4dd5fb77d255f5a
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76791396"
---
# <a name="icordebugthread3getactiveinternalframes-method"></a>ICorDebugThread3::GetActiveInternalFrames メソッド
スタック上の内部フレーム ([ICorDebugInternalFrame2](icordebuginternalframe2-interface.md) objects) の配列を返します。  
  
## <a name="syntax"></a>構文  
  
```cpp 
HRESULT GetActiveInternalFrames  
      (  
      [in] ULONG32 cInternalFrames,  
      [out] ULONG32 *pcInternalFrames,  
      [in, out,size_is(cInternalFrames), length_is(*pcInternalFrames)]  
            ICorDebugInternalFrame2 * ppInternalFrames[]  
      );  
```  
  
## <a name="parameters"></a>パラメーター  
 `cInternalFrames`  
 から`ppInternalFrames`に必要な内部フレームの数。  
  
 `pcInternalFrames`  
 入出力スタック上の内部フレームの数を格納している `ULONG32` へのポインター。  
  
 `ppInternalFrames`  
 [入力、出力]スタック上の内部フレームの配列のアドレスへのポインター。  
  
## <a name="return-value"></a>戻り値  
 このメソッドは、次の特定の HRESULT と、メソッドの失敗を示す HRESULT エラーも返します。  
  
|HRESULT|説明|  
|-------------|-----------------|  
|S_OK|[ICorDebugInternalFrame2](icordebuginternalframe2-interface.md)オブジェクトが正常に作成されました。|  
|E_INVALIDARG|`cInternalFrames` が0ではなく、`ppInternalFrames` が `null`か、または `pcInternalFrames` が `null`。|  
|HRESULT_FROM_WIN32(ERROR_INSUFFICIENT_BUFFER)|`ppInternalFrames` が内部フレームの数よりも小さくなっています。|  
  
## <a name="exceptions"></a>例外  
  
## <a name="remarks"></a>コメント  
 内部フレームは、一時データを格納するためにランタイムによってスタックにプッシュされるデータ構造です。  
  
 最初に `GetActiveInternalFrames`を呼び出すときは、`cInternalFrames` パラメーターを 0 (ゼロ) に設定し、`ppInternalFrames` パラメーターを null に設定する必要があります。 `GetActiveInternalFrames` 最初にを返すと、`pcInternalFrames` にはスタック上の内部フレームの数が含まれます。  
  
 `GetActiveInternalFrames` は、2回目に呼び出す必要があります。 `cInternalFrames` パラメーターに適切なカウント (`pcInternalFrames`) を渡し、`ppInternalFrames`で適切にサイズ設定された配列へのポインターを指定する必要があります。  
  
 実際のスタックフレームを返すには、の[テキスト](icordebugthread3-getactiveinternalframes-method.md)を使用します。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ インターフェイス](debugging-interfaces.md)
- [デバッグ](index.md)
