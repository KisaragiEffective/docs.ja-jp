---
title: ICorDebugProcess2::SetUnmanagedBreakpoint メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugProcess2.SetUnmanagedBreakpoint
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess2::SetUnmanagedBreakpoint
helpviewer_keywords:
- ICorDebugProcess2::SetUnmanagedBreakpoint method [.NET Framework debugging]
- SetUnmanagedBreakpoint method [.NET Framework debugging]
ms.assetid: 93829d15-d942-4e2d-b7a4-dfc9d7fb96be
topic_type:
- apiref
ms.openlocfilehash: ffab2762fd86e95c3272ca456039028e0897bc41
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73137179"
---
# <a name="icordebugprocess2setunmanagedbreakpoint-method"></a>ICorDebugProcess2::SetUnmanagedBreakpoint メソッド
指定したネイティブイメージオフセットにアンマネージブレークポイントを設定します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT SetUnmanagedBreakpoint (  
    [in]  CORDB_ADDRESS    address,  
    [in]  ULONG32          bufsize,  
    [out, size_is(bufsize), length_is(*bufLen)]   
        BYTE               buffer[],  
    [out] ULONG32          *bufLen  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `address`  
 からネイティブイメージオフセットを指定する `CORDB_ADDRESS` オブジェクト。  
  
 `bufsize`  
 から`buffer` 配列のサイズ (バイト単位)。  
  
 `buffer`  
 入出力ブレークポイントによって置き換えられるオペコードを格納している配列。  
  
 `bufLen`  
 入出力`buffer` 配列で返されたバイト数へのポインター。  
  
## <a name="remarks"></a>Remarks  
 ネイティブイメージオフセットが共通言語ランタイム (CLR) 内にある場合、ブレークポイントは無視されます。 これにより、ブレークポイントがデバッガーによって設定されたときに、CLR は帯域外のブレークポイントのディスパッチを回避できます。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
