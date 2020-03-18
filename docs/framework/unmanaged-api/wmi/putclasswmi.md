---
title: PutClassWmi 関数 (アンマネージ API リファレンス)
description: PutClassWmi 関数は、新しいクラスを作成するか、既存のクラスを更新します。
ms.date: 11/06/2017
api_name:
- PutClassWmi
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- PutClassWmi
helpviewer_keywords:
- PutClassWmi function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 95a5e1f6339bde9dfe5c5ad9f989fc06e10fa7f8
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73101788"
---
# <a name="putclasswmi-function"></a>PutClassWmi 関数

新しいクラスが作成されるか、既存のクラスが更新されます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT PutClassWmi (
   [in] IWbemClassObject*    pObject,
   [in] long                 lFlags,
   [in] IWbemContext*        pCtx,
   [out] IWbemCallResult**   ppCallResult
);
```

## <a name="parameters"></a>パラメーター

`pObject`\
から有効なクラス定義へのポインター。 この値は、必要なすべてのプロパティ値で正しく初期化されている必要があります。

`lFlags`\
からこの関数の動作に影響を与えるフラグの組み合わせ。 次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_FLAG_USE_AMENDED_QUALIFIERS` | 0x20000 | 設定した場合、WMI は、修正されたフレーバーを持つ修飾子を格納しません。 <br> 設定されていない場合は、このオブジェクトがローカライズされていないと見なされ、すべての修飾子がこのインスタンスと共に格納されます。 |
| `WBEM_FLAG_CREATE_OR_UPDATE` | 0 | クラスが存在しない場合は作成し、既に存在する場合は上書きします。 |
| `WBEM_FLAG_UPDATE_ONLY` | 1 | クラスを更新します。 呼び出しを成功させるには、クラスが存在している必要があります。 |
| `WBEM_FLAG_CREATE_ONLY` | 2 | クラスを作成します。 クラスが既に存在する場合、呼び出しは失敗します。 |
| `WBEM_FLAG_RETURN_IMMEDIATELY` | 0x10 | このフラグにより、半同期呼び出しが発生します。 |
| `WBEM_FLAG_OWNER_UPDATE` | 0x10000 | プッシュプロバイダーは、このクラスが変更されたことを示すために `PutClassWmi` を呼び出すときにこのフラグを指定する必要があります。 |
| `WBEM_FLAG_UPDATE_COMPATIBLE` | 0 | 派生クラスとそのクラスのインスタンスが存在しない場合に、クラスを更新できるようにします。 また、説明の修飾子などの重要でない修飾子だけが変更された場合にも、すべての更新が可能になります。 クラスにインスタンスがある場合、または重要な修飾子が変更された場合、更新は失敗します。 |
| `WBEM_FLAG_UPDATE_SAFE_MODE` | 0x20 | 変更によって子クラスとの競合が発生しない限り、子クラスがある場合でも、クラスを更新できます。 たとえば、このフラグを使用すると、前に子クラスには記載されていなかった基本クラスに新しいプロパティを追加できます。 クラスにインスタンスがある場合、更新は失敗します。 |
| `WBEM_FLAG_UPDATE_FORCE_MODE` | 0x40 | 競合する子クラスが存在する場合に、クラスを強制的に更新します。 たとえば、このフラグは、子クラスでクラス修飾子が定義されている場合に更新を強制します。基底クラスは、既存の修飾子と競合する同じ修飾子を追加しようとします。 Force モードでは、子クラスの競合している修飾子を削除することによって、このような競合が解決されます。 |

`pCtx`\
から通常、この値は `null`です。 それ以外の場合は、要求されたクラスを提供しているプロバイダーが使用できる[IWbemContext](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext)インスタンスへのポインターです。

`ppCallResult`\
入出力`null`した場合、このパラメーターは使用されません。 `lFlags` に `WBEM_FLAG_RETURN_IMMEDIATELY`が含まれている場合、関数はすぐに `WBEM_S_NO_ERROR`を返します。 `ppCallResult` パラメーターは、新しい[IWbemCallResult](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcallresult)オブジェクトへのポインターを受け取ります。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、 *WbemCli*ヘッダーファイルで定義されています。また、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_E_ACCESS_DENIED` | 0x80041003 | ユーザーには、クラスを作成または変更するためのアクセス許可がありません。 |
| `WBEM_E_FAILED` | 0x80041001 | 特定できないエラーが発生しました。 |
| `WBEM_E_INVALID_CLASS` | 0x80041010 | 指定されたクラスは無効です。 通常、これは `pObject` がインスタンスオブジェクトを指定していることを示します。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが有効ではありません。 |
| `WBEM_E_INVALID OPERATION` | 0x80041016 | 指定されたクラス名が無効です。 |
| `WBEM_E_CLASS_HAS_CHILDREN` | 0x8004-5 | サブクラスを無効にする変更を加えようとしました。 |
| `WBEM_E_ALREADY_EXISTS` | 0x80041019 | `WBEM_FLAG_CREATE_ONLY` フラグが指定されましたが、このクラスは既に存在します。 |
| `WBEM_E_NOT_FOUND` | 0x80041002 | `lFlags`に `WBEM_FLAG_UPDATE_ONLY` が指定されましたが、クラスが見つかりませんでした。 |
| `WBEM_E_INCOMPLETE_CLASS` | 0x80040 | クラスの必須プロパティがすべて設定されていません。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | 操作を完了するために必要なメモリが不足しています。 |
| `WBEM_E_SHUTTING_DOWN` | 0x80041033 | WMI が停止し、再起動されたことがあります。 [Connectserverwmi](connectserverwmi.md)を再度呼び出します。 |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | 現在のプロセスと WMI の間のリモートプロシージャコール (RPC) リンクが失敗しました。 |
| `WBEM_S_NO_ERROR` | 0 | 関数の呼び出しに成功しました。  |

## <a name="remarks"></a>Remarks

この関数は、 [IWbemServices::P utClass](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemservices-putclass)メソッドの呼び出しをラップします。

ユーザーは、名前の先頭または末尾にアンダースコア文字を持つクラスを作成することはできません。

関数呼び出しが失敗した場合は、 [GetErrorInfo](geterrorinfo.md)関数を呼び出して追加のエラー情報を取得できます。

## <a name="requirements"></a>［要件］

**:** 「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンスカウンター (アンマネージ API リファレンス)](index.md)
