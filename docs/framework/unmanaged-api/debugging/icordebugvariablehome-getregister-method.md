---
title: 'いい変数 Home:: GetRegister メソッド'
ms.date: 03/30/2017
api_name:
- ICorDebugVariableHome.GetRegister
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugVariableHome::GetRegister
helpviewer_keywords:
- ICorDebugVariableHome::GetRegister method [.NET Framework debugging]
- GetRegister method, ICorDebugVariableHome interface [.NET Framework debugging]
ms.assetid: a5eecd7b-b04c-4266-bff2-7c8771d519a8
topic_type:
- apiref
ms.openlocfilehash: 396dd9c017fca6dc7037b43355ba7f726d7390ea
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76790988"
---
# <a name="icordebugvariablehomegetregister-method"></a>いい変数 Home:: GetRegister メソッド
`VLT_REGISTER`の場所の種類を持つ変数と、場所の種類が `VLT_REGISTER_RELATIVE`の変数の基本レジスタを含むレジスタを取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetRegister(  
    [out] CorDebugRegister *pRegister  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `pRegister`  
 入出力`VLT_REGISTER`の場所の種類を持つ変数のレジスタと、`VLT_REGISTER_RELATIVE`の場所の種類を持つ変数の基本レジスタを示す CorDebugRegister 列挙値。  
  
## <a name="return-value"></a>戻り値  
 メソッドは、次の値を返します。  
  
|Value|説明|  
|-----------|-----------------|  
|`S_OK`|変数は、`pRegister` 引数によって示されるレジスタにあります。|  
|`E_FAIL`|変数がレジスタまたはレジスタの相対位置にありません。|  
  
## <a name="requirements"></a>要件  
 **:** 「[システム要件](../../../../docs/framework/get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v462plus](../../../../includes/net-current-v462plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [VariableLocationType 列挙型](variablelocationtype-enumeration.md)
- [ICorDebugVariableHome インターフェイス](icordebugvariablehome-interface.md)
