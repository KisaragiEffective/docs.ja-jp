---
description: '詳細については、次のページを参照してください: いいね!:: GetRegisters メソッド'
title: ICorDebugRegisterSet::GetRegisters メソッド
ms.date: 03/30/2017
api_name:
- ICorDebugRegisterSet.GetRegisters
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugRegisterSet::GetRegisters
helpviewer_keywords:
- GetRegisters method, ICorDebugRegisterSet interface [.NET Framework debugging]
- ICorDebugRegisterSet::GetRegisters method [.NET Framework debugging]
ms.assetid: fdf91864-48ea-4aa6-b70c-361b7a3184c7
topic_type:
- apiref
ms.openlocfilehash: efb0f19fe9eb823912203b82267803739fc3e2cd
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99690848"
---
# <a name="icordebugregistersetgetregisters-method"></a>ICorDebugRegisterSet::GetRegisters メソッド

ビットマスクによって指定された、(現在コードを実行しているコンピューター上の) 各レジスタの値を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetRegisters (  
    [in] ULONG64       mask,
    [in] ULONG32       regCount,  
    [out, size_is(regCount), length_is(regCount)]  
        CORDB_REGISTER regBuffer[]  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `mask`  
 から取得するレジスタ値を指定するビットマスク。 各ビットは、レジスタに対応します。 ビットが1に設定されている場合は、レジスタの値が取得されます。それ以外の場合は、レジスタの値は取得されません。  
  
 `regCount`  
 から取得するレジスタ値の数。  
  
 `regBuffer`  
 入出力オブジェクトの配列 `CORDB_REGISTER` 。各オブジェクトは、レジスタの値を受け取ります。  
  
## <a name="remarks"></a>解説  

 配列のサイズは、ビットマスクで1に設定されているビット数と同じである必要があります。 パラメーターは、 `regCount` レジスタ値を受け取るバッファー内の要素の数を指定します。 `regCount`マスクによって示されるレジスタの数に対して値が小さすぎる場合は、番号が大きいレジスタがセットから切り捨てられます。 `regCount`値が大きすぎる場合、未使用の `regBuffer` 要素は変更されません。  
  
 使用できないレジスタがビットマスクによって指定されている場合、は `GetRegisters` そのレジスタの不定値を返します。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebugRegisterSet インターフェイス](icordebugregisterset-interface.md)
- [ICorDebugRegisterSet2 インターフェイス](icordebugregisterset2-interface.md)
