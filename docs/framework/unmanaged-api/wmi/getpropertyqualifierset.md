---
title: GetPropertyQualifierSet 関数 (アンマネージド API リファレンス)
description: GetPropertyQualifierSet 関数を使用すると、プロパティに設定されている修飾子が取得されます。
ms.date: 11/06/2017
api_name:
- GetPropertyQualifierSet
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- GetPropertyQualifierSet
helpviewer_keywords:
- GetPropertyQualifierSet function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 4133145c7bea1fb3c018d809b9fea3de38270619
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127456"
---
# <a name="getpropertyqualifierset-function"></a>GetPropertyQualifierSet 関数

特定のプロパティで設定された修飾子が取得されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT GetPropertyQualifierSet (
   [in] int                 vFunc,
   [in] IWbemClassObject*   ptr,
   [in] LPCWSTR             wszProperty,
   [out] IWbemQualifierSet  **ppQualSet
);
```

## <a name="parameters"></a>パラメーター

`vFunc`\
[in] このパラメーターは使用されません。

`ptr`\
[in] [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) インスタンスへのポインター。

`wszMethod`\
[in] プロパティの名前。 `wszProperty` は有効な `LPCWSTR` を指している必要があります。

`ppQualSet`\
[out] プロパティの修飾子へのアクセスを許可するインターフェイス ポインターを受け取ります。 `ppQualSet` として `null` を使用することはできません。 エラーが発生した場合、新しいオブジェクトは返されず、ポインターは `null` を指すように設定されます。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|`WBEM_E_FAILED` | 0x80041001 | 一般的なエラーが発生しました。 |
| `WBEM_E_NOT_FOUND` | 0x80041002 | 指定されたメソッドは存在しません。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | メモリ不足のため、操作を完了できません。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが `null` です。 |
| `WBEM_E_SYSTEM_PROPERTY` | 0x80041030 | 関数で、システム プロパティの修飾子の取得が試みられています。 |
|`WBEM_S_NO_ERROR` | 0 | 関数呼び出しに成功しました。  |

## <a name="remarks"></a>解説

この関数では、[IWbemClassObject::GetPropertyQualifierSet](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-getpropertyqualifierset) メソッドの呼び出しがラップされます。

この関数の呼び出しは、現在のオブジェクトが CIM クラス定義である場合にのみサポートされます。 CIM インスタンスを指す [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) ポインターでは、メソッド操作を使用できません。

各メソッドには独自の修飾子を指定できるため、[IWbemQualifierSet ポインター](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset)を使用すると呼び出し元はこれらの修飾子を追加、編集、または削除できます。

システム プロパティには修飾子がないため、システム プロパティの [IWbemQualifierSet](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemqualifierset) ポインターを取得しようとすると、関数から `WBEM_E_SYSTEM_PROPERTY` が返されます。

## <a name="requirements"></a>必要条件

**:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils.idl

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
