---
title: ConnectServerWmi 関数 (アンマネージ API リファレンス)
description: ConnectServerWmi 関数は、DCOM を使用して WMI 名前空間への接続を作成します。
ms.date: 11/06/2017
api_name:
- ConnectServerWmi
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- ConnectServerWmi
helpviewer_keywords:
- ConnectServerWmi function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 25a73060ed242fd0e77042cd0ea9618b9b27250f
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128691"
---
# <a name="connectserverwmi-function"></a>ConnectServerWmi 関数

指定したコンピューターにある WMI 名前空間との接続が DCOM 経由で作成されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT ConnectServerWmi (
   [in] BSTR               strNetworkResource,
   [in] BSTR               strUser,
   [in] BSTR               strPassword,
   [in] BSTR               strLocale,
   [in] long               lSecurityFlags,
   [in] BSTR               strAuthority,
   [in] IWbemContext*      pCtx,
   [out] IWbemServices**   ppNamespace,
   [in] DWORD              impLevel,
   [in] DWORD              authLevel
);
```

## <a name="parameters"></a>パラメーター

`strNetworkResource`\
から正しい WMI 名前空間のオブジェクトパスを含む有効な `BSTR` へのポインター。 詳細については、「[解説](#remarks)」を参照してください。

`strUser`\
からユーザー名を含む有効な `BSTR` へのポインター。 `null` 値は、現在のセキュリティコンテキストを示します。 ユーザーが現在のドメインとは異なるドメインにある場合、`strUser` には、円記号で区切られたドメインとユーザー名を含めることもできます。 `strUser` は、`userName@domainName`などのユーザープリンシパル名 (UPN) 形式で指定することもできます。 詳細については、「[解説](#remarks)」を参照してください。

`strPassword`\
からパスワードを格納している有効な `BSTR` へのポインター。 現在のセキュリティコンテキストを示す `null`。 空の文字列 ("") は、長さ0の有効なパスワードを示します。

`strLocale`\
から情報取得の正しいロケールを示す有効な `BSTR` へのポインター。 Microsoft ロケール識別子の場合、文字列の形式は "MS\_*xxx*" です。ここで、 *xxx*はロケール識別子 (LCID) を示す16進数形式の文字列です。 無効なロケールが指定されている場合、メソッドは、サーバーの既定のロケールが代わりに使用される Windows 7 以外の `WBEM_E_INVALID_PARAMETER` を返します。 ' Null1 ' の場合、現在のロケールが使用されます。

`lSecurityFlags`\
から`ConnectServerWmi` メソッドに渡すフラグ。 このパラメーターにゼロ (0) を指定すると、サーバーへの接続が確立された後にのみを返す `ConnectServerWmi` が呼び出されます。 これにより、サーバーが破損した場合にアプリケーションが無期限に応答しなくなる可能性があります。 その他の有効な値は次のとおりです。

| 定数  | [値]  | 説明  |
|---------|---------|---------|
| `CONNECT_REPOSITORY_ONLY` | 0x40 | 内部使用のために予約されています。 使用しないでください。 |
| `WBEM_FLAG_CONNECT_USE_MAX_WAIT` | 0x80 | `ConnectServerWmi` は2分以内で返されます。 |

`strAuthority`\
からユーザーのドメイン名。 次の値のいずれかを取ります。

| [値] | 説明 |
|---------|---------|
| 空白 | NTLM 認証が使用され、現在のユーザーの NTLM ドメインが使用されます。 `strUser` ドメイン (推奨される場所) を指定する場合は、ここで指定しないでください。 この関数は、両方のパラメーターでドメインを指定した場合に `WBEM_E_INVALID_PARAMETER` を返します。 |
| Kerberos:*プリンシパル名* | Kerberos 認証が使用され、このパラメーターには Kerberos プリンシパル名が含まれます。 |
| NTLMDOMAIN:*ドメイン名* | NT LAN Manager 認証が使用され、このパラメーターには NTLM ドメイン名が含まれます。 |

`pCtx`\
から通常、このパラメーターは `null`です。 それ以外の場合は、1つまたは複数の動的クラスプロバイダーが必要とする[IWbemContext](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext)オブジェクトへのポインターです。

`ppNamespace`\
入出力関数から制御が戻るときに、指定した名前空間にバインドされている[IWbemServices](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemservices)オブジェクトへのポインターを受け取ります。 エラーが発生したときに `null` を指すように設定されています。

`impLevel`\
から偽装レベル。

`authLevel`\
から承認レベル。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_E_FAILED` | 0x80041001 | 一般的なエラーが発生しました。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが有効ではありません。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 操作を完了するために必要なメモリが不足しています。 |
| `WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。  |

## <a name="remarks"></a>Remarks

この関数は、 [IWbemLocator:: ConnectServer](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemlocator-connectserver)メソッドの呼び出しをラップします。

既定の名前空間へのローカルアクセスの場合、`strNetworkResource` には単純なオブジェクトパス ("root\default" または "\\.\root\default") を指定できます。 COM または Microsoft と互換性のあるネットワークを使用してリモートコンピューター上の既定の名前空間にアクセスするには、"\\myserver\root\default" というコンピューター名を含めます。 コンピューター名には、DNS 名または IP アドレスを指定することもできます。 `ConnectServerWmi` 関数は、ipv6 アドレスを使用して IPv6 を実行しているコンピューターに接続することもできます。

`strUser` を空の文字列にすることはできません。 ドメインが `strAuthority`で指定されている場合は、`strUser`に含めないようにする必要があります。そうしないと、関数は `WBEM_E_INVALID_PARAMETER`を返します。

## <a name="requirements"></a>［要件］

 **:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

 **ヘッダー:** WMINet_Utils

 **.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
