---
title: PutInstanceWmi 関数 (アンマネージド API リファレンス)
description: PutInstanceWmi 関数を使用すると、既存のクラスのインスタンスが作成または更新されます。
ms.date: 11/06/2017
api_name:
- PutInstanceWmi
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- PutInstanceWmi
helpviewer_keywords:
- PutInstanceWmi function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: b33f31d69c64ce520580c29f1014c058c78d0953
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73127352"
---
# <a name="putinstancewmi-function"></a>PutInstanceWmi 関数

既存のクラスのインスタンスが作成または更新されます。 インスタンスは、WMI リポジトリに書き込まれます。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>構文

```cpp
HRESULT PutInstanceWmi (
   [in] IWbemClassObject*    pInst,
   [in] long                 lFlags,
   [in] IWbemContext*        pCtx,
   [out] IWbemCallResult**   ppCallResult
);
```

## <a name="parameters"></a>パラメーター

`pInst`\
[in] 書き込まれるインスタンスへのポインター。

`lFlags`\
[in] この関数の動作に影響を与えるフラグの組み合わせ。 次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_FLAG_USE_AMENDED_QUALIFIERS` | 0x20000 | 設定した場合、**修正された** フレーバーを持つ修飾子は、WMI によって格納されません。 <br> 設定しない場合、このオブジェクトはローカライズされていないものと見なされ、すべての修飾子がこのインスタンスと共に格納されます。 |
| `WBEM_FLAG_CREATE_OR_UPDATE` | 0 | 存在しない場合はインスタンスを作成し、既に存在する場合は上書きします。 |
| `WBEM_FLAG_UPDATE_ONLY` | 1 | インスタンスを更新します。 呼び出しが成功するには、インスタンスが存在している必要があります。 |
| `WBEM_FLAG_CREATE_ONLY` | 2 | インスタンスを作成します。 インスタンスが既に存在する場合、呼び出しは失敗します。 |
| `WBEM_FLAG_RETURN_IMMEDIATELY` | 0x10 | このフラグにより、半同期呼び出しが発生します。 |

`pCtx`\
[in] 通常、この値は `null` です。 それ以外の場合は、要求されたクラスを提供しているプロバイダーが使用できる [IWbemContext](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcontext) インスタンスへのポインターです。

`ppCallResult`\
[out] `null` の場合、このパラメーターは使用されていません。 `lFlags` に `WBEM_FLAG_RETURN_IMMEDIATELY` が含まれている場合、`WBEM_S_NO_ERROR` が発生すると関数はすぐに戻ります。 `ppCallResult` パラメーターは、新しい [IWbemCallResult](/windows/desktop/api/wbemcli/nn-wbemcli-iwbemcallresult) オブジェクトへのポインターを受け取ります。

## <a name="return-value"></a>戻り値

この関数によって返される次の値は、*WbemCli.h* ヘッダー ファイル内で定義されています。または、コード内で定数として定義することもできます。

|定数  |[値]  |説明  |
|---------|---------|---------|
| `WBEM_E_ACCESS_DENIED` | 0x80041003 | 指定されたクラスのインスタンスを更新するためのアクセス許可がユーザーにありません。 |
| `WBEM_E_FAILED` | 0x80041001 | 特定できないエラーが発生しました。 |
| `WBEM_E_INVALID_CLASS` | 0x80041010 | このインスタンスをサポートしているクラスが無効です。 |
| `WBEM_E_ILLEGAL_NULL` | 0x80041028 | **Indexed** または **Not_Null** 修飾子でマークされているものなど、`null` にすることができないプロパティに `null` が指定されました。 |
| `WBEM_E_INVALID_OBJECT` | 0x8004100f | 指定したインスタンスが有効ではありません。 (たとえば、クラスで `PutInstanceWmi` を呼び出すと、この値が返されます。) |
| `WBEM_E_INVALID_PARAMETER` | 0x80041008 | パラメーターが無効です。 |
| `WBEM_E_ALREADY_EXISTS` | 0x80041019 | `WBEM_FLAG_CREATE_ONLY` フラグが指定されましたが、そのインスタンスは既に存在します。 |
| `WBEM_E_NOT_FOUND` | 0x80041002 | `lFlags` で `WBEM_FLAG_UPDATE_ONLY` が指定されましたが、そのインスタンスは存在しません。 |
| `WBEM_E_OUT_OF_MEMORY` | 0x80041006 | メモリ不足のため、操作を完了できません。 |
| `WBEM_E_SHUTTING_DOWN` | 0x80041033 | WMI が停止し、再起動されている可能性があります。 [ConnectServerWmi](connectserverwmi.md) をもう一度呼び出してください。 |
| `WBEM_E_TRANSPORT_FAILURE` | 0x80041015 | 現在のプロセスと WMI の間のリモート プロシージャ コール (RPC) リンクが失敗しました。 |
| `WBEM_S_NO_ERROR` | 0 | 関数呼び出しに成功しました。 |

## <a name="remarks"></a>解説

この関数では、[IWbemServices::PutInstance](/windows/desktop/api/wbemcli/nf-wbemcli-iwbemservices-putinstance) メソッドの呼び出しがラップされます。

`PutInstanceWmi` 関数では、インスタンスの作成と、既存のクラスのインスタンスの更新のみがサポートされます。  `pCtx` パラメーターの設定方法によっては、インスタンスの一部またはすべてのプロパティが更新されます。

`pInst` によってポイントされているインスタンスがサブクラスに属している場合、サブクラスの派生元であるクラスを担当するすべてのプロバイダーが Windows Management によって呼び出されます。 元の `PutInstanceWmi` 要求が成功するには、これらのプロバイダーのすべてが成功する必要があります。 階層内の最上位のクラスをサポートするプロバイダーが最初に呼び出されます。 呼び出し順序は、最上位クラスのサブクラスから続けて、`pInst` によってポイントされたインスタンスを所有するクラスのプロバイダーに Windows Management が到達するまで、上から下に進みます。
インスタンスのどの子クラスのプロバイダーも、Windows Management によって呼び出されません。

クラス階層に属しているインスタンスをアプリケーションで更新する必要がある場合は、`pInst` パラメーターで、変更するプロパティが含まれるインスタンスをポイントする必要があります。 たとえば、**ClassB** に属するターゲット インスタンスについて考えます。 **ClassB** のインスタンスは **ClassA** から派生し、**ClassA** でプロパティ **PropA** が定義されています。 アプリケーションで **ClassB** のインスタンスの **PropA** の値を変更する場合は、**ClassA** のインスタンスではなくそのインスタンスに、`pInst` を設定する必要があります。

抽象クラスのインスタンスで `PutInstanceWmi` を呼び出すことはできません。

関数呼び出しが失敗した場合は、[GetErrorInfo](geterrorinfo.md) 関数を呼び出して追加のエラー情報を取得できます。

## <a name="requirements"></a>必要条件

**:**「[システム要件](../../get-started/system-requirements.md)」を参照してください。

**ヘッダー:** WMINet_Utils.idl

**.NET Framework のバージョン:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>関連項目

- [WMI およびパフォーマンス カウンター (アンマネージド API リファレンス)](index.md)
