---
description: '詳細情報: ICorDebugInstanceFieldSymbol::GetName メソッド'
title: ICorDebugInstanceFieldSymbol::GetName メソッド
ms.date: 03/30/2017
ms.assetid: d9c12b1f-9c1d-4943-8e9e-93b55faf085f
ms.openlocfilehash: 9cc2106d1527aa0b4d9e5c52115b703ffe55037f
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99791193"
---
# <a name="icordebuginstancefieldsymbolgetname-method"></a>ICorDebugInstanceFieldSymbol::GetName メソッド

インスタンス フィールドの名前を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT GetName(  
   [in] ULONG32 cchName,
   [out] ULONG32 *pcchName,
   [out, size_is(cchName), length_is(*pcchName)] WCHAR szName[]  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `cchName`  
 [in] `szName` バッファー内の文字数。  
  
 `pcchName`  
 [out] `szName` バッファーに実際に書き込まれた文字数へのポインター。  
  
 `szName`  
 [out] 返される名前を格納する文字配列。  
  
## <a name="remarks"></a>解説  
  
> [!NOTE]
> このメソッドは .NET ネイティブでのみ使用できます。  
  
## <a name="requirements"></a>必要条件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebugInstanceFieldSymbol インターフェイス](icordebuginstancefieldsymbol-interface.md)
- [デバッグのインターフェイス](debugging-interfaces.md)
