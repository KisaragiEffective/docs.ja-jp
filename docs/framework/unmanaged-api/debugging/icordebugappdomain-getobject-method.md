---
title: ICorDebugAppDomain::GetObject メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugAppDomain.GetObject
api_location:
- corguids.lib
api_type:
- COM
f1_keywords:
- ICorDebugAppDomain::GetObject
helpviewer_keywords:
- ICorDebugAppDomain::GetObject method [.NET Framework debugging]
- GetObject method, ICorDebugAppDomain interface [.NET Framework debugging]
ms.assetid: 78232e6f-ae18-4cfa-a6cd-e79471cf9d76
topic_type:
- apiref
ms.openlocfilehash: f2c881603cfa0e4b3d2dc8d1e996631b51d1e850
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73134715"
---
# <a name="icordebugappdomaingetobject-method"></a>ICorDebugAppDomain::GetObject メソッド
共通言語ランタイム (CLR) アプリケーションドメインへのインターフェイスポインターを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetObject (  
    [out] ICorDebugValue   **ppObject  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `ppObject`  
 入出力CLR アプリケーションドメインを表す ICorDebugValue インターフェイスオブジェクトのアドレスへのポインター。  
  
## <a name="return-value"></a>戻り値  
 このアプリケーションドメインに対してマネージ <xref:System.AppDomain?displayProperty=nameWithType> オブジェクトが構築されていない場合、メソッドは `S_FALSE` を返し、`NULL` を `*ppObject`に配置します。  
  
## <a name="remarks"></a>Remarks  
 プロセス内の各アプリケーションドメインは、それを表すランタイムにマネージ <xref:System.AppDomain?displayProperty=nameWithType> オブジェクトを持つことができます。 この関数は、このマネージ <xref:System.AppDomain?displayProperty=nameWithType> オブジェクトに対応する ICorDebugValue インターフェイスオブジェクトを取得します。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]
