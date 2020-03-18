---
title: ICorDebugProcess2::GetReferenceValueFromGCHandle メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugProcess2.GetReferenceValueFromGCHandle
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess2::GetReferenceValueFromGCHandle
helpviewer_keywords:
- GetReferenceValueFromGCHandle method [.NET Framework debugging]
- ICorDebugProcess2::GetReferenceValueFromGCHandle method [.NET Framework debugging]
ms.assetid: 8bdd7f4c-19f2-4ede-875e-603773e8c128
topic_type:
- apiref
ms.openlocfilehash: 47647bf0460507b4c88b47bf87bfcc3bf620aecc
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73137222"
---
# <a name="icordebugprocess2getreferencevaluefromgchandle-method"></a>ICorDebugProcess2::GetReferenceValueFromGCHandle メソッド
ガベージコレクションハンドルを持つ指定したマネージオブジェクトへの参照ポインターを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetReferenceValueFromGCHandle (  
    [in]  UINT_PTR                 handle,  
    [out] ICorDebugReferenceValue  **pOutValue  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `handle`  
 からガベージコレクションハンドルを持つマネージオブジェクトへのポインター。 この値は <xref:System.IntPtr> オブジェクトであり、マネージオブジェクトの <xref:System.Runtime.InteropServices.GCHandle> から取得できます。  
  
 `pOutValue`  
 入出力指定したマネージオブジェクトへの参照を表す、値オブジェクトのアドレスへのポインター。  
  
## <a name="remarks"></a>Remarks  
 返された参照値とガベージコレクション参照値を混同しないようにしてください。  
  
 返される参照は、通常の参照のように動作します。 ブレークポイント後にコードの実行が続行される場合は無効になります。 ターゲットオブジェクトの有効期間は、参照値の有効期間の影響を受けません。  
  
> [!NOTE]
> `GetReferenceValueFromGCHandle` メソッドでは、ハンドルは検証されません。 したがって、`GetReferenceValueFromGCHandle` メソッドは、無効なハンドルが渡された場合に、デバッガーとデバッグされているコードの両方を破損する可能性があります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
