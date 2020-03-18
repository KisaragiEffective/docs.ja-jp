---
title: は、次のメソッドを実行します。
ms.date: 03/30/2017
api_name:
- ICorDebugVariableHomeEnum.Next
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugVariableHomeEnum::Next
helpviewer_keywords:
- ICorDebugVariableHomeEnum::Next method [.NET Framework debugging]
- Next method, ICorDebugVariableHomeEnum interface [.NET Framework debugging]
ms.assetid: eb9ea96c-5b58-4655-8104-094fc8b393b8
topic_type:
- apiref
ms.openlocfilehash: 2bb6fee00bb99555bc19f35e1250880cc3985f7f
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76790937"
---
# <a name="icordebugvariablehomeenumnext-method"></a>は、次のメソッドを実行します。
関数内のローカル変数および引数に関する情報を格納している指定された数の表示変数[home](icordebugvariablehome-interface.md)インスタンスを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT Next(  
    [in] ULONG celt,  
    [out, size_is(celt), length_is(*pceltFetched)] ICorDebugVariableHome *homes[],  
    [out] ULONG *pceltFetched  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `celt`  
 [in] 取得するオブジェクトの数。  
  
 `homes`  
 ポインターの配列。各ポインターは、関数のローカル変数または引数に関する情報を提供[する、の各オブジェクトを](icordebugvariablehome-interface.md)参照します。  
  
 `pceltFetched`  
 入出力実際にオブジェクトで返されたインスタンスの数。  
  
## <a name="return-value"></a>戻り値  
 メソッドは、次の値を返します。  
  
|HRESULT|説明|  
|-------------|-----------------|  
|`S_OK`|メソッドは正常に終了しました。|  
|`S_FALSE`|`pceltFetched`に反映された実際に取得されたインスタンスの数が、要求されたインスタンスの数より少なくなっています。|  
  
## <a name="remarks"></a>コメント  
 [次](icordebugvariablehomeenum-next-method.md)のメソッドは、列挙子の現在位置から最大 `celt` オブジェクトを取得します。 メソッドから制御が戻ったときに、`pceltFetched` 取得したオブジェクトの実際の数が含まれます。  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v462plus](../../../../includes/net-current-v462plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebugVariableHomeEnum インターフェイス](icordebugvariablehomeenum-interface.md)
- [ICorDebugVariableHome インターフェイス](icordebugvariablehome-interface.md)
