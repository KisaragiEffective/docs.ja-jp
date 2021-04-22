---
title: Delete 関数 (アンマネージド API リファレンス)
description: Delete 関数を使用すると、指定したプロパティとそのすべての修飾子が、CIM クラス定義から削除されます。
ms.date: 11/06/2017
api_name:
- Delete
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- Delete
helpviewer_keywords:
- Delete function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 6b8f287be831702dd31a8335f9b2f6447bcee540
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127664"
---
# <a name="delete-function"></a>Delete 関数

指定したプロパティとそのすべての修飾子が、CIM クラス定義から削除されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT Delete (
   [in] int               vFunc,
   [in] IWbemClassObject* ptr,
   [in] LPCWSTR           wszName
);
```

## <a name="parameters"></a>パラメーター

`vFunc`\
[in] このパラメーターは使用されません。

`ptr`\
[in] [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) インスタンスへのポインター。

`wszName`\
[in] 削除するプロパティの名前。 `wszName` は有効な `LPCWSTR` へのポインターである必要があります。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_E_FAILED` | 0x80041001 | 特定できないエラーが発生しました。 |
| `WBEM_E_INVALID_OPERATION` | 0x80041016 | プロパティを削除できません。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | `wszName` が無効です。 |
| `WBEM_E_NOT_FOUND` | 0x80041002 | 指定されたプロパティが存在しません。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 操作を完了させるための十分なメモリがありません。 |
| `WBEM_E_PROPAGATED_PROPERTY` | 0x8004101c | プロパティは、基底クラスから継承されています。 |
| `WBEM_E_SYSTEM_PROPERTY` | | プロパティはシステム プロパティです。 |
|`WBEM_S_NO_ERROR` | 0 | 関数呼び出しに成功しました。  |
| `WBEM_E_RESET_TO_DEFAULT` | 0x80041030 | 関数により、現在のクラスのオーバーライドの既定値が削除されました。 親クラスのこのプロパティの既定値が、再アクティブ化されています。 |

## <a name="remarks"></a>解説

この関数では、[IWbemClassObject::Delete](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-delete) メソッドの呼び出しがラップされます。

## <a name="requirements"></a>必要条件

**:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils.idl

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
