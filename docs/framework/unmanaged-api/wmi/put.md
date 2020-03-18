---
title: Put 関数 (アンマネージ API リファレンス)
description: Put 関数は、名前付きプロパティに新しい値を割り当てます。
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
ms.translationtype: MT
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
からこのパラメーターは使用されていません。

`ptr`\
から[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)インスタンスへのポインター。

`wszName`\
からプロパティの名前。 このパラメーターを `null` とすることはできません。

`lFlags`\
[in] 予約されています。 このパラメーターには0を指定する必要があります。

`pVal`\
から新しいプロパティ値になる有効な `VARIANT` へのポインター。 `pVal` が `null` または `VT_NULL`型の `VARIANT` を指している場合、プロパティは `null`に設定されます。

`vtType`\
から`pVal`によってポイントされている `VARIANT` の型。 詳細については、「[解説](#remarks)」を参照してください。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
|`WBEM_E_FAILED` | 0x80041001 | 一般的なエラーが発生しました。 |
|`WBEM_E_INVALID_PARAMETER` | 0x80041008 | 1つ以上のパラメーターが無効です。 |
|`WBEM_E_INVALID_PROPERTY_TYPE` | 0x80041024 | プロパティの型が認識されません。 この値は、クラスが既に存在する場合に、クラスのインスタンスを作成するときに返されます。 |
|`WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 操作を完了するために必要なメモリが不足しています。 |
| `WBEM_E_TYPE_MISMATCH` | 0x80041005 | インスタンスの場合: `pVal` が、プロパティに対して正しくない型の `VARIANT` を指していることを示します。 <br/> クラス定義の場合: プロパティは既に親クラスに存在し、新しい COM 型は古い COM 型とは異なります。 |
|`WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。 |

## <a name="remarks"></a>Remarks

この関数は、 [IWbemClassObject::P ut](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-put)メソッドの呼び出しをラップします。

この関数は、常に現在のプロパティ値を新しい値で上書きします。 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)がクラス定義を指している場合は、`Put` によってプロパティ値が作成または更新されます。 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)が CIM インスタンスをポイントすると、`Put` はプロパティ値のみを更新します。プロパティ値を作成 `Put` ことはできません。

`__CLASS` システムプロパティは、クラスの作成時に、空白のままにできない場合にのみ書き込み可能です。 その他のすべてのシステムプロパティは読み取り専用です。

ユーザーは、アンダースコア ("_") で始まる名前のプロパティを作成することはできません。 これは、システムクラスおよびプロパティ用に予約されています。

`Put` 関数によって設定されたプロパティが親クラスに存在する場合、プロパティの型が親クラスの型と一致しない限り、プロパティの既定値が変更されます。 プロパティが存在せず、型が一致しない場合は、プロパティが作成されます。

`vtType` パラメーターは、CIM クラス定義で新しいプロパティを作成する場合にのみ使用し、`pVal` は `null` または `VT_NULL`型の `VARIANT` を指します。 この場合、`vType` パラメーターによって、プロパティの CIM 型が指定されます。 それ以外の場合は、`vtType` を0にする必要があります。 プロパティの型が固定され、変更できないため、基になるオブジェクトがインスタンスの場合 (`Val` が `null`の場合でも)、`vtType` も0にする必要があります。

## <a name="example"></a>例

例については、「 [IWbemClassObject::P ut](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemclassobject-put)メソッド」を参照してください。

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
