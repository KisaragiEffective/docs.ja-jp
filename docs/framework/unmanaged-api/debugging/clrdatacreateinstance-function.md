---
title: CLRDataCreateInstance 関数
ms.date: 03/30/2017
api_name:
- CLRDataCreateInstance
api_location:
- mscordbi.dll
- mscordacwks.dll
api_type:
- COM
f1_keywords:
- CLRDataCreateInstance
helpviewer_keywords:
- CLRDataCreateInstance function [.NET Framework debugging]
ms.assetid: 440bad90-5a88-45e7-9157-4596801d8d19
topic_type:
- apiref
ms.openlocfilehash: 71836108dbd0ce01a64b4d9ac773c28d385dfd7c
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73099685"
---
# <a name="clrdatacreateinstance-function"></a>CLRDataCreateInstance 関数
指定したターゲット項目のインターフェイスオブジェクトを作成します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT CLRDataCreateInstance (  
    [in]  REFIID           iid,   
    [in]  ICLRDataTarget  *target,   
    [out] void           **iface  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `iid`  
 からインスタンス化するインターフェイスの識別子。  
  
 `target`  
 からインターフェイスオブジェクトの作成対象となる項目を表す、ユーザーによって実装された[ICLRDataTarget](iclrdatatarget-interface.md)オブジェクトへのポインター。  
  
 `iface`  
 入出力返されたインターフェイスオブジェクトのアドレスへのポインター。  
  
## <a name="remarks"></a>Remarks  
 `ICLRDataTarget` オブジェクトは、デバッグアプリケーションのライターによって実装されます。 実装は、表示されるターゲット項目の種類によって異なります。 ターゲット項目には、プロセス、メモリダンプ、リモートコンピューターなどがあります。  
  
## <a name="requirements"></a>［要件］  
 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** ClrData .idl  
  
 **ライブラリ:** CorGuids.lib  
  
 **.NET Framework のバージョン:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ グローバル静的関数](debugging-global-static-functions.md)
