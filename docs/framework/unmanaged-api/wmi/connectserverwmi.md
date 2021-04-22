---
title: ConnectServerWmi 関数 (アンマネージド API リファレンス)
description: ConnectServerWmi 関数を使用すると、DCOM を使用して WMI 名前空間への接続が作成されます。
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
ms.translationtype: HT
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
[in] 正しい WMI 名前空間のオブジェクト パスが格納されている有効な `BSTR` へのポインター。 詳細については、「[解説](#remarks)」セクションを参照してください。

`strUser`\
[in] ユーザー名が格納されている有効な `BSTR` へのポインター。 `null` 値は、現在のセキュリティ コンテキストを示します。 現在のドメインとは異なるドメインのユーザーの場合は、円記号で区切られたドメインとユーザー名を `strUser` に格納することもできます。 `strUser` は、ユーザー プリンシパル名 (UPN) 形式で指定することもできます (`userName@domainName` など)。 詳細については、「[解説](#remarks)」セクションを参照してください。

`strPassword`\
[in] パスワードが格納されている有効な `BSTR` へのポインター。 `null` は、現在のセキュリティ コンテキストを示します。 空の文字列 ("") は、長さ 0 の有効なパスワードを示します。

`strLocale`\
[in] 情報を取得するための正しいロケールを示す有効な `BSTR` へのポインター。 Microsoft ロケール識別子の場合、文字列の形式は "MS\_*xxx*" です。*xxx* は、ロケール識別子 (LCID) を示す 16 進数形式の文字列です。 無効なロケールを指定した場合は、メソッドから `WBEM_E_INVALID_PARAMETER` が返されます。ただし、Windows 7 の場合は、 サーバーの既定のロケールが代わりに使用されます。 "null" の場合は、現在のロケールが使用されます。

`lSecurityFlags`\
[in] `ConnectServerWmi` メソッドに渡すフラグ。 このパラメーターの値がゼロ (0) の場合は、サーバーへの接続が確立された後でのみ、`ConnectServerWmi` の呼び出しから戻ります。 これにより、サーバーが破損している場合、アプリケーションが無期限に応答しなくなる可能性があります。 他の有効な値は次のとおりです。

| 定数  | [値]  | 説明  |
|---------|---------|---------|
| `CONNECT_REPOSITORY_ONLY` | 0x40 | 内部使用のために予約されています。 使用しないでください。 |
| `WBEM_FLAG_CONNECT_USE_MAX_WAIT` | 0x80 | `ConnectServerWmi` からは、2 分以下で戻ります。 |

`strAuthority`\
[in] ユーザーのドメイン名。 次のいずれかの値になります。

| [値] | 説明 |
|---------|---------|
| blank | NTLM 認証が使用され、現在のユーザーの NTLM ドメインが使用されます。 `strUser` でドメインを指定する場合は (推奨される場所)、ここで指定しないでください。 両方のパラメーターでドメインを指定した場合、関数から `WBEM_E_INVALID_PARAMETER` が返されます。 |
| Kerberos:<*プリンシパル名*> | Kerberos 認証が使用され、このパラメーターには Kerberos プリンシパル名を格納します。 |
| NTLMDOMAIN:<*ドメイン名*> | NT LAN Manager 認証が使用され、このパラメーターには NTLM ドメイン名を格納します。 |

`pCtx`\
[in] 通常、このパラメーターは `null` です。 それ以外の場合は、1 つまたは複数の動的クラス プロバイダーが必要とする [IWbemContext](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext) オブジェクトへのポインターです。

`ppNamespace`\
[out] 関数から戻るときに、指定した名前空間にバインドされている [IWbemServices](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemservices) オブジェクトへのポインターを受け取ります。 エラーが発生したときは、`null` を指すように設定されます。

`impLevel`\
[in] 偽装レベル。

`authLevel`\
[in] 承認レベル。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_E_FAILED` | 0x80041001 | 一般エラーが発生しました。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが無効です。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | メモリ不足のため、操作を完了できません。 |
| `WBEM_S_NO_ERROR` | 0 | 関数呼び出しに成功しました。  |

## <a name="remarks"></a>解説

この関数では、[IWbemLocator::ConnectServer](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemlocator-connectserver) メソッドの呼び出しがラップされます。

既定の名前空間へのローカル アクセスの場合、`strNetworkResource` には単純なオブジェクト パス ("root\default" または "\\.\root\default") を指定できます。 COM または Microsoft 互換ネットワークを使用してリモート コンピューター上の既定の名前空間にアクセスする場合は、コンピューター名 "\\myserver\root\default" を含めます。 コンピューター名は、DNS 名または IP アドレスでもかまいません。 `ConnectServerWmi` 関数は、IPv6 アドレスを使用して IPv6 を実行しているコンピューターに接続することもできます。

`strUser` を空の文字列にすることはできません。 `strAuthority` でドメインを指定する場合、`strUser` にも含めることはできません。そうしないと、関数から `WBEM_E_INVALID_PARAMETER` が返されます。

## <a name="requirements"></a>必要条件

 **:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

 **ヘッダー:** WMINet_Utils.idl

 **.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
