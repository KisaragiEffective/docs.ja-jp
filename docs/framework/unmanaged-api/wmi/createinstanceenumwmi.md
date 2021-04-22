---
title: CreateInstanceEnumWmi 関数 (アンマネージド API リファレンス)
description: CreateInstanceEnumWmi 関数を使用すると、選択条件を満たす、指定したクラスのインスタンスを含む列挙子が返されます。
ms.date: 11/06/2017
api_name:
- CreateInstanceEnumWmi
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- CreateInstanceEnumWmi
helpviewer_keywords:
- CreateInstanceEnumWmi function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 9ffa718be0e8b67471fdf8cb277df201388d2840
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130404"
---
# <a name="createinstanceenumwmi-function"></a>CreateInstanceEnumWmi 関数

指定したクラスのインスタンスの中から指定した選択条件を満たすものを返す列挙子が返されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT CreateInstanceEnumWmi (
   [in] BSTR                    strFilter,
   [in] long                    lFlags,
   [in] IWbemContext*           pCtx,
   [out] IEnumWbemClassObject** ppEnum,
   [in] DWORD                   authLevel,
   [in] DWORD                   impLevel,
   [in] IWbemServices*          pCurrentNamespace,
   [in] BSTR                    strUser,
   [in] BSTR                    strPassword,
   [in] BSTR                    strAuthority
);
```

## <a name="parameters"></a>パラメーター

`strFilter`\
[in] 目的のインスタンスのクラスの名前。 このパラメーターを `null` とすることはできません。

`lFlags`\
[in] この関数の動作に影響を与えるフラグの組み合わせ。 次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_FLAG_USE_AMENDED_QUALIFIERS` | 0x20000 | 設定すると、関数により、現在の接続のロケールのローカライズされた名前空間に格納されている修正された修飾子が取得されます。 <br/> 設定されていない場合、関数では、直近の名前空間に格納されている修飾子だけが取得されます。 |
| `WBEM_FLAG_DEEP` | 0 | 列挙には、これと、階層内のすべてのサブクラスが含まれます。 |
| `WBEM_FLAG_SHALLOW` | 1 | 列挙には、このクラスの純粋なインスタンスだけが含まれ、このクラス内に見つからないプロパティを指定するサブクラスのインスタンスはすべて除外されます。 |
| `WBEM_FLAG_RETURN_IMMEDIATELY` | 0x10 | このフラグにより、半同期呼び出しが発生します。 |
| `WBEM_FLAG_FORWARD_ONLY` | 0x20 | 関数によって順方向専用の列挙子が返されます。 通常、順方向専用の列挙子は、従来の列挙子よりも高速で、使用されるメモリが少なくなりますが、[Clone](clone.md) の呼び出しは許可されません。 |
| `WBEM_FLAG_BIDIRECTIONAL` | 0 | WMI では、列挙内のオブジェクトが解放されるまで、それらに対するポインターが保持されます。 |

最適なパフォーマンスを得るために推奨されるフラグは `WBEM_FLAG_RETURN_IMMEDIATELY` と `WBEM_FLAG_FORWARD_ONLY` です。

`pCtx`\
[in] 通常、この値は `null` です。 それ以外の場合は、要求されたインスタンスを提供しているプロバイダーが使用できる [IWbemContext](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext) インスタンスへのポインターです。

`ppEnum`\
[out] 列挙子へのポインターを受け取ります。

`authLevel`\
[in] 承認レベル。

`impLevel`\
[in] 偽装レベル。

`pCurrentNamespace`\
[in] 現在の名前空間を表す [IWbemServices](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemservices) オブジェクトへのポインター。

`strUser`\
[in] ユーザー名。 詳細については、「[ConnectServerWmi](connectserverwmi.md) 関数」を参照してください。

`strPassword`\
[in] パスワード。 詳細については、「[ConnectServerWmi](connectserverwmi.md) 関数」を参照してください。

`strAuthority`\
[in] ユーザーのドメイン名。 詳細については、「[ConnectServerWmi](connectserverwmi.md) 関数」を参照してください。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_E_ACCESS_DENIED` | 0x80041003 | 指定されたクラスのインスタンスを表示するためのアクセス許可がユーザーにありません。 |
| `WBEM_E_FAILED` | 0x80041001 | 特定できないエラーが発生しました。 |
| `WBEM_E_INVALID_CLASS` | 0x80041010 | `strFilter` は存在しません。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが無効です。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | メモリ不足のため、操作を完了できません。 |
| `WBEM_E_SHUTTING_DOWN` | 0x80041033 | WMI が停止し、再起動されている可能性があります。 [ConnectServerWmi](connectserverwmi.md) をもう一度呼び出してください。 |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | 現在のプロセスと WMI の間のリモート プロシージャ コール (RPC) リンクが失敗しました。 |
|`WBEM_S_NO_ERROR` | 0 | 関数呼び出しに成功しました。  |

## <a name="remarks"></a>解説

この関数では、[IWbemServices::CreateClassEnum](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemservices-createinstanceenum) メソッドの呼び出しがラップされます。

返される列挙子に要素が含まれない場合があることに注意してください。

関数呼び出しが失敗した場合は、[GetErrorInfo](geterrorinfo.md) 関数を呼び出して追加のエラー情報を取得できます。

## <a name="requirements"></a>必要条件

**:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils.idl

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
