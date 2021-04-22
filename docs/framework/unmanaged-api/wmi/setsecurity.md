---
title: SetSecurity 関数 (アンマネージド API リファレンス)
description: SetSecurity 関数を使用すると、現在のスレッドの偽装トークンが取得されます。
ms.date: 11/06/2017
api_name:
- SetSecurity
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- SetSecurity
helpviewer_keywords:
- SetSecurity function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 809f6a95fdd6854b3a591b496877838c48d52199
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79176735"
---
# <a name="setsecurity-function"></a>SetSecurity 関数

現在のスレッドに関連付けられている偽装トークンが取得されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT SetSecurity (
   [out] boolean* pNeedToReset,
   [out] HANDLE* pCurrentThreadToken
);
```

## <a name="parameters"></a>パラメーター

`pNeedToReset`\
[out] 関数から制御が戻った時点では、[ResetSecurity](resetsecurity.md) 関数を呼び出すことによってトークンをリセットする必要があるかどうかを示す、`boolean` へのポインターが格納されています。

`token`\
[out] 関数から制御が戻った時点では、現在のスレッドに関連付けられている偽装トークンのハンドルへのポインターが格納されています。 現在のスレッドに関連付けられているトークンが存在しない場合、その値は `null` の可能性があります。

## <a name="return-value"></a>戻り値

関数が成功した場合の戻り値は `S_OK` (0) です。

関数が失敗した場合の戻り値はゼロ以外のエラー コードです。 拡張されたエラー情報を取得するには、[GetErrorInfo](geterrorinfo.md) 関数を呼び出します。

## <a name="requirements"></a>必要条件

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

 **ヘッダー:** WMINet_Utils.idl

 **.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
