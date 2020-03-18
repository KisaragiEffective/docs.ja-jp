---
title: IDebugAutoAttach::AutoAttach メソッド
ms.date: 03/30/2017
api_name:
- IDebugAutoAttach.AutoAttach
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- IDebugAutoAttach::AutoAttach
helpviewer_keywords:
- AutoAttach method [.NET Framework debugging]
- IDebugAutoAttach::AutoAttach method [.NET Framework debugging]
ms.assetid: 3cf3bd9c-7d88-4afa-a476-94cdc7609aa6
topic_type:
- apiref
ms.openlocfilehash: 766aeb31436101babeab31b615a1c633578bfcc5
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74445526"
---
# <a name="idebugautoattachautoattach-method"></a>IDebugAutoAttach::AutoAttach メソッド
サーバーによって呼び出されたデバッガーの自動アタッチを実行します。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT AutoAttach  
(  
    [in]  REFGUID   guidPort,  
    [in]  DWORD     dwPid,  
    [in]  AUTOATTACH_PROGRAM_TYPE dwProgramType,  
    [in]  DWORD     dwProgramId,  
    [in]  LPCWSTR   pszSessionId  
);  
```  
  
## <a name="parameters"></a>パラメーター  
 `guidPort`  
 から常に `GUID_NULL`に設定します。  
  
 `dwPid`  
 からプロセス ID。通常は `GetCurrentProcessId` 関数を使用して取得されます。  
  
 `dwProgramType`  
 からプログラムの種類: `AUTOATTACH_PROGRAM_WIN32`、`AUTOATTACH_PROGRAM_COMPLUS`、または `AUTOATTACH_PROGRAM_UNKNOWN`。  
  
 `dwProgramId`  
 からプログラム ID。  
  
 `pszSessionId`  
 からデバッグ動詞によって渡された文字列。  
  
## <a name="return-value"></a>戻り値  
 メソッドが成功した場合は S_OK します。  
  
## <a name="requirements"></a>要件  
 **ヘッダー:** DbgAutoAttach .h  
  
## <a name="see-also"></a>参照

- [IDebugAutoAttach インターフェイス](../../../../docs/framework/unmanaged-api/diagnostics/idebugautoattach-interface.md)
