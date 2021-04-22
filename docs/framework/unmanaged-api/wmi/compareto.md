---
title: CompareTo 関数 (アンマネージド API リファレンス)
description: CompareTo 関数を使用すると、オブジェクトと別の WMI オブジェクトが比較されます。
ms.date: 11/06/2017
api_name:
- CompareTo
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- CompareTo
helpviewer_keywords:
- CompareTo function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 0d210795016cd2e0179b902a224ca0c62f4ac01f
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128705"
---
# <a name="compareto-function"></a>CompareTo 関数

オブジェクトが、別の Windows 管理オブジェクトと比較されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT CompareTo (
   [in] int               vFunc,
   [in] IWbemClassObject* ptr,
   [in] LONG              flags,
   [in] IWbemClassObject* pCompareTo
);
```

## <a name="parameters"></a>パラメーター

`vFunc`\
[in] このパラメーターは使用されません。

`ptr`\
[in] [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) インスタンスへのポインター。

`flags`\
[in] 比較で考慮するオブジェクトの特性を指定するフラグのビットごとの組み合わせ。 詳細については、「[解説](#remarks)」セクションを参照してください。

`pCompareTo`\
[in] 比較対象のオブジェクト。 `pCompareTo` は有効な [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) インスタンスである必要があります。`null` にすることはできません。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_E_FAILED` | 0x80041001 | 特定できないエラーが発生しました。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが無効です。 |
| `WBEM_E_UNEXPECTED` | 0x8004101d | `BeginEnumeration` への 2 回目の呼び出しは、[`EndEnumeration`](endenumeration.md) への中間呼び出しなしで行われました。 |
| `WBEM_S_NO_ERROR` | 0 | 関数呼び出しに成功しました。  |
| `WBEM_S_DIFFERENT` | 0x40003 | オブジェクトが異なっています。 |
| `WBEM_S_SAME` | 0 | オブジェクトは、比較フラグに基づいて同じものです。 |

## <a name="remarks"></a>解説

この関数では、[IWbemClassObject::CompareTo](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-compareto) メソッドの呼び出しがラップされます。

`lEnumFlags` 引数として渡すことができるフラグは、*WbemCli.h* ヘッダー ファイル内で定義されているか、コード内で定数として定義することができます。 次のフラグのビットごとの組み合わせを指定することで、比較に含める個々の特性を指定できます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_FLAG_IGNORE_OBJECT_SOURCE` | 2 | ソース (元になっているサーバーと名前空間) を無視します。 |
| `WBEM_FLAG_IGNORE_QUALIFIERS` | 1 | すべての修飾子を無視します (**Key** と **Dynamic** を含みます) |
| `WBEM_FLAG_IGNORE_DEFAULT_VALUES` | 4 | プロパティの既定値を無視します。 このフラグは、クラスの比較にのみ適用されます。 |
| `WBEM_FLAG_IGNORE_FLAVOR` | 0x20 | 修飾子のフレーバーを無視します。 このフラグは、修飾子を考慮に入れますが、反映規則やオーバーライド制限などのフレーバーの区別を無視します。 |
| `WBEM_FLAG_IGNORE_CASE` | 0x10 | 文字列値の比較で大文字と小文字を区別しません。 これは、文字列と修飾子値の両方に適用されます。 プロパティと修飾子の名前の比較では、このフラグが設定されているかどうかに関係なく、常に大文字と小文字が区別されます。 |
| `WBEM_FLAG_IGNORE_CLASS` | 0x8 | 比較するオブジェクトが同じクラスのインスタンスであると見なします。 結果として、このフラグを指定すると、インスタンス関連の情報のみが比較されます。 パフォーマンスを最適化するには、このフラグを使用します。 オブジェクトが同じクラスではない場合、結果は未定義になります。 |

または、次のように 1 つの複合フラグを指定することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|`WBEM_COMPARISON_INCLUDE_ALL` | 0 | 比較ですべての特徴を考慮します。 |

## <a name="requirements"></a>必要条件

**:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils.idl

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
