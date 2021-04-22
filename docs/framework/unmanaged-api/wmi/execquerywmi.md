---
title: ExecQueryWmi 関数 (アンマネージド API リファレンス)
description: ExecQueryWmi 関数を使用すると、オブジェクトを取得するためのクエリが実行されます。
ms.date: 11/06/2017
api_name:
- ExecQueryWmi
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- ExecQueryWmi
helpviewer_keywords:
- ExecQueryWmi function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 3c6ea58eca5ac635893a24b57ade261e04a69721
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130435"
---
# <a name="execquerywmi-function"></a>ExecQueryWmi 関数

オブジェクトを取得するクエリが実行されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT ExecQueryWmi (
   [in] BSTR                    strQueryLanguage,
   [in] BSTR                    strQuery,
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

`strQueryLanguage`\
[in] Windows Management でサポートされている有効なクエリ言語を含む文字列。 これは、WMI Query Language の頭字語である "WQL" である必要があります。

`strQuery`\
[in] クエリのテキスト。 このパラメーターを `null` とすることはできません。

`lFlags`\
[in] この関数の動作に影響を与えるフラグの組み合わせ。 次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

| 定数 | [値]  | 説明  |
|---------|---------|---------|
| `WBEM_FLAG_USE_AMENDED_QUALIFIERS` | 0x20000 | 設定すると、関数により、現在の接続のロケールのローカライズされた名前空間に格納されている修正された修飾子が取得されます。 <br/> 設定されていない場合、関数では、直近の名前空間に格納されている修飾子だけが取得されます。 |
| `WBEM_FLAG_RETURN_IMMEDIATELY` | 0x10 | このフラグにより、半同期呼び出しが発生します。 |
| `WBEM_FLAG_FORWARD_ONLY` | 0x20 | 関数によって順方向専用の列挙子が返されます。 通常、順方向専用の列挙子は、従来の列挙子よりも高速で、使用されるメモリが少なくなりますが、[Clone](clone.md) の呼び出しは許可されません。 |
| `WBEM_FLAG_BIDIRECTIONAL` | 0 | WMI では、列挙内のオブジェクトが解放されるまで、それらに対するポインターが保持されます。 |
| `WBEM_FLAG_ENSURE_LOCATABLE` | 0x100 | **__PATH**、 **__RELPATH**、 **__SERVER** などのシステム プロパティが `null` にならないように、返されるオブジェクトに十分な情報が含まれるようにします。 |
| `WBEM_FLAG_PROTOTYPE` | 2 | このフラグは、プロトタイプ作成のために使用します。 クエリは実行されず、代わりに通常の結果オブジェクトのように見えるオブジェクトが返されます。 |
| `WBEM_FLAG_DIRECT_READ` | 0x200 | 親クラスまたはサブクラスに関係なく、指定したクラスのプロバイダーに直接アクセスします。 |

最適なパフォーマンスを得るために推奨されるフラグは `WBEM_FLAG_RETURN_IMMEDIATELY` と `WBEM_FLAG_FORWARD_ONLY` です。

`pCtx`\
[in] 通常、この値は `null` です。 それ以外の場合は、要求されたクラスを提供しているプロバイダーが使用できる [IWbemContext](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext) インスタンスへのポインターです。

`ppEnum`\
[out] エラーが発生しなかった場合、呼び出し元がクエリの結果セット内のインスタンスを取得できるようにする列挙子へのポインターを受け取ります。 クエリの結果セットには、インスタンスが含まれない場合があります。 詳細については、「[解説](#remarks)」セクションを参照してください。

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
| `WBEM_E_ACCESS_DENIED` | 0x80041003 | ユーザーに、関数から返される可能性がある 1 つ以上のクラスを表示するアクセス許可がありません。 |
| `WBEM_E_FAILED` | 0x80041001 | 特定できないエラーが発生しました。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが無効です。 |
| `WBEM_E_INVALID_QUERY` | 0x80041017 | クエリで構文エラーが発生しました。 |
| `WBEM_E_INVALID_QUERY_TYPE` | 0x80041018 | 要求したクエリ言語がサポートされていません。 |
| `WBEM_E_QUOTA_VIOLATION` | 0x8004106c | クエリが複雑すぎます。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | メモリ不足のため、操作を完了できません。 |
| `WBEM_E_SHUTTING_DOWN` | 0x80041033 | WMI が停止し、再起動されている可能性があります。 [ConnectServerWmi](connectserverwmi.md) をもう一度呼び出してください。 |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | 現在のプロセスと WMI の間のリモート プロシージャ コール (RPC) リンクが失敗しました。 |
| `WBEM_E_NOT_FOUND` | 0x80041002 | 存在しないクラスがクエリに指定されています。 |
| `WBEM_S_NO_ERROR` | 0 | 関数呼び出しに成功しました。  |

## <a name="remarks"></a>解説

この関数では、[IWbemServices::ExecQuery](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemservices-execquery) メソッドの呼び出しがラップされます。

この関数を使用すると、`strQuery` パラメーターで指定したクエリが処理されて、呼び出し元がクエリ結果にアクセスできる列挙子が作成されます。 列挙子は、[IEnumWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-ienumwbemclassobject) インターフェイスへのポインターです。クエリの結果は、[IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject) インターフェイスを通じて使用できるようになるクラス オブジェクトのインスタンスです。

WQL クエリで使用できる `AND` と`OR` のキーワードの数には制限があります。 複雑なクエリで使用される WQL キーワードの数が多いと、WMI が `WBEM_E_QUOTA_VIOLATION` (または 0x8004106c) エラー コードを `HRESULT` 値として返すことがあります。 WQL キーワードの制限は、クエリの複雑さによって異なります。

関数呼び出しが失敗した場合は、[GetErrorInfo](geterrorinfo.md) 関数を呼び出して追加のエラー情報を取得できます。

## <a name="requirements"></a>必要条件

**:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils.idl

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
