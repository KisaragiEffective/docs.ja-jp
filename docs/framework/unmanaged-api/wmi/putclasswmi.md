---
title: PutClassWmi 関数 (アンマネージド API リファレンス)
description: PutClassWmi 関数を使用すると、新しいクラスが作成されるか、または既存のクラスが更新されます。
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
ms.translationtype: HT
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
[in] 有効なクラス定義へのポインター。 必要なすべてのプロパティ値で、正しく初期化されている必要があります。

`lFlags`\
[in] この関数の動作に影響を与えるフラグの組み合わせ。 次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_FLAG_USE_AMENDED_QUALIFIERS` | 0x20000 | 設定した場合、修正されたフレーバーを持つ修飾子は、WMI によって格納されません。 <br> 設定しない場合、このオブジェクトはローカライズされていないものと見なされ、すべての修飾子がこのインスタンスと共に格納されます。 |
| `WBEM_FLAG_CREATE_OR_UPDATE` | 0 | 存在しない場合はクラスを作成し、既に存在する場合は上書きします。 |
| `WBEM_FLAG_UPDATE_ONLY` | 1 | クラスを更新します。 呼び出しが成功するには、クラスが存在している必要があります。 |
| `WBEM_FLAG_CREATE_ONLY` | 2 | クラスを作成します。 クラスが既に存在する場合、呼び出しは失敗します。 |
| `WBEM_FLAG_RETURN_IMMEDIATELY` | 0x10 | このフラグにより、半同期呼び出しが発生します。 |
| `WBEM_FLAG_OWNER_UPDATE` | 0x10000 | このクラスが変更されたことを示すには、`PutClassWmi` を呼び出すときに、プッシュ プロバイダーでこのフラグを指定する必要があります。 |
| `WBEM_FLAG_UPDATE_COMPATIBLE` | 0 | そのクラスの派生クラスもインスタンスも存在しない場合に、クラスを更新できるようにします。 また、Description 修飾子などの重要でない修飾子だけを変更する場合も、すべての更新を許可します。 クラスにインスタンスがある場合、または重要な修飾子を変更する場合、更新は失敗します。 |
| `WBEM_FLAG_UPDATE_SAFE_MODE` | 0x20 | 子クラスが存在しても、変更によって子クラスで競合が発生しない場合に限り、クラスの更新を許可します。 たとえば、このフラグを使用すると、それまでにどの子クラスでも言及されていない基底クラスに新しいプロパティを追加できます。 クラスにインスタンスがある場合、更新は失敗します。 |
| `WBEM_FLAG_UPDATE_FORCE_MODE` | 0x40 | 競合する子クラスが存在しても、強制的にクラスを更新します。 たとえば、子クラスでクラス修飾子が定義されていて、基底クラスで既存の修飾子と競合する同じ修飾子を追加しようとする場合、このフラグを指定すると更新が強制されます。 強制モードでは、子クラスの競合している修飾子を削除することによって、この競合が解決されます。 |

`pCtx`\
[in] 通常、この値は `null` です。 それ以外の場合は、要求されたクラスを提供しているプロバイダーが使用できる [IWbemContext](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext) インスタンスへのポインターです。

`ppCallResult`\
[out] `null` の場合、このパラメーターは使用されていません。 `lFlags` に `WBEM_FLAG_RETURN_IMMEDIATELY` が含まれている場合、`WBEM_S_NO_ERROR` が発生すると関数はすぐに戻ります。 `ppCallResult` パラメーターは、新しい [IWbemCallResult](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcallresult) オブジェクトへのポインターを受け取ります。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_E_ACCESS_DENIED` | 0x80041003 | ユーザーは、クラスを作成または変更するアクセス許可を持っていません。 |
| `WBEM_E_FAILED` | 0x80041001 | 特定できないエラーが発生しました。 |
| `WBEM_E_INVALID_CLASS` | 0x80041010 | 指定したクラスが有効ではありません。 通常、これは、`pObject` によってインスタンス オブジェクトが指定されていることを示します。 |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが無効です。 |
| `WBEM_E_INVALID OPERATION` | 0x80041016 | 指定したクラス名が有効ではありません。 |
| `WBEM_E_CLASS_HAS_CHILDREN` | 0x80041025 | サブクラスを無効にする変更を行おうとしました。 |
| `WBEM_E_ALREADY_EXISTS` | 0x80041019 | `WBEM_FLAG_CREATE_ONLY` フラグを指定しましたが、そのクラスは既に存在します。 |
| `WBEM_E_NOT_FOUND` | 0x80041002 | `lFlags` で `WBEM_FLAG_UPDATE_ONLY` を指定しましたが、そのクラスが見つかりませんでした。 |
| `WBEM_E_INCOMPLETE_CLASS` | 0x80041020 | クラスの必須プロパティがすべて設定されていません。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | メモリ不足のため、操作を完了できません。 |
| `WBEM_E_SHUTTING_DOWN` | 0x80041033 | WMI が停止し、再起動されている可能性があります。 [ConnectServerWmi](connectserverwmi.md) をもう一度呼び出してください。 |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | 現在のプロセスと WMI の間のリモート プロシージャ コール (RPC) リンクが失敗しました。 |
| `WBEM_S_NO_ERROR` | 0 | 関数呼び出しに成功しました。  |

## <a name="remarks"></a>解説

この関数では、[IWbemServices::PutClass](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemservices-putclass) メソッドの呼び出しがラップされます。

ユーザーは、名前の先頭または末尾がアンダースコア文字のクラスを作成することはできません。

関数呼び出しが失敗した場合は、[GetErrorInfo](geterrorinfo.md) 関数を呼び出して追加のエラー情報を取得できます。

## <a name="requirements"></a>必要条件

**:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils.idl

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
