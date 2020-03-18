---
title: ICorDebugHeapValue2::CreateHandle メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugHeapValue2.CreateHandle
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugHeapValue2::CreateHandle
helpviewer_keywords:
- CreateHandle method [.NET Framework debugging]
- ICorDebugHeapValue2::CreateHandle method [.NET Framework debugging]
ms.assetid: fbc418e8-fa22-420d-84ec-e0e1800db041
topic_type:
- apiref
ms.openlocfilehash: b9eab1274f2d0ad562c0dec6adeddb85c6cfc458
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73138393"
---
# <a name="icordebugheapvalue2createhandle-method"></a>ICorDebugHeapValue2::CreateHandle メソッド
この ICorDebugHeapValue2 オブジェクトによって表されるヒープ値に対して指定された型のハンドルを作成します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT CreateHandle (  
    [in] CorDebugHandleType      type,   
    [out] ICorDebugHandleValue   **ppHandle  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `type`  
 から作成するハンドルの種類を指定する CorDebugHandleType 列挙体の値。  
  
 `ppHandle`  
 入出力このヒープ値の新しいハンドルを表す、値オブジェクトのアドレスへのポインター。  
  
## <a name="remarks"></a>Remarks  
 ハンドルは、ヒープ値に関連付けられているアプリケーションドメインに作成され、アプリケーションドメインがアンロードされると無効になります。  
  
 同じヒープ値に対してこの関数を複数回呼び出すと、複数のハンドルが作成されます。 ハンドルはガベージコレクターのパフォーマンスに影響を与えるため、デバッガーは、一度にアクティブな比較的少数のハンドル (約 256) に制限する必要があります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
