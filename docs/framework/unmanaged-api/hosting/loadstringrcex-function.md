---
description: '詳細情報: LoadStringRCEx 関数'
title: LoadStringRCEx 関数
ms.date: 03/30/2017
api_name:
- LoadStringRCEx
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- LoadStringRCEx
helpviewer_keywords:
- LoadStringRCEx function [.NET Framework hosting]
ms.assetid: bc789636-ca14-4f07-8f77-9305874d7495
topic_type:
- apiref
ms.openlocfilehash: d3fe4b97014e5093dd8d209a5e27bac4ed7b879f
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99679915"
---
# <a name="loadstringrcex-function"></a>LoadStringRCEx 関数

HRESULT 値を、指定したカルチャの適切なエラー メッセージに変換します。  
  
 この関数は .NET Framework 4 で非推奨とされました。  
  
## <a name="syntax"></a>構文  
  
```cpp  
HRESULT LoadStringRCEx (  
    [in]  LCID    lcid,
    [in]  UINT    iResouceID,
    [out] LPWSTR  szBuffer,
    [in]  int     iMax,
    [in]  int     bQuiet,
    [out] int    *pcwchUsed  
);  
```  
  
## <a name="parameters"></a>パラメーター  

 `lcid`  
 からカルチャ識別子。 既定のカルチャを使用するには、に-1 を渡し `lcid` ます。  
  
 `iResourceID`  
 からHRESULT。  
  
 `szBuffer`  
 入出力正常に完了したときのエラーメッセージを格納するバッファー。  
  
 `iMax`  
 からエラーメッセージバッファーのサイズ。  
  
 `bQuiet`  
 から無効.  
  
 `pcwchUsed`  
 入出力エラーメッセージの長さへのポインター。  
  
## <a name="return-value"></a>戻り値  

 このメソッドは、次の値に加えて、Winerror.h で定義されている標準の COM エラーコードを返します。  
  
|リターン コード|説明|  
|-----------------|-----------------|  
|S_OK|メソッドは正常に完了しました。|  
|E_INVALIDARG|`szBuffer` が null であるか、または `iMax` がゼロ (0) です。|  
  
## <a name="remarks"></a>解説  

 メソッドが正常に完了しなかった場合、には `szBuffer` 空の文字列が含まれます。  
  
## <a name="requirements"></a>要件  

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。  
  
 **ヘッダー:** Mscoree.dll  
  
 **ライブラリ:** MSCorEE.dll  
  
 **.NET Framework のバージョン:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.Globalization.CultureInfo.LCID%2A?displayProperty=nameWithType>
- [LoadStringRC 関数](loadstringrc-function.md)
- [非推奨の CLR ホスト関数](deprecated-clr-hosting-functions.md)
