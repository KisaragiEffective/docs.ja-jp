---
title: EndEnumeration 関数 (アンマネージ API リファレンス)
description: EndEnumeration 関数は、列挙体を終了します。
ms.date: 11/06/2017
api_name:
- EndEnumeration
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- EndEnumeration
helpviewer_keywords:
- EndEnumeration function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: b9fd1f094c8fb56c94421a07437aa25a3549c487
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73132045"
---
# <a name="endenumeration-function"></a>EndEnumeration 関数

[Beginenumeration 関数](beginenumeration.md)の呼び出しで開始された列挙シーケンスを終了します。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT EndEnumeration (
   [in] int               vFunc,
   [in] IWbemClassObject* ptr
);
```

## <a name="parameters"></a>パラメーター

`vFunc`\
からこのパラメーターは使用されていません。

`ptr`\
から[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)インスタンスへのポインター。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|`WBEM_E_FAILED` | 0x80041001 | 一般的なエラーが発生しました。 |
|`WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。  |

## <a name="remarks"></a>Remarks

この関数は、 [IWbemClassObject:: EndEnumeration](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)メソッドの呼び出しをラップします。

`EndEnumeration` 関数の呼び出しは必須ではありませんが、列挙型に関連付けられているリソースを解放するためお勧めします。 ただし、次の列挙が開始されるか、オブジェクトが解放されると、リソースは自動的に割り当て解除されます。

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
