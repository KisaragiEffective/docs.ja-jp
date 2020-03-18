---
title: ICorDebugFrame::GetStackRange メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugFrame.GetStackRange
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugFrame::GetStackRange
helpviewer_keywords:
- GetStackRange method, ICorDebugFrame interface [.NET Framework debugging]
- ICorDebugFrame::GetStackRange method [.NET Framework debugging]
ms.assetid: fab037cb-fda6-40fb-9367-921e435dd5a0
topic_type:
- apiref
ms.openlocfilehash: 828e4dc67cb93d0a35879e94b54c9fac6e5bda16
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73124084"
---
# <a name="icordebugframegetstackrange-method"></a>ICorDebugFrame::GetStackRange メソッド
このスタックフレームの絶対アドレス範囲を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetStackRange (  
    [out] CORDB_ADDRESS      *pStart,   
    [out] CORDB_ADDRESS      *pEnd  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pStart`  
 入出力この `ICorDebugFrame` オブジェクトで表されるスタックフレームの開始アドレスを指定する `CORDB_ADDRESS` へのポインター。  
  
 `pEnd`  
 入出力この `ICorDebugFrame` オブジェクトで表されるスタックフレームの終了アドレスを指定する `CORDB_ADDRESS` へのポインター。  
  
## <a name="remarks"></a>Remarks  
 スタックのアドレス範囲は、複数のデバッグエンジンから収集されたインターリーブスタックトレースを piecing する場合に役立ちます。 数値の範囲は、スタックフレームの内容に関する情報を提供しません。 スタックフレームの位置を比較する場合にのみ意味があります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
