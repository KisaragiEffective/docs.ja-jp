---
description: '詳細情報: _EFN_GetManagedObjectName 関数'
title: _EFN_GetManagedObjectName 関数
ms.date: 03/30/2017
api_name:
- _EFN_GetManagedObjectName
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- _EFN_GetManagedObjectName
helpviewer_keywords:
- _EFN_GetManagedObjectName function [.NET Framework debugging]
ms.assetid: 6e7c6bee-7ced-495f-bf6c-2a5f0c716f7e
topic_type:
- apiref
ms.openlocfilehash: 4fa47848ace973f43acbcf8ab0322db4b974b205
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99637899"
---
# <a name="_efn_getmanagedobjectname-function"></a>\_EFN \_ GetManagedObjectName 関数

指定されたマネージオブジェクトポインターを使用して、型の名前を取得します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT _EFN_GetManagedObjectName(  
    [in]  PDEBUG_CLIENT  Client,  
    [in]  ULONG64        objAddr,  
    [out] __out_ecount(cbName) PSTR szName,  
    [out] ULONG          cbName  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `Client`  
 からデバッグクライアントへのポインター。  
  
 `objAddr`  
 からマネージオブジェクトポインター。  
  
 szName  
 入出力型の名前。  
  
 `cbName`  
 入出力文字列バッファーで使用できる文字数。  
  
## <a name="remarks"></a>解説  

 現在コンテキスト内にあるスレッドにマネージコードがない場合、関数は、ファシリティ値が0xa0 でエラーコードが0x1000 の HRESULT SOS_E_NOMANAGEDCODE を返します。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** SOS_Stacktrace  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- [デバッグ グローバル静的関数](debugging-global-static-functions.md)
