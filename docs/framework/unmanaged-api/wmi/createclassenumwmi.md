---
title: CreateClassEnumWmi 関数 (アンマネージ API リファレンス)
description: CreateClassEnumWmi 関数は、指定された条件を満たすすべてのクラスの列挙子を返します。
ms.date: 11/06/2017
api_name:
- CreateClassEnumWmi
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- CreateClassEnumWmi
helpviewer_keywords:
- CreateClassEnumWmi function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 1d637479bd140e635ee647a1e30d03343d8b0dcd
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73107533"
---
# <a name="createclassenumwmi-function"></a>CreateClassEnumWmi 関数
指定した選択条件を満たしたすべてのクラスに対する列挙子が返されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT CreateClassEnumWmi (
   [in] BSTR                    strSuperclass,
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

`strSuperclass`\
から`null` ない場合、または空白の場合は、親クラスの名前を指定します。列挙子は、このクラスのサブクラスのみを返します。 `null` または空白で `lFlags` が WBEM_FLAG_SHALLOW の場合、はトップレベルのクラス (親クラスを持たないクラス) のみを返します。 `null` または空白で `lFlags` が `WBEM_FLAG_DEEP`場合、は名前空間のすべてのクラスを返します。

`lFlags`\
からこの関数の動作に影響を与えるフラグの組み合わせ。 次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_FLAG_USE_AMENDED_QUALIFIERS` | 0x20000 | 設定すると、関数は、現在の接続のロケールのローカライズされた名前空間に格納されている修正された修飾子を取得します。 <br/> 設定されていない場合、関数は、イミディエイト名前空間に格納されている修飾子だけを取得します。 |
| `WBEM_FLAG_DEEP` | 0 | 列挙には階層内のすべてのサブクラスが含まれますが、このクラスは含まれません。 |
| `WBEM_FLAG_SHALLOW` | 1 | 列挙体には、このクラスの純粋なインスタンスだけが含まれ、このクラスで見つからないプロパティを指定するサブクラスのすべてのインスタンスは除外されます。 |
| `WBEM_FLAG_RETURN_IMMEDIATELY` | 0x10 | このフラグにより、半同期呼び出しが発生します。 |
| `WBEM_FLAG_FORWARD_ONLY` | 0x20 | 関数は、順方向専用の列挙子を返します。 通常、順方向専用の列挙子は、従来の列挙子よりも高速で使用されるメモリが少なくなりますが、[複製](clone.md)の呼び出しは許可されません。 |
| `WBEM_FLAG_BIDIRECTIONAL` | 0 | WMI は、列挙体が解放されるまで、そのオブジェクトへのポインターを保持します。 |

最適なパフォーマンスを得るために、推奨されるフラグは `WBEM_FLAG_RETURN_IMMEDIATELY` と `WBEM_FLAG_FORWARD_ONLY` です。

`pCtx`\
から通常、この値は `null`です。 それ以外の場合は、要求されたクラスを提供しているプロバイダーが使用できる[IWbemContext](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext)インスタンスへのポインターです。

`ppEnum`\
入出力列挙子へのポインターを受け取ります。

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
| `WBEM_E_INVALID_CLASS` | 0x80041010 | `strSuperClass` は存在しません。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが有効ではありません。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 操作を完了するために必要なメモリが不足しています。 |
| `WBEM_E_SHUTTING_DOWN` | 0x80041033 | WMI が停止し、再起動されたことがあります。 [Connectserverwmi](connectserverwmi.md)を再度呼び出します。 |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | 現在のプロセスと WMI の間のリモートプロシージャコール (RPC) リンクが失敗しました。 |
|`WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。  |

## <a name="remarks"></a>Remarks

この関数は、 [IWbemServices:: CreateClassEnum](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemservices-createclassenum)メソッドへの呼び出しをラップします。

関数呼び出しが失敗した場合は、 [GetErrorInfo](geterrorinfo.md)関数を呼び出して追加のエラー情報を取得できます。

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
