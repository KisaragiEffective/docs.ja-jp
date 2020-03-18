---
title: ExecQueryWmi 関数 (アンマネージ API リファレンス)
description: ExecQueryWmi 関数は、オブジェクトを取得するクエリを実行します。
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
ms.translationtype: MT
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
からWindows Management でサポートされている有効なクエリ言語を含む文字列。 これは、WMI Query Language の頭字語である "WQL" である必要があります。

`strQuery`\
からクエリのテキスト。 このパラメーターを `null` とすることはできません。

`lFlags`\
からこの関数の動作に影響を与えるフラグの組み合わせ。 次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

| 定数 | [値]  | 説明  |
|---------|---------|---------|
| `WBEM_FLAG_USE_AMENDED_QUALIFIERS` | 0x20000 | 設定すると、関数は、現在の接続のロケールのローカライズされた名前空間に格納されている修正された修飾子を取得します。 <br/> 設定されていない場合、関数は、イミディエイト名前空間に格納されている修飾子だけを取得します。 |
| `WBEM_FLAG_RETURN_IMMEDIATELY` | 0x10 | このフラグにより、半同期呼び出しが発生します。 |
| `WBEM_FLAG_FORWARD_ONLY` | 0x20 | 関数は、順方向専用の列挙子を返します。 通常、順方向専用の列挙子は、従来の列挙子よりも高速で使用されるメモリが少なくなりますが、[複製](clone.md)の呼び出しは許可されません。 |
| `WBEM_FLAG_BIDIRECTIONAL` | 0 | WMI は、列挙体が解放されるまで、そのオブジェクトへのポインターを保持します。 |
| `WBEM_FLAG_ENSURE_LOCATABLE` | 0x100 | 返されたオブジェクトに十分な情報が含まれていることを確認して、 **__ PATH**、 **__RELPATH**、 **__ SERVER**などのシステムプロパティが `null`ないようにします。 |
| `WBEM_FLAG_PROTOTYPE` | 2 | このフラグは、プロトタイプを行うために使用されます。 クエリは実行されず、代わりに通常の結果オブジェクトのように見えるオブジェクトを返します。 |
| `WBEM_FLAG_DIRECT_READ` | 0x200 | 親クラスまたはサブクラスに関係なく、指定されたクラスのプロバイダーに直接アクセスします。 |

最適なパフォーマンスを得るために、推奨されるフラグは `WBEM_FLAG_RETURN_IMMEDIATELY` と `WBEM_FLAG_FORWARD_ONLY` です。

`pCtx`\
から通常、この値は `null`です。 それ以外の場合は、要求されたクラスを提供しているプロバイダーが使用できる[IWbemContext](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext)インスタンスへのポインターです。

`ppEnum`\
入出力エラーが発生しなかった場合、は、呼び出し元がクエリの結果セット内のインスタンスを取得できるようにする列挙子へのポインターを受け取ります。 クエリには、インスタンスがゼロの結果セットを含めることができます。 詳細については、「[解説](#remarks)」を参照してください。

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
| `WBEM_E_INVALID_QUERY` | 0x80041017 | クエリで構文エラーが発生しました。 |
| `WBEM_E_INVALID_QUERY_TYPE` | 0x80041018 | 要求されたクエリ言語はサポートされていません。 |
| `WBEM_E_QUOTA_VIOLATION` | 0x8004106c | クエリが複雑すぎます。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 操作を完了するために必要なメモリが不足しています。 |
| `WBEM_E_SHUTTING_DOWN` | 0x80041033 | WMI が停止し、再起動されたことがあります。 [Connectserverwmi](connectserverwmi.md)を再度呼び出します。 |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | 現在のプロセスと WMI の間のリモートプロシージャコール (RPC) リンクが失敗しました。 |
| `WBEM_E_NOT_FOUND` | 0x80041002 | このクエリでは、存在しないクラスが指定されています。 |
| `WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。  |

## <a name="remarks"></a>Remarks

この関数は、 [IWbemServices:: ExecQuery](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemservices-execquery)メソッドの呼び出しをラップします。

この関数は、`strQuery` パラメーターで指定されたクエリを処理し、呼び出し元がクエリ結果にアクセスするための列挙子を作成します。 列挙子は、 [IEnumWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-ienumwbemclassobject)インターフェイスへのポインターです。クエリの結果は、 [IWbemClassObject](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemclassobject)インターフェイスを通じて使用できるクラスオブジェクトのインスタンスです。

WQL クエリで使用できる `AND` および `OR` キーワードの数には制限があります。 複雑なクエリで使用される WQL キーワードの数が多いと、WMI が `WBEM_E_QUOTA_VIOLATION` (または 0x8004106c) エラーコードを `HRESULT` 値として返すことがあります。 WQL キーワードの制限は、クエリの複雑さによって異なります。

関数呼び出しが失敗した場合は、 [GetErrorInfo](geterrorinfo.md)関数を呼び出して追加のエラー情報を取得できます。

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
