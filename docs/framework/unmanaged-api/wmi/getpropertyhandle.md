---
title: GetPropertyHandle 関数 (アンマネージド API リファレンス)
description: GetPropertyHandle 関数を使用すると、プロパティを識別する一意のハンドルが返されます。
ms.date: 11/06/2017
api_name:
- GetPropertyHandle
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- GetPropertyHandle
helpviewer_keywords:
- GetPropertyHandle function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 5af003f0295e0b403727f9af6b03ab81c4b8bccb
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73101870"
---
# <a name="getpropertyhandle-function"></a>GetPropertyHandle 関数

プロパティを識別する一意のハンドルが返されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT GetPropertyHandle (
   [in] int                  vFunc,
   [in] IWbemObjectAccess*   ptr,
   [in] LPCWSTR              wszPropertyName,
   [out] CIMTYPE*            pType,
   [out] long*               pHandle
);
```

## <a name="parameters"></a>パラメーター

`vFunc`\
[in] このパラメーターは使用されません。

`ptr`\
[in] [IWbemObjectAccess](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemobjectaccess) インスタンスへのポインター。

`wszPropertyName`\
[in] プロパティ名が格納されている、UTF16 でエンコードされた文字の null で終わる文字列。

`pType`\
[out] プロパティの CIM 型を表す [`CIMTYPE`](/windows/win32/api/wbemcli/ne-wbemcli-cimtype_enumeration) 列挙型メンバーへのポインター。

`pHandle`\
[out] プロパティ ハンドルが格納されている整数へのポインター。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|`WBEM_E_NOT_FOUND` | 0x80041002 | 指定したプロパティ名が見つかりませんでした。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが無効です。 |
|`WBEM_E_NOT_SUPPORTED` | 0x8004100c | 要求されたプロパティの型は `CIM_OBJECT` または `CIM_ARRAY` です。 |
|`WBEM_S_NO_ERROR` | 0 | 関数呼び出しに成功しました。  |

## <a name="remarks"></a>解説

この関数では、[IWbemClassObject::GetPropertyHandle](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemobjectaccess-getpropertyhandle) メソッドの呼び出しがラップされます。

[IWbemObjectAccess](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemobjectaccess) メソッドを使用してプロパティ値の読み取りまたは書き込みを行うときに、このハンドルを使用してプロパティを識別できます。

`CIM_OBJECT` および `CIM_ARRAY` 以外のすべてのデータ型のプロパティのハンドルを取得できます。 返されるハンドルは、クラスのすべてのインスタンスで機能します。

## <a name="requirements"></a>必要条件

**:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils.idl

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
