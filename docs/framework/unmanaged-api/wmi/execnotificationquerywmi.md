---
title: ExecNotificationQueryWmi 関数 (アンマネージ API リファレンス)
description: ExecNotificationQueryWmi 関数は、イベントを受信するためのクエリを実行します。
ms.date: 11/06/2017
api_name:
- ExecNotificationQueryWmi
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- ExecNotificationQueryWmi
helpviewer_keywords:
- ExecNotificationQueryWmi function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 3d8a7683eef52a5e91bf7aa84d5aa7db7dbdac8d
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73130451"
---
# <a name="execnotificationquerywmi-function"></a>ExecNotificationQueryWmi 関数

イベントを受信するクエリが実行されます。 呼び出しはすぐに返されます。呼び出し元は、返されたイベントの列挙子をポーリングできます。 返された列挙子を解放すると、クエリが取り消されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT ExecNotificationQueryWmi (
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
からWindows Management でサポートされている有効なクエリ言語を含む文字列。 これは、WMI Query Language の頭字語である "WQL" である必要があります。

`strQuery`\
からクエリのテキスト。 このパラメーターを `null` とすることはできません。

`lFlags`\
からこの関数の動作に影響を与える、次の2つのフラグの組み合わせ。 これらの値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

| 定数 | [値]  | 説明  |
|---------|---------|---------|
| `WBEM_FLAG_RETURN_IMMEDIATELY` | 0x10 | このフラグにより、半同期呼び出しが発生します。 このフラグが設定されていない場合、呼び出しは失敗します。 これは、イベントが連続して受信されるためです。これは、ユーザーが返された列挙子をポーリングする必要があることを意味します。 この呼び出しを無制限にブロックすると、そのようなことは不可能になります。 |
| `WBEM_FLAG_FORWARD_ONLY` | 0x20 | 関数は、順方向専用の列挙子を返します。 通常、順方向専用の列挙子は、従来の列挙子よりも高速で使用されるメモリが少なくなりますが、[複製](clone.md)の呼び出しは許可されません。 |

`pCtx`\
から通常、この値は `null`です。 それ以外の場合は、要求されたイベントを提供しているプロバイダーが使用できる[IWbemContext](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext)インスタンスへのポインターです。

`ppEnum`\
入出力エラーが発生しなかった場合、は、呼び出し元がクエリの結果セット内のインスタンスを取得できるようにする列挙子へのポインターを受け取ります。 詳細については、「[解説](#remarks)」を参照してください。

`authLevel`\
から承認レベル。

`impLevel`\
から偽装レベル。

`pCurrentNamespace`\
から現在の名前空間を表す[IWbemServices](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemservices)オブジェクトへのポインター。

`strUser`\
からユーザー名。 詳細については、「 [Connectserverwmi](connectserverwmi.md)関数」を参照してください。

`strPassword`\
からパスワード。 詳細については、「 [Connectserverwmi](connectserverwmi.md)関数」を参照してください。

`strAuthority`\
からユーザーのドメイン名。 詳細については、「 [Connectserverwmi](connectserverwmi.md)関数」を参照してください。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_E_ACCESS_DENIED` | 0x80041003 | 関数が返すことのできる1つ以上のクラスを表示するアクセス許可がユーザーにありません。 |
| `WBEM_E_FAILED` | 0x80041001 | 特定できないエラーが発生しました。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが有効ではありません。 |
| `WBEM_E_INVALID_CLASS` | 0x80041010 | このクエリでは、存在しないクラスが指定されています。 |
| `WBEMESS_E_REGISTRATION_TOO_PRECISE` | 0x80042002 | 要求されたイベント配信の精度が大きすぎます。 より大きなポーリング許容範囲を指定する必要があります。 |
| `WBEMESS_E_REGISTRATION_TOO_BROAD` | 0x80042001 | このクエリでは、Windows Management で提供されるよりも多くの情報が要求されます。 この `HRESULT` は、イベントクエリによって名前空間内のすべてのオブジェクトをポーリングする要求が発生した場合に返されます。 |
| `WBEM_E_INVALID_QUERY` | 0x80041017 | クエリで構文エラーが発生しました。 |
| `WBEM_E_INVALID_QUERY_TYPE` | 0x80041018 | 要求されたクエリ言語はサポートされていません。 |
| `WBEM_E_QUOTA_VIOLATION` | 0x8004106c | クエリが複雑すぎます。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 操作を完了するために必要なメモリが不足しています。 |
| `WBEM_E_SHUTTING_DOWN` | 0x80041033 | WMI が停止し、再起動されたことがあります。 [Connectserverwmi](connectserverwmi.md)を再度呼び出します。 |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | 現在のプロセスと WMI の間のリモートプロシージャコール (RPC) リンクが失敗しました。 |
| `WBEM_E_UNPARSABLE_QUERY` | 0x80041058 | クエリを解析できません。 |
| `WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。  |

## <a name="remarks"></a>Remarks

この関数は、 [IWbemServices:: ExecNotificationQuery](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemservices-execnotificationquery)メソッドの呼び出しをラップします。

関数が返された後、呼び出し元は、返された `ppEnum` オブジェクトを定期的に[次](next.md)の関数に渡して、イベントが使用可能かどうかを確認します。

WQL クエリで使用できる `AND` および `OR` キーワードの数には制限があります。 複雑なクエリで使用される WQL キーワードの数が多いと、WMI が `WBEM_E_QUOTA_VIOLATION` (または 0x8004106c) エラーコードを `HRESULT` 値として返すことがあります。 WQL キーワードの制限は、クエリの複雑さによって異なります。

関数呼び出しが失敗した場合は、 [GetErrorInfo](geterrorinfo.md)関数を呼び出して追加のエラー情報を取得できます。

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
