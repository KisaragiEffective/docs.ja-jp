---
title: Put 関数 (アンマネージド API リファレンス)
description: Put 関数を使用すると、名前付きプロパティに新しい値が割り当てられます。
ms.date: 11/06/2017
api_name:
- Put
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- Put
helpviewer_keywords:
- Put function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: f1bb8aa09a269e3b8fd23f393d63a275d308a77c
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127403"
---
# <a name="put-function"></a>Put 関数

名前付きプロパティが新しい値に設定されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT Put (
   [in] int               vFunc,
   [in] IWbemClassObject* ptr,
   [in] LPCWSTR           wszName,
   [in] LONG              lFlags,
   [in] VARIANT*          pVal,
   [in] CIMTYPE           vtType
);
```

## <a name="parameters"></a>パラメーター

`vFunc`\
[in] このパラメーターは使用されません。

`ptr`\
[in] [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) インスタンスへのポインター。

`wszName`\
[in] プロパティの名前。 このパラメーターを `null` とすることはできません。

`lFlags`\
[in] 予約されています。 このパラメーターは、0 にする必要があります。

`pVal`\
[in] 新しいプロパティ値になる有効な `VARIANT` へのポインター。 `pVal` が `null` であるか、`VT_NULL` 型の `VARIANT` を指している場合、プロパティは `null` に設定されます。

`vtType`\
[in] `pVal` が指している `VARIANT` の型。 詳細については、「[解説](#remarks)」セクションを参照してください。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|`WBEM_E_FAILED` | 0x80041001 | 一般エラーが発生しました。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | 1 つ以上のパラメーターが無効です。 |
|`WBEM_E_INVALID_PROPERTY_TYPE` | 0x8004102a | プロパティ型が認識されません。 この値は、クラスが既に存在する場合に、クラスのインスタンスを作成すると返されます。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | メモリ不足のため、操作を完了できません。 |
| `WBEM_E_TYPE_MISMATCH` | 0x80041005 | インスタンスの場合: `pVal` がプロパティに対して正しくない型の `VARIANT` を指していることを示します。 <br/> クラス定義の場合: プロパティは既に親クラスに存在し、新しい COM 型が古い COM 型と異なります。 |
|`WBEM_S_NO_ERROR` | 0 | 関数呼び出しに成功しました。 |

## <a name="remarks"></a>解説

この関数では、[IWbemClassObject::Put](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-put) メソッドの呼び出しがラップされます。

この関数は、常に、現在のプロパティ値を新しい値で上書きします。 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) がクラス定義を指している場合は、`Put` によりプロパティの値が作成または更新されます。 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) が CIM インスタンスを指している場合は、`Put` によりプロパティの値の更新のみが行われます。`Put` でプロパティの値を作成することはできません。

`__CLASS` システム プロパティは、クラスの作成時に、空白のままにできない場合にのみ書き込み可能です。 その他のシステム プロパティはすべて読み取り専用です。

ユーザーは、アンダースコア ("_") で開始または終了する名前を持つプロパティを作成することはできません。 これは、システム クラスおよびプロパティ用に予約されています。

`Put` 関数によって設定されるプロパティが親クラスに存在する場合、プロパティの型が親クラスの型と一致していない場合を除き、プロパティの既定値が変更されます。 プロパティが存在せず、型が一致しない場合は、プロパティが作成されます。

CIM クラス定義に新しいプロパティを作成し、`pVal` が `null` であるか、`VT_NULL` 型の `VARIANT` を指している場合にのみ、`vtType` パラメーターを使用します。 この場合、`vType` パラメーターでプロパティの CIM 型を指定します。 それ以外のすべての場合、`vtType` は 0 である必要があります。 基になるオブジェクトがインスタンスである場合は (`Val` が `null` の場合でも)、プロパティの型は固定され、変更できないため、`vtType` も 0 にする必要があります。

## <a name="example"></a>例

例については、「[IWbemClassObject::Put](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-put) メソッド」を参照してください。

## <a name="requirements"></a>必要条件

**:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils.idl

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
