---
description: '詳細情報: ICorDebugMutableDataTarget::WriteVirtual Method'
title: ICorDebugMutableDataTarget::WriteVirtual メソッド
ms.date: 03/30/2017
ms.assetid: 80833648-58a7-491a-8dc8-9a48e9bb3adc
ms.openlocfilehash: 3f3400456b7af51f4b24d7e14e35d641f03a8bfd
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99637821"
---
# <a name="icordebugmutabledatatargetwritevirtual-method"></a>ICorDebugMutableDataTarget::WriteVirtual メソッド

ターゲット プロセスのアドレス空間にメモリを書き込みます。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT WriteVirtual(  
   [in] CORDB_ADDRESS address,  
   [in, size_is(bytesRequested)] const BYTE * pBuffer,  
   [in] ULONG32 bytesRequested);  
```  
  
## <a name="parameters"></a>パラメーター  

 `address`  
 [in] `pBuffer` の内容の書き込み先のアドレスです。  
  
 `pBuffer`  
 [in] 書き込むバイト数が格納されているバイト配列へのポインター。  
  
 `address`  
 [in] `pBuffer` のバイト数。  
  
## <a name="return-value"></a>戻り値  

 正常に完了した場合は `S_OK`、失敗した場合はその他の `HRESULT`。  
  
## <a name="remarks"></a>解説  

 バイトを書き込むことができない場合、メソッド呼び出しは失敗し、ターゲット アドレス空間のバイトは変更されません。 (それ以外の場合、ターゲットは不整合な状態になり、デバッグの信頼性がさらに低下します。)  
  
## <a name="requirements"></a>必要条件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** CorDebug.idl、CorDebug.h  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v46plus](../../../../includes/net-current-v46plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [ICorDebugMutableDataTarget インターフェイス](icordebugmutabledatatarget-interface.md)
- [デバッグのインターフェイス](debugging-interfaces.md)
