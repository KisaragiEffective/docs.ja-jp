---
title: QualifierSet_Put 関数 (アンマネージド API リファレンス)
description: QualifierSet_Put 関数を使用すると、名前付きの修飾子とその値が書き込まれます。
ms.date: 11/06/2017
api_name:
- QualifierSet_Put
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- QualifierSet_Put
helpviewer_keywords:
- QualifierSet_Put function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: a35025c6d16455a51b7b22d822ba77337ddd894a
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73120235"
---
# <a name="qualifierset_put-function"></a>QualifierSet_Put 関数

名前付き修飾子と値が書き込まれます。 新しい修飾子は、同じ名前の前の値を上書きします。 修飾子が存在しない場合は、作成されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT QualifierSet_Put (
   [in] int                  vFunc,
   [in] IWbemQualifierSet*   ptr,
   [in] LPCWSTR              wszName,
   [in] VARIANT*             pVal,
   [in] LONG                 lFlavor
);
```

## <a name="parameters"></a>パラメーター

`vFunc`\
[in] このパラメーターは使用されません。

`ptr`\
[in] [IWbemQualifierSet](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset) インスタンスへのポインター。

`wszName`\
[in] 書き込む修飾子の名前。

`pVal`\
[in] 書き込む修飾子が格納されている有効な `VARIANT` へのポインター。 このパラメーターを `null` とすることはできません。

`lFlavor`\
[in] この修飾子に必要な修飾子フレーバーを定義する、次のいずれかの定数。 既定値は `WBEM_FLAVOR_OVERRIDABLE` (0) です。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_FLAVOR_OVERRIDABLE` | 0 | この修飾子は、派生クラスまたはインスタンスでオーバーライドできます。 **これが既定値です。** |
| `WBEM_FLAVOR_FLAG_PROPAGATE_TO_INSTANCE` | 1 | この修飾子は、インスタンスに反映されます。 |
| `WBEM_FLAVOR_FLAG_PROPAGATE_TO_DERIVED_CLASS` | 2 | この修飾子は、派生クラスに反映されます。 |
| `WBEM_FLAVOR_NOT_OVERRIDABLE` | 0x10 | この修飾子は、派生クラスまたはインスタンスではオーバーライドできません。 |
| `WBEM_FLAVOR_AMENDED` | 0x80 | 修飾子はローカライズされています。 |

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_E_CANNOT_BE_KEY` | 0x8004101f | キーを指定できないプロパティに対して、**Key** 修飾子を指定しようとしました。 キーは、オブジェクトのクラス定義で指定されるため、インスタンスごとには変更できません。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが無効です。 |
| `WBEM_E_INVALID_QUALIFIER_TYPE` | 0x80041029 | `pVal` パラメーターは、修飾子の有効な型ではありません。 |
| `WBEM_E_OVERRIDE_NOT_ALLOWED` | 0x8004101a | 所有オブジェクトでオーバーライドが許可されていないため、修飾子に対して `QualifierSet_Put` メソッドを呼び出すことはできません。 |
| `WBEM_S_NO_ERROR` | 0 | 関数呼び出しに成功しました。  |

## <a name="remarks"></a>解説

この関数では、[IWbemQualifierSet::Put](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemqualifierset-put) メソッドの呼び出しがラップされます。

## <a name="requirements"></a>必要条件

**:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils.idl

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
